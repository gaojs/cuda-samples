# Nsight Systems 使用指南：以 simpleStreams 为例

本文以 CUDA Samples 的 `simpleStreams` 为练习案例，介绍 Nsight Systems（`nsys`）的基本用法、时间线阅读方式和常见指标。

选择 `simpleStreams` 的原因是：它同时包含 Kernel、Device↔Host 内存拷贝、多个 CUDA Stream 和 CUDA Event 计时，适合观察一个 CUDA 程序从 CPU 提交到 GPU 执行完成的完整过程。

对应源码：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu)，示例目录：[simpleStreams](../Samples/0_Introduction/simpleStreams)。

> Nsight Systems 主要回答“整个应用何时做了什么、是否并发、哪里在等待”。它不是单个 Kernel 的详细硬件计数器分析工具；
>
> 需要分析访存效率、Warp 停顿、共享内存冲突等问题时，应使用 Nsight Compute。

## 1. 本案例演示什么

`simpleStreams` 将一个数组分成多个部分，并在多个 CUDA Stream 中重复执行以下工作：

```text
Kernel 初始化数组的一部分
        ↓
异步 Device→Host 拷贝对应部分
```

源码默认使用 4 个 Stream，并将 Kernel 与异步拷贝分别提交到各个 Stream。它还提供非 Stream 版本作为对照：

- `non-streamed`：默认 Stream 中依次执行 Kernel 和同步拷贝；
- `4 streams`：使用多个 Stream 异步提交分块 Kernel 和分块拷贝。

Kernel 内部故意使用循环增加计算量，使 Kernel 执行时间足够长，更容易在时间线上观察计算与拷贝的关系。主机内存使用页锁定内存，满足异步 Host↔Device 拷贝的条件。

源码关键位置：

- Kernel 定义：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu#L78-L84)
- Stream 创建：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu#L334-L340)
- 非 Stream 对照：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu#L375-L390)
- 多 Stream 执行：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu#L392-L424)

## 2. Nsight Systems 能回答什么问题

使用这个案例，重点观察以下问题：

1. CPU 线程何时调用 CUDA Runtime API？在 CPU Threads 时间线 + Events View 里。
2. Kernel 何时被提交，何时真正开始执行？CUDA HW Stream 时间线上 kernel 色块的左边缘。
3. Device→Host 拷贝是否与 Kernel 重叠？不同 Stream：色块错开就是重叠；色块首尾相接就是串行。
4. 4 个 Stream 是否真正并发，还是因为硬件资源或依赖关系而串行？多个 Stream 里的色块**时间区间有重叠。**
5. CPU 是否在等待 GPU，GPU 时间线上是否存在空闲间隔？CPU 线程有阻塞色块；CUDA HW 时间线有空白。
6. 多 Stream 版本是否减少了端到端重复处理时间？分别多单流和多流trace，对比总耗时和单次耗时。
7. 程序总时间主要花在 Kernel、内存传输、同步还是 API 提交？nsys stats --report cuda\_api\_gpu\_sum，会自动汇总：Kernel 总耗时、MemOps（HtoD/DtoH）总耗时、同步 API（sync 类）耗时、runtime API 提交开销。

这些问题属于应用时间线问题，正是 Nsight Systems 的主要使用场景。

## 3. 准备环境

确认程序已经构建：

```bash
(cd build/Samples/0_Introduction/simpleStreams && ./simpleStreams --help)
```

该示例需要指定 `--sync_method` 才会执行测试。例如使用阻塞同步：

```bash
(cd build/Samples/0_Introduction/simpleStreams && \
  ./simpleStreams --sync_method=4)
```

如果要对比主机内存分配方式，可以分别运行：

```bash
(cd build/Samples/0_Introduction/simpleStreams && \
  ./simpleStreams --sync_method=4 --use_generic_memory)

(cd build/Samples/0_Introduction/simpleStreams && \
  ./simpleStreams --sync_method=4 --use_cuda_malloc_host)
```

本指南只建议先固定一种内存分配方式，避免把内存分配方式的影响与 Stream 并发影响混在一起。

确认 Nsight Systems 命令行工具：

```bash
nsys --version
```

图形界面启动器使用 `nsys-ui`（例如 `/usr/local/cuda-12.8/bin/nsys-ui`）。无桌面或 SSH 环境通常没有可用的显示服务；此时用 `nsys` 生成报告，再将 `.nsys-rep` 文件复制到有图形界面的机器查看。

## 4. 第一次抓取：最小命令

在仓库根目录运行：

```bash
nsys profile \
  --trace=cuda,osrt \
  --sample=none \
  --stats=true \
  -o ./simpleStreams \
  build/Samples/0_Introduction/simpleStreams/simpleStreams \
  --sync_method=4
```

命令从仓库根目录运行，报告会生成在当前仓库目录下，不会写入系统临时目录。参数含义：

- `profile`：启动一次性能采集；
- `--trace=cuda,osrt`：采集 CUDA API/GPU 活动和操作系统运行时信息；
- `--sample=none`：不进行 CPU 周期采样，减少本案例无关数据；
- `--stats=true`：采集结束后在终端输出统计摘要；
- `-o ./simpleStreams`：指定报告前缀，通常生成 `.nsys-rep` 文件；
- 最后的内容是被分析程序及其参数。

第一次练习建议使用最小采集范围。采集时间线时，不要一开始启用所有可能的 trace 类型，否则报告会包含大量与 CUDA 并发无关的信息。

## 5. 查看报告

### 5.1 命令行查看统计

可以直接查看报告统计：

```bash
nsys stats ./simpleStreams.nsys-rep
```

只查看 CUDA API 统计：

```bash
nsys stats --report cuda_api_sum ./simpleStreams.nsys-rep
```

查看 CUDA Kernel 统计：

```bash
nsys stats --report cuda_gpu_kern_sum ./simpleStreams.nsys-rep
```

查看 GPU 内存操作统计：

```bash
nsys stats --report cuda_gpu_mem_time_sum ./simpleStreams.nsys-rep
```

不同 Nsight Systems 版本支持的 report 名称可能略有差异。如果某个 report 不存在，应先运行：

```bash
nsys stats --help
```

### 5.2 图形界面查看

在带桌面环境的机器上打开：

```bash
nsys-ui ./simpleStreams.nsys-rep
```

如果安装位置不同，可用 `command -v nsys-ui` 查找启动器路径。

GUI 中优先查看 **Timeline（时间线）** 页面。不同 Nsight Systems 版本、采集选项和目标平台的轨道名称可能不同，不要把下面名称理解为固定的菜单或面板名称。以当前报告中实际显示的轨道为准，重点寻找：

- **进程/线程轨道**：查看程序线程、CUDA API 调用以及 CPU 是否处于运行、等待或阻塞状态；
- **CUDA API 轨道**：查看 `cudaLaunchKernel`、`cudaMemcpyAsync`、同步调用的提交时间和持续时间；
- **CUDA GPU 工作轨道**：查看 Kernel 的执行区间、所属 Stream 以及不同 Stream 之间是否重叠；
- **CUDA 内存传输轨道**：查看 Host↔Device 拷贝方向、持续时间、所属 Stream 以及拷贝之间是否重叠；
- **GPU 引擎/硬件轨道**（如果当前采集启用并显示）：查看计算引擎和拷贝引擎的活动；
- **OS Runtime 轨道**（如果启用）：辅助判断 CPU 线程是否在 `poll`、`sem_wait` 等系统调用中等待。

常见界面可能将这些信息分组显示在 **Threads**、**CUDA**、**GPU** 或 **CUDA HW** 等分组下；具体名称以当前 Nsight Systems 版本和报告内容为准。拖动时间轴放大到一轮 Kernel+拷贝，先对齐同一时间范围，再比较不同 Stream 的活动是否在同一时间段重叠。

## 6. 核心数据：先看本次抓取结果

以下结果来自本目录的 `simpleStreams.nsys-rep`，使用 `nsys stats` 生成。程序参数为 `--sync_method=4`，GPU 为 NVIDIA GeForce RTX 4090 D，报告版本为 Nsight Systems 2024.6.2。

### 6.1 程序输出

```text
non-streamed: 3.29 ms
4 streams:    2.68 ms
```

在本次运行中，4 Stream 路径比 non-streamed 路径快约 **18.5%**，加速比约 **1.23 倍**。这是程序输出的对照耗时，不是 Nsight 汇总直接计算出的结果。

### 6.2 CUDA API 汇总

`cuda_api_sum` 的关键结果：

| API | 调用次数 | 累计 CPU 调用时间 |
|---|---:|---:|
| `cudaMemcpy` | 101 | 328.02 ms |
| `cudaEventSynchronize` | 4 | 269.33 ms |
| `cudaLaunchKernel` | 501 | 2.69 ms |
| `cudaMemcpyAsync` | 401 | 1.61 ms |

### 6.3 GPU Kernel 汇总

`cuda_gpu_kern_sum` 的结果：

| Kernel | 次数 | 平均 GPU 时间 | 累计 GPU 时间 |
|---|---:|---:|---:|
| `init_array(int *, int *, int)` | 501 | 36.19 μs | 18.13 ms |

### 6.4 GPU 内存操作汇总

`cuda_gpu_mem_time_sum` 和 `cuda_gpu_mem_size_sum` 的结果：

| 操作 | 次数 | 平均耗时 | 累计耗时 | 累计数据量 |
|---|---:|---:|---:|---:|
| Device→Host | 501 | 1.149 ms | 575.45 ms | 13,488.88 MB |
| Host→Device | 1 | 0.64 μs | 0.64 μs | 0 MB |
| CUDA memset | 2 | 35.26 μs | 70.53 μs | 134.22 MB |

### 6.5 如何解释这些结果

- `cudaMemcpy` 和 `cudaEventSynchronize` 的累计 CPU 时间较高，说明 CPU 在同步拷贝和等待事件上花费了较多时间。
- `cudaLaunchKernel` 只有 2.69 ms，这是 CPU 提交 Kernel 的时间，不是 Kernel 在 GPU 上的执行时间。
- `cudaMemcpyAsync` 很快返回，也不能说明 GPU 拷贝已经完成；实际拷贝应以 GPU 时间线为准。
- `init_array` 的 GPU 累计执行时间为 18.13 ms；Device→Host 拷贝累计为 575.45 ms，表明本案例主要包含大量数据回传。
- 拷贝和 Kernel 的累计时间可能因不同 Stream 的重叠而大于程序端到端时间，不能直接相加得到总运行时间。
- 当前环境无法打开 GUI，因此尚未确认具体时间线上的重叠关系；上面的统计只能说明“发生了哪些操作及其累计数据”，不能单独证明它们是否重叠。

### 6.6 时间线中继续确认什么

在有图形界面的机器打开报告后，重点核对：

- 不同 Stream 的 `init_array` Kernel 是否在同一时间段重叠；
- Device→Host 拷贝是否与其他 Stream 的 Kernel 重叠；
- `cudaEventSynchronize` 对应的 CPU 等待是否与 GPU 活动相对应；
- GPU 是否存在没有 Kernel 或拷贝的空闲区间。

如果多个 Stream 的 GPU 活动首尾相接，可能是资源不足或存在依赖；如果活动时间段交叠，才能确认实际形成了并发。

### 6.7 时间与吞吐量

Nsight Systems 主要提供时间线和持续时间。优化效果需要用同一条件下的基线比较：

```text
加速比 = 基线平均时间 / 优化后平均时间
时间下降比例 = 1 - 优化后平均时间 / 基线平均时间
```

`simpleStreams` 程序自身输出 `non-streamed` 和 `4 streams` 的平均耗时，可以先比较这两个值，再用时间线确认时间差是否来自真实的 Kernel/拷贝重叠，而不是测量误差。

## 7. 如何用本案例做一次对照实验

### 7.1 先运行普通程序

```bash
(cd build/Samples/0_Introduction/simpleStreams && \
  ./simpleStreams --sync_method=4)
```

记录：

- `memcopy`；
- `kernel`；
- `non-streamed`；
- `4 streams`；
- 最终正确性输出。

### 7.2 抓取普通程序

```bash
nsys profile --trace=cuda,osrt --sample=none --stats=true \
  -o ./simpleStreams-streamed \
  build/Samples/0_Introduction/simpleStreams/simpleStreams \
  --sync_method=4
```

### 7.3 修改为对照版本

源码中的非 Stream 路径已经存在，可以把它单独作为基线；多 Stream 路径作为优化版本。不要仅看程序终端的一个数字，应确认两次实验使用：

- 相同 GPU；
- 相同数组规模；
- 相同 Kernel 迭代次数；
- 相同同步方法；
- 相同主机内存分配方式；
- 相同预热和重复次数。

### 7.4 解释结果

如果多 Stream 版本更快，理想的时间线应显示：

```text
Stream 0：Kernel → 拷贝
Stream 1：      Kernel → 拷贝
Stream 2：             Kernel → 拷贝
Stream 3：                    Kernel → 拷贝
```

实际时间线可能因 GPU 架构和资源限制而不同。正确结论应是“哪些活动发生了部分重叠、重叠减少了多少总时间”，而不是简单断言所有 Stream 完全并行。

## 8. 常见误区

### 8.1 异步 API 不等于实际并发

`cudaMemcpyAsync` 和多 Stream 只能说明程序允许异步调度。是否并发，必须看 Nsight Systems 的 GPU 时间线。

### 8.2 CPU API 时间不等于 GPU 执行时间

Kernel launch API 很快返回时，GPU 可能仍在执行 Kernel。应区分 CPU API 区间和 GPU Kernel 区间。

### 8.3 采集运行不能直接作为性能基准

Nsight Systems 会增加采集开销，尤其是启用较多 trace 时。性能基线应使用程序自带 CUDA Event 计时和多次独立运行；Nsight 用于解释时间线和瓶颈。

### 8.4 不要用 Systems 代替 Compute

Systems 适合发现：

- 哪个阶段耗时；
- 是否存在并发；
- 是否存在同步等待；
- 数据传输和计算如何排列。

Compute 适合分析：

- 全局内存合并访问；
- L1/L2/DRAM 吞吐；
- shared memory 存储体冲突；
- Warp 停顿；
- 寄存器、共享内存和占用率。

## 9. 建议的学习步骤

1. 运行 `simpleStreams --sync_method=4`，先理解程序输出。
2. 用最小 `nsys profile` 命令抓取 CUDA 时间线。
3. 在 GUI 中定位 CUDA API、Kernel 和内存拷贝轨道。
4. 找到同一轮工作在不同 Stream 中的时间关系。
5. 用 `nsys stats` 查看 CUDA API、Kernel 和内存操作汇总。
6. 对比普通路径和多 Stream 路径的总时间与重叠情况。
7. 再用 `matrixMul` 或 `transpose` 学习 Nsight Compute 的单 Kernel 分析。

## 10. 结果记录模板

每次实验建议记录以下信息：

```text
GPU：NVIDIA GeForce RTX 4090 D（Compute Capability 8.9）
CUDA Toolkit：12.8
Nsight Systems：2024.6.2
程序参数：--sync_method=4
数组规模：16,777,216 个 int（64 MiB）
Stream 数量：4
同步方式：cudaDeviceScheduleBlockingSync（阻塞同步事件）
主机内存方式：通用页对齐内存 + cudaHostRegister 注册
non-streamed 平均时间：3.29 ms
多 Stream 平均时间：2.68 ms
观察到的 Kernel/拷贝重叠：本次未通过 GUI 时间线确认；命令行汇总不能证明具体重叠
主要等待或空闲区间：cudaEventSynchronize 累计 CPU API 时间约 269.33 ms；需在时间线定位具体等待区间
最终正确性：当前采集输出未出现最终 PASS/TEST PASSED，需单独运行程序核验
报告文件：simpleStreams.nsys-rep（252,648 字节）
SQLite 文件：simpleStreams.sqlite（1,126,400 字节）
采集统计：init_array 共 501 次，GPU 累计约 18.13 ms；D→H 拷贝 501 次，累计约 575.45 ms、约 13,488.88 MB
```

以上数值来自一次采集，仅作示例记录。Nsight 汇总中的累计 GPU 操作时间可能因多 Stream 重叠而大于程序端到端时间；CPU API 累计时间也不等同 GPU 执行时间。

这样可以把“程序输出的性能变化”和“时间线上看到的执行原因”对应起来。

## 11. 相关资料

- [Nsight Systems 官方文档](https://docs.nvidia.com/nsight-systems/)
- [CUDA Samples 构建指南](00_CUDA示例编译指导.md)
- [simpleStreams 源码](../Samples/0_Introduction/simpleStreams/simpleStreams.cu)
- [simpleStreams 中文 README](../Samples/0_Introduction/simpleStreams/README.zh-CN.md)

