# Nsight Compute 使用指南：以 transpose 为例

本文以 CUDA Samples 的 `transpose` 为练习案例，介绍 Nsight Compute（命令行工具 `ncu`）的基本用法、核心指标、报告结果和分析方法。

选择 `transpose` 的原因是：同一个程序包含多个可对比的 Kernel，能够从“全局内存访问不理想”逐步过渡到共享内存、存储体冲突（bank conflict）和对角线 Block 重排。它比只分析一个 Kernel 更适合学习“提出问题—采集指标—解释瓶颈—比较优化”的完整流程。

对应源码：[transpose.cu](../Samples/6_Performance/transpose/transpose.cu)，示例目录：[transpose](../Samples/6_Performance/transpose)，中文 README：[README.zh-CN.md](../Samples/6_Performance/transpose/README.zh-CN.md)。

> Nsight Compute 主要分析单个 CUDA Kernel 的硬件性能和资源使用。它与 Nsight Systems 的分工不同：Systems 关注整个应用的 CPU/GPU 时间线、并发和等待；Compute 关注某个 Kernel 为什么快或慢。

## 1. 本案例演示什么

`transpose` 对浮点矩阵执行拷贝或转置，并对多个 Kernel 进行计时。重点 Kernel 如下：

| Kernel                     | 是否完整转置 | 适合观察的现象                                             |
| -------------------------- | -----: | --------------------------------------------------- |
| `transposeNaive`           |      是 | 一侧全局内存访问跨步，访存效率较低                                   |
| `transposeCoalesced`       |      是 | 通过 shared memory 让全局读写合并，但 tile 读取可能有 bank conflict |
| `transposeNoBankConflicts` |      是 | 使用 `TILE_DIM + 1` 填充，减少共享内存存储体冲突                    |
| `transposeDiagonal`        |      是 | 在无 bank conflict 基础上重排 Block 坐标，观察分区访问顺序影响          |
| `transposeCoarseGrained`   |      否 | 只用于拆分分析完整转置的组成部分                                    |
| `transposeFineGrained`     |      否 | 只用于拆分分析完整转置的组成部分                                    |

本指南主要分析前三个完整转置 Kernel，并把 `transposeDiagonal` 作为扩展对照。`coarse-grained` 和 `fine-grained` 不是完整转置，不能仅凭吞吐量与完整 Kernel 直接比较。它们与其他 Kernel 一样，使用 `16 × 16 = 256` 个线程组成一个 Block；区别在于每个线程在循环中只处理一个元素（因为 `TILE_DIM=16`、`BLOCK_ROWS=16`），而不是线程数量不同。

## 2. 开始前先建立基线

先直接运行示例，确认程序可执行，并记录程序自身输出的 `Time` 和 `Throughput`：

```bash
build/Samples/6_Performance/transpose/transpose
```

也可以固定输入尺寸，便于重复比较：

```bash
build/Samples/6_Performance/transpose/transpose \
  -dimX=2048 -dimY=2048
```

命令行选项以当前版本的帮助输出为准：

```bash
build/Samples/6_Performance/transpose/transpose -help
```

本案例的程序计时使用 CUDA Events，适合建立无分析工具介入的性能基线。Nsight Compute 采集会增加开销，因此不要把 `ncu` 运行时间直接当作基准性能。

## 3. Nsight Compute 的两种使用方式

### 3.1 命令行方式

`ncu` 可以启动目标程序、选择 Kernel、采集指标并输出报告。先确认工具可用：

```bash
ncu --version
```

如果命令不在 `PATH` 中，可查找安装位置：

```bash
command -v ncu
whereis ncu
```

### 3.2 图形界面方式

Linux 无桌面或 SSH 环境通常不能启动 Nsight Compute GUI。可以在服务器上用 `ncu` 生成 `.ncu-rep`，再把报告复制到 Windows，用 Windows 版 Nsight Compute 打开。

报告文件本身已经包含采集结果，不需要转换格式。Windows PowerShell 示例：

```powershell
scp hill@<服务器地址>:/home/hill/cuda-samples/transpose-naive.ncu-rep .
```

如果使用非默认 SSH 端口：

```powershell
scp -P 2222 hill@<服务器地址>:/home/hill/cuda-samples/transpose-naive.ncu-rep .
```

## 4. 第一次采集：只分析一个 Kernel

为了避免一次采集所有 Kernel 造成报告过大，第一次先只分析 `transposeNaive`：

```bash
ncu \
  --kernel-name regex:transposeNaive \
  --launch-skip 1 \
  --launch-count 1 \
  --set basic \
  -o ./transpose-naive \
  build/Samples/6_Performance/transpose/transpose \
  -dimX=2048 -dimY=2048
```

参数含义：

- `--kernel-name regex:transposeNaive`：只选择名称匹配的 Kernel；
- `--launch-skip 1`：跳过第一次匹配的启动，减少预热或首次启动影响；如果当前版本或程序没有多余启动，可去掉此参数；
- `--launch-count 1`：只采集一次匹配的 Kernel；
- `--kernel-name regex:<函数名>`：`regex:` 表示使用正则表达式匹配，后面的内容不是 Kernel 函数名的一部分；
- `--set basic`：采集基础指标集合，适合作为第一次练习；
- `-o ./transpose-naive`：将报告写入当前仓库目录，生成 `transpose-naive.ncu-rep`；
- 最后的内容是被分析程序及其参数。

如果跳过一次后没有采集到 Kernel，可改用更简单的命令：

```bash
ncu --kernel-name regex:transposeNaive \
  --launch-count 1 \
  --set basic \
  -o ./transpose-naive \
  build/Samples/6_Performance/transpose/transpose \
  -dimX=2048 -dimY=2048
```

不同 Nsight Compute 版本支持的选项可能略有差异。遇到参数问题时先查看：

```bash
ncu --help
ncu --list-sections
```

## 5. 采集三个 Kernel 进行对比

可以分别生成三个报告，避免把不同 Kernel 的结果混在一起。这里使用 `--kernel-name regex:<函数名>` 只选择名称匹配的 Kernel：

```bash
ncu --kernel-name regex:transposeNaive \
  --launch-count 1 --set basic -o ./transpose-naive \
  build/Samples/6_Performance/transpose/transpose -dimX=2048 -dimY=2048

ncu --kernel-name regex:transposeCoalesced \
  --launch-count 1 --set basic -o ./transpose-coalesced \
  build/Samples/6_Performance/transpose/transpose -dimX=2048 -dimY=2048

ncu --kernel-name regex:transposeNoBankConflicts \
  --launch-count 1 --set basic -o ./transpose-no-bank-conflicts \
  build/Samples/6_Performance/transpose/transpose -dimX=2048 -dimY=2048
```

`regex:` 是 Nsight Compute 的名称匹配器前缀，表示后面的模式按正则表达式解释。这里的 `transposeNaive` 本身没有正则特殊字符，因此匹配效果等同于按该名称查找；写成 `regex:transposeNaive` 可以明确告诉 `ncu` 使用正则匹配语法。若直接写 `transposeNaive`，则使用工具默认的名称匹配方式，不应把 `regex:` 理解为 Kernel 函数名的一部分。

报告文件在当前目录：

```text
transpose-naive.ncu-rep
transpose-coalesced.ncu-rep
transpose-no-bank-conflicts.ncu-rep
```

如果 `--kernel-name` 没有匹配到目标，应先查看程序实际输出的 Kernel 名称，或从源码和 Nsight Compute 的采集提示中确认名称。`--query-metrics` 不是用于列出 Kernel 名称的命令，它用于查询当前 Nsight Compute 支持的硬件性能指标及其名称，例如吞吐量、缓存、占用率和 Warp 指标。可以使用：

```bash
ncu --query-metrics
```

输出内容属于 Nsight Compute 工具支持的指标，不是当前应用实际执行的 Kernel 列表。`transpose` 程序输出中的 `transpose naive`、`transpose coalesced` 等是显示标签；采集时应使用源码中的实际函数名，例如 `transposeNaive`、`transposeCoalesced` 和 `transposeNoBankConflicts`。

不要在第一次学习时使用 `--set full`。完整采集可能需要多次 Kernel 重放，开销较大，也会产生大量指标。

## 6. 核心结果：先看哪些数据

Nsight Compute 报告的具体页面名称可能随版本和界面语言变化，但通常先看以下几类结果。命令行也可以先输出摘要：

```bash
ncu --import ./transpose-naive.ncu-rep --page details
```

如当前版本不接受 `--page details`，可直接导入报告：

```bash
ncu --import ./transpose-naive.ncu-rep
```

### 6.1 Kernel 摘要

先记录：

- Kernel 名称；
- 实际采集的启动次数；
- Kernel Duration；
- Grid 和 Block 规模；
- 计算能力与 GPU 型号。

`Kernel Duration` 是该次 Kernel 在 GPU 上的执行时间，和程序中 CUDA Event 的平均时间用途不同：前者用于工具采集的单次实例，后者通常是多次重复执行的平均值。

程序输出的 `Workgroup = 256` 表示每个 Block 使用 256 个线程。本示例中 Block 配置为 `16 × 16`，因此 `16 × 16 = 256`；它不是 Grid 中 Block 的数量。`Grid` 的规模由矩阵尺寸和 tile 大小决定，本次 `2048 × 2048` 矩阵、`16 × 16` tile 对应 `128 × 128` 个 Block。

### 6.2 Speed of Light 与 Roofline

重点看：

- SM Throughput：SM 计算管线的利用程度；
- Memory Throughput：内存子系统的利用程度；
- Compute/Memory Roofline：判断 Kernel 更接近计算受限还是内存受限。

如果 Memory Throughput 明显高而计算吞吐较低，通常说明访存是主要限制；如果计算吞吐接近设备能力而内存吞吐不高，可能更偏计算受限。Roofline 是判断方向，不应只凭单个百分比下结论。

### 6.3 Memory Workload Analysis

对 `transpose`，优先查看：

- Global Load/Store Efficiency；
- Global Memory Throughput；
- L1、L2 和 DRAM 相关吞吐；
- 读取/写入请求是否产生额外事务；
- 访问是否合并（coalescing）。

`transposeNaive` 的转置写入容易形成跨步访问，可能造成较低的全局内存访问效率。`transposeCoalesced` 使用 shared memory 把全局读写组织得更适合合并访问，因此通常是更有价值的对照。

### 6.4 Shared Memory 与 Bank Conflict

`transposeCoalesced` 使用：

```cpp
__shared__ float tile[TILE_DIM][TILE_DIM];
```

`transposeNoBankConflicts` 使用：

```cpp
__shared__ float tile[TILE_DIM][TILE_DIM + 1];
```

后者增加一列填充，使转置读取时的地址跨距改变，减少多个线程落入同一个存储体的情况。Nsight Compute 中应关注 shared memory 访问统计、相关 replay 或 stall 指标，以及报告中的规则检查结果。

不要把“看到一个 bank conflict 指标”简单等同为全部性能损失；要结合 Kernel Duration、共享内存吞吐和 Warp 停顿判断它是否是主要瓶颈。

### 6.5 Occupancy 与资源使用

记录：

- Achieved Occupancy；
- 理论占用率与实际占用率；
- 每线程寄存器数量；
- 每个 Block 的共享内存使用；
- 活跃 Warps 和 Block 数量。

占用率低不一定意味着性能差，占用率高也不一定意味着最优。它应与内存延迟隐藏、寄存器压力、共享内存使用和实际吞吐量一起解释。

### 6.6 Warp Stall 与执行效率

查看 Warp Stall Reason 或同类停顿分析结果，区分：

- 等待内存依赖；
- 等待同步；
- 执行管线不可用；
- 指令发射或其他依赖等待。

对 `transpose`，如果 `transposeNaive` 的内存访问导致更多等待，应结合全局内存效率和实际耗时验证；对 `transposeCoalesced` 与 `transposeNoBankConflicts`，应观察共享内存访问改进是否伴随停顿减少。

## 7. 如何把结果与源码对应起来

### 7.1 `transposeNaive`

Kernel 直接计算输入和输出索引：

```cpp
int index_in = xIndex + width * yIndex;
int index_out = yIndex + height * xIndex;
```

输入按行读取较容易合并，但转置后的写入方向使相邻线程可能写入相隔较远的位置。用 Nsight Compute 重点检查全局存储效率、内存事务和 Kernel Duration。

### 7.2 `transposeCoalesced`

Kernel 先把输入 tile 合并读入 shared memory，再交换索引从 shared memory 读出并合并写回。它解决了朴素版本的全局内存访问问题，但二维 tile 的转置读取可能造成 shared memory bank conflict。

重点比较：

```text
transposeNaive → 全局内存访问效率
transposeCoalesced → 全局访问改善，但共享内存冲突可能存在
```

### 7.3 `transposeNoBankConflicts`

它只改变 shared memory 的列数：

```cpp
TILE_DIM × TILE_DIM → TILE_DIM × (TILE_DIM + 1)
```

全局访问组织基本保持不变，主要用于验证减少 bank conflict 后是否降低 Kernel 时间。应比较三个结果：Kernel Duration、共享内存相关指标和全局内存指标。

### 7.4 `transposeDiagonal`

该 Kernel 在无 bank conflict 的基础上重排 Block 坐标：

```cpp
blockIdx_x = (blockIdx.x + blockIdx.y) % gridDim.x;
```

它用于尝试改善某些架构上的显存分区访问热点。若本机结果没有更快，不代表实现错误；需要结合不同矩阵规模、架构和时间线/指标验证分区驻留是否实际存在。

## 8. 建议的实验记录

对每个报告记录：

```text
GPU：
CUDA Toolkit：
Nsight Compute：
矩阵尺寸：
Kernel：
Grid/Block：
Kernel Duration：
SM Throughput：
Memory Throughput：
Global Load/Store Efficiency：
Shared Memory 相关指标：
Achieved Occupancy：
寄存器/共享内存使用：
主要 Warp Stall：
程序输出 Throughput：
结果校验：
```

比较时固定 GPU、矩阵尺寸、编译架构、程序参数和采集设置。程序输出的 `Throughput` 是有效搬运吞吐量；Nsight Compute 的硬件吞吐指标是另一组分析数据，不能混为同一个数值。

## 9. 常见问题

### 9.1 为什么报告采集很慢？

Nsight Compute 需要插桩并可能多次重放 Kernel 以收集不同指标。减少 `--launch-count`、使用 `--set basic`、只选择一个 Kernel，能降低第一次练习的采集成本。

### 9.2 为什么报告中的时间和程序输出不同？

程序通常用 CUDA Events 对多次执行求平均；Nsight Compute 报告的是被采集的 Kernel 实例，并且工具会改变执行环境。应使用各自的数值做对应分析，不要要求二者完全一致。

### 9.3 服务器上没有 GUI 怎么办？

在 Ubuntu SSH 环境中使用前面的 `ncu` 命令采集报告即可，无需在本节重复命令。

将生成的 `.ncu-rep` 报告复制到 Windows 后，在 Windows 版 Nsight Compute 中打开，查看 Summary、Details、Source、Memory、Occupancy 和 Warp 相关页面。具体页面名称以安装版本为准。

## 10. 推荐学习顺序

1. 直接运行 `transpose`，记录四个完整转置 Kernel 的 `Time` 和 `Throughput`。
2. 只采集 `transposeNaive`，理解 Kernel Duration、SM/Memory Throughput 和全局访问效率。
3. 只采集 `transposeCoalesced`，观察 shared memory 和全局访问的变化。
4. 采集 `transposeNoBankConflicts`，验证 `TILE_DIM + 1` 填充的作用。
5. 改变矩阵尺寸，重新比较结果稳定性。
6. 最后再分析 `transposeDiagonal`，不要预设它一定比基础实现更快。
7. 需要端到端时间线时，回到 Nsight Systems；需要单 Kernel 硬件原因时，使用 Nsight Compute。

## 11. 相关资料

- [Nsight Compute 官方文档](https://docs.nvidia.com/nsight-compute/)
- [Nsight Compute CLI 文档](https://docs.nvidia.com/nsight-compute/2025.2/NsightComputeCli/index.html)
- [Kernel Profiling Guide](https://docs.nvidia.com/nsight-compute/2025.1/ProfilingGuide/index.html)
- [transpose 源码](../Samples/6_Performance/transpose/transpose.cu)
- [transpose 中文 README](../Samples/6_Performance/transpose/README.zh-CN.md)
- [CUDA Samples 构建指南](00_CUDA示例编译指导.md)

