# 矩阵转置优化：从朴素实现到对角线重排

本文基于 CUDA Samples 12.8 的 [`transpose`](../Samples/6_Performance/transpose/transpose.cu) 示例，按源码中的执行顺序介绍矩阵转置的实现策略、各自的问题以及后一种策略如何改进前一种策略。重点是理解 GPU 全局内存的合并访问（coalesced global memory access）、共享内存（shared memory）、存储体冲突（bank conflict）和分区驻留（partition camping），而不是把某次运行的数值当作所有 GPU 的固定结论。

示例使用 `TILE_DIM=16`、`BLOCK_ROWS=16`。一个线程块处理一个 `16 × 16` tile；默认矩阵为方阵，命令行指定尺寸时，边长必须是 16 的整数倍。

## 1. 为什么矩阵转置需要优化

矩阵按行连续存储。对于一个 warp，如果相邻线程访问相邻地址，硬件通常可以用较少的全局内存事务完成访问，这称为**合并访问（coalescing）**。矩阵转置改变了行列索引：输入按行读取比较自然，但输出写入可能变成跨行、跨步访问。

跨步访问会导致：

- 一个 warp 需要更多内存事务，实际搬运的数据中有较多无效部分；
- 全局内存带宽利用率下降；
- Kernel 耗时增加，有效吞吐量降低。

优化的基本思路是：让线程以合并方式（coalesced）从全局内存读入 tile，在共享内存（shared memory）中完成索引交换，再让线程以合并方式写回转置后的 tile。

## 2. 示例中的八种策略

示例实际测试 8 个 Kernel。前两个是拷贝参考，后六个用于观察转置实现及其组成部分。

### 2.1 `copy`：简单拷贝参考

每个线程直接从全局内存读取一个或多个元素，再写回对应位置。线程访问输入和输出时都是连续的，因此这是一个简单的合并访问（coalescing）基线。

它不是转置，只用于估计在当前矩阵规模、线程块配置和 GPU 上，类似连续读写能够达到的有效带宽。它没有转置的索引交换，也没有共享内存（shared memory）和同步开销。

\*\*局限：\*\*它的工作模式比转置简单，不能作为转置的正确性结果，也不是显存物理带宽的精确上限。

### 2.2 `copySharedMem`：使用共享内存（shared memory）的拷贝参考

线程先把数据写入共享内存（shared memory）tile，经过线程块同步后，再从 tile 读出并写回全局内存。它保持拷贝方向，不改变行列索引。

这个版本用于观察共享内存（shared memory）和同步本身带来的成本。与 `copy` 相比，它增加了共享内存访问和 `cg::sync(cta)`，但全局内存访问仍然是合并访问（coalescing）。

\*\*局限：\*\*它仍然不是转置；如果仅为了拷贝数据，增加 shared memory 通常没有必要。它的价值主要是作为“使用 tile 但不交换索引”的对照组。

### 2.3 `transposeNaive`：朴素转置（写入不合并）

每个线程直接从输入矩阵读取元素，并根据转置后的行列坐标直接写入输出：

- 输入索引按原矩阵位置计算；
- 输出索引交换行列方向；
- 不使用共享内存（shared memory）。

它的读取通常比较连续，但转置后的写入容易呈跨步模式。一个 warp 的线程可能写入相距较远的地址，导致全局内存事务利用率很低。

\*\*问题：\*\*全局内存写入不合并，带宽利用率差。它实现简单，但通常不是高性能转置的合适方案。

### 2.4 `transposeCoalesced`：用共享内存（shared memory）恢复合并访问（coalescing）

该版本采用 tile 转置：

1. 线程以合并方式把输入 tile 写入 `tile[行][列]`；
2. 线程块同步，确保 tile 已经完整写入；
3. 线程交换共享内存（shared memory）的行列索引；
4. 以合并方式（coalesced）把转置后的 tile 写回输出。

这样既避免了 `transposeNaive` 的跨步全局内存写入，又保留了转置结果。

**新问题：**共享内存（shared memory）的二维数组是 `tile[16][16]`。转置读取时，一个 warp 中的线程可能访问同一 shared memory bank 的不同地址，形成**存储体冲突（bank conflict）**。虽然全局内存访问已经合并，但共享内存访问会被拆成多个请求，降低 tile 转置阶段的速度。

### 2.5 `transposeNoBankConflicts`：通过填充避免存储体冲突（bank conflict）

该版本与 `transposeCoalesced` 的主要区别是共享内存（shared memory）声明为：

```cpp
__shared__ float tile[TILE_DIM][TILE_DIM + 1];
```

每行额外增加一个元素，使下一行相对于上一行的起始地址错开一个位置。转置读取时，原本容易落到相同存储体（bank）的线程会分散到不同存储体，从而避免或显著减少存储体冲突（bank conflict）。

\*\*代价：\*\*每个 tile 多占用一列 shared memory，增加的空间通常很小；但具体收益仍取决于 GPU 架构、访问模式和其他资源限制。这个版本通常是示例中最直接的高效完整转置实现。

### 2.6 `transposeCoarseGrained`：只测转置流程的粗粒度部分

该 Kernel 使用无存储体冲突（bank conflict）的共享内存（shared memory）tile：

1. 将全局内存中的 tile 以合并方式（coalesced）读入共享内存（shared memory）；
2. 同步线程块；
3. 从共享内存读取时不交换共享内存的行列索引，直接写出。

它刻意只保留部分转置步骤，用来分解观察“全局内存读入 + 共享内存（shared memory）访问 + 全局内存写出”等组成部分的成本。

\*\*重要限制：\*\*它不是完整的矩阵转置，源码明确将它作为性能分解实验，因此会跳过完整转置结果校验。不能把它与完整转置 Kernel 的正确性或最终应用性能直接等价比较。

### 2.7 `transposeFineGrained`：只测更细粒度的转置部分

该 Kernel 也使用带填充的共享内存（shared memory），但采用另一种索引组合：

1. 将输入 tile 写入共享内存（shared memory）；
2. 同步线程块；
3. 从共享内存中按转置方向读取；
4. 使用源码指定的输出索引写出。

它用于单独观察更细粒度的共享内存（shared memory）转置访问和全局内存访问成本，与 `transposeCoarseGrained` 共同帮助分析完整转置由哪些阶段组成。

\*\*重要限制：\*\*它同样不是完整的矩阵转置，源码将其结果校验设为跳过。它的名称和实验用途是性能分析，不应作为生产转置实现。

### 2.8 `transposeDiagonal`：对角线重排，缓解分区驻留（partition camping）

该版本保留 `transposeNoBankConflicts` 的 tile 填充和合并访问（coalescing），只改变线程块处理矩阵 tile 的逻辑顺序。

对于方阵，它把原始的二维 block 坐标映射到对角线顺序：一个坐标表示当前对角线上的位置，另一个坐标表示不同对角线。这样可以改变相邻时间内访问的内存区域分布，减少多个正在执行的 block 集中访问同一内存分区的情况。

**解决的问题：**即使单个 warp 的访问已经合并，多个 block 的访问仍可能在短时间内集中到少数显存分区，形成分区驻留（partition camping）。对角线重排通过改变 block 的访问顺序缓解这种热点。

**注意：**分区驻留（partition camping）是否明显、对角线重排是否有效，取决于 GPU 架构、矩阵大小、分区映射和调度状态。在所有 GPU 上都不一定有收益；如果重排带来的额外索引计算或调度影响超过收益，性能甚至可能不变或下降。

## 3. 存储体冲突（bank conflict）是什么

共享内存（shared memory）通常划分为多个存储体（bank）。连续地址通常映射到连续存储体。一个 warp 同时访问不同存储体时可以并行完成；如果多个线程访问不同地址但这些地址映射到同一个存储体，就会发生**存储体冲突（bank conflict）**。

发生冲突时，一次共享内存请求可能被拆成多个串行请求，导致：

- 共享内存指令耗时增加；
- warp 在共享内存操作上等待；
- 全局内存访问已经优化后，共享内存反而成为新的瓶颈。

在本示例中，`tile[16][16]` 的转置访问容易产生规则性的存储体冲突（bank conflict）。`transposeNoBankConflicts` 通过把 tile 改为 `tile[16][17]`，改变每行的起始偏移，打散这种规律访问。实际项目中还可以调整数据布局、访问顺序或 tile 形状，但应先通过分析工具确认冲突确实存在。

## 4. 分区驻留（partition camping）是什么

显存并非只有一个连续的物理通道，通常由多个内存分区共同服务。地址会按硬件规则映射到不同分区。**分区驻留（partition camping）**指大量并发 block 或 warp 的访问在时间上集中到少数分区，造成局部热点，而其他分区利用不足。

它的坏处是：

- 总体显存带宽无法充分利用；
- 某些分区排队，访问延迟增加；
- 单线程或单 warp 看起来已经合并访存，但整个 Kernel 的吞吐量仍然不高。

它与存储体冲突（bank conflict）不同：

- 存储体冲突（bank conflict）发生在一个 SM 内部的共享内存访问层面；
- 分区驻留（partition camping）发生在更大范围的显存分区访问分布层面。

避免或缓解分区驻留（partition camping）的方法包括：

- 改变 block 的访问顺序，例如本示例的 `transposeDiagonal`；
- 调整 tile 尺寸、矩阵布局或访问步长，使访问分布更均匀；
- 避免让大量 block 在同一时间访问具有相同分区映射规律的地址；
- 使用 Nsight Compute 或 Nsight Systems 观察实际瓶颈，而不是仅凭理论判断。

## 5. 如何观察性能

### 5.1 首先看正确性

运行结束时应先确认 `Test passed`。`copy` 和 `copySharedMem` 与输入数据比较；完整转置 Kernel 与 Host 端生成的参考转置结果比较；`transposeCoarseGrained` 和 `transposeFineGrained` 是部分转置，源码会跳过完整结果校验。

部分转置 Kernel 即使输出不等于完整转置，也不代表程序失败；它们的用途是拆分性能组成部分。

### 5.2 主要看耗时和有效吞吐量

示例在每个 Kernel 正式计时前先预热一次，然后在默认 Stream 中连续执行 `NUM_REPS=100` 次，并使用 CUDA Events 记录 GPU 时间。输出包含：

- `Time`：平均每次 Kernel 的执行时间，单位为 ms；
- `Throughput`：按一次读入矩阵并写出矩阵估算的有效吞吐量。

源码计算方式为：

```text
平均时间（秒）= kernelTime（毫秒）/ NUM_REPS / 1000
有效字节数 = 2 × 矩阵字节数
有效吞吐量（GiB/s）= 有效字节数 / 平均时间（秒）/ 2^30
```

虽然输出标签写作 `GB/s`，但源码除以的是 `1024^3`，严格说更接近 GiB/s。比较策略时，优先在相同 GPU、矩阵尺寸、编译选项和运行状态下同时观察 `Time` 与 `Throughput`：

- 时间越短越好；
- 有效吞吐量越高越好；
- 两者本质上是同一测量的不同表达，不能把它们当成两个独立的性能指标；
- 不要仅根据不同 GPU 之间的绝对数值判断策略优劣。

建议改变矩阵尺寸重复测试，例如 512、1024、2048；每次只改变一个因素，并记录 GPU 型号、Toolkit、矩阵尺寸和结果。矩阵过大时可能超出显存容量。

## 6. 构建与运行

先按[CUDA 示例编译指导](00_CUDA示例编译指导.md)完成构建，从仓库根目录运行：

```bash
(cd build/Samples/6_Performance/transpose && ./transpose -help)
(cd build/Samples/6_Performance/transpose && ./transpose)
(cd build/Samples/6_Performance/transpose && ./transpose -dimX=1024 -dimY=1024)
(cd build/Samples/6_Performance/transpose && ./transpose -device=0 -dimX=2048 -dimY=2048)
```

`-dimX` 和 `-dimY` 必须相等，并且都应是 16 的整数倍。每个命令在子 Shell 中运行，不会改变当前终端所在目录。

相关文件：[transpose 源码](../Samples/6_Performance/transpose/transpose.cu)、[transpose 中文 README](../Samples/6_Performance/transpose/README.zh-CN.md)、[转置白皮书](../Samples/6_Performance/transpose/doc/MatrixTranspose.pdf)。
