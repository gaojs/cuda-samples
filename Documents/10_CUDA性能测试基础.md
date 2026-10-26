# CUDA 性能测试基础：transpose

本文基于 CUDA Samples 12.8 的 [`transpose`](../Samples/6_Performance/transpose/transpose.cu) 示例，介绍如何通过对照实验观察 CUDA Kernel 的访存效率，以及如何理解 Events 计时与有效带宽。该示例是教学型性能研究，不应把单次输出视为所有 GPU 或应用的性能保证。

## 1. 示例做什么

示例对浮点方阵执行拷贝或转置，并比较 8 种 Kernel：

| Kernel                     | 目的                                            |
| -------------------------- | --------------------------------------------- |
| `copy`                     | 简单拷贝，提供基础参考                                   |
| `copySharedMem`            | 使用 shared memory 的拷贝参考                        |
| `transposeNaive`           | 朴素转置，展示不理想的全局内存访问模式                           |
| `transposeCoalesced`       | 用 shared memory 改善转置两侧的合并访问，但仍有 bank conflict |
| `transposeNoBankConflicts` | 将 shared tile 填充一列，避免 bank conflict           |
| `transposeCoarseGrained`   | 粗粒度部分转置，用于分解观察访存开销，不是完整转置结果                   |
| `transposeFineGrained`     | 细粒度部分转置，用于性能分析，不是完整转置结果                       |
| `transposeDiagonal`        | 对角线重排 Block 执行顺序，展示缓解 partition camping 的策略   |

矩阵以 `16 × 16` tile 切分（`TILE_DIM=16`、`BLOCK_ROWS=16`），每个 CUDA Block 处理一个 tile。示例默认使用设备相关尺寸；命令行覆盖尺寸时，要求方阵且边长为 16 的整数倍。

## 2. 访存模式与性能改进

矩阵按行连续存储。Warp 中相邻线程若访问相邻地址，通常更容易形成合并的全局内存事务。转置的输入和输出索引方向不同：朴素版本容易使一侧访问呈跨步模式，导致访存效率下降。

shared-memory 版本将 tile 从全局内存按合并方式读入，再在片上共享内存中交换索引方向，最后按合并方式写回。Block 内线程通过 `cg::sync(cta)` 等待 tile 数据全部写入 shared memory 后再读取。shared memory 的二维布局若使多个线程访问同一 bank，会出现 bank conflict；`transposeNoBankConflicts` 使用 `tile[TILE_DIM][TILE_DIM + 1]` 的填充布局改变访问跨距，避免该冲突。

`transposeDiagonal` 在逻辑矩阵 tile 坐标上重排 Block 调度顺序，使 Block 沿对角线分布。它用于展示某些内存分区访问热点（partition camping）可通过访问顺序改善；效果受架构和工作负载影响。

`copy` 与 `copySharedMem` 是有用的带宽参考，但它们并不是转置 Kernel 的绝对理论上限。粗粒度/细粒度部分转置在源码中明确用于分析完整转置的组成性能，不应按完整正确的转置结果来解释。

## 3. 计时方法

示例在每个 Kernel 第一次正式计时前先启动一次预热 Kernel，以减轻首次启动等开销对结果的影响。正式测量中：

1. `cudaEventRecord(start, 0)` 在默认 Stream 记录起始 Event。
2. 在同一默认 Stream 中连续启动该 Kernel `NUM_REPS=100` 次。
3. 记录 `stop` Event，并用 `cudaEventSynchronize(stop)` 等待 GPU 到达结束边界。
4. `cudaEventElapsedTime` 取得 100 次启动所在 GPU 时间线区间的毫秒数。
5. 报告平均每次 Kernel 时间：`kernelTime / NUM_REPS`。

Kernel launch 是异步提交；如果只用 CPU 时钟测量 launch 语句周围的时间而没有同步，往往只测到提交耗时。CUDA Event 记录在同一 Stream 时，可以按该 Stream 的设备时间线测量其间 GPU 工作。此示例测的是多次 Kernel 执行区间的平均值，不包含 Host 数据初始化和首次设备分配等整个程序端到端成本。

## 4. 有效吞吐量如何计算

代码按每次转置读入一份矩阵、写出一份矩阵估算数据量：

```text
有效字节数 = 2 × 矩阵字节数
每次平均时间（秒）= kernelTime（毫秒）/ NUM_REPS / 1000
有效吞吐量（GB/s）= 有效字节数 / 每次平均时间（秒）/ 2^30
```

源码打印的标签为 `GB/s`，公式除数使用 `1024^3`，因此数值按 GiB/s 计算（尽管输出标签写 GB/s）。这个数是根据假定读写字节量和测得时间计算的有效吞吐率，不是直接测得的显存物理带宽；缓存行为、事务浪费和具体实现都会影响它。拷贝 Kernel 同样沿用读+写字节数进行比较。

## 5. 结果校验与解释

Host 端生成参考转置结果；完整转置 Kernel 与参考结果比较，简单拷贝与输入比较。部分转置 Kernel 按源码设计跳过完整转置参考校验。最终所有计时结果之外还会报告测试是否通过。

性能比较时应：

- 在同一设备、相同矩阵尺寸和相同构建条件下比较不同 Kernel。
- 先确认结果校验通过，再讨论速度。
- 关注时间与有效吞吐量，而不只看一个数字；不同 GPU 架构的排名和绝对值可能不同。
- 多次运行并观察稳定性，避免将单次结果当作精确基准。
- 认识到本例矩阵尺寸必须是方阵且为 16 的倍数；输入尺寸改变也会改变访存和缓存行为。

## 6. 构建与运行

先按[CUDA 示例编译指导](00_CUDA示例编译指导.md)完成构建，从仓库根目录运行：

```bash
(cd build/Samples/6_Performance/transpose && ./transpose -help)
(cd build/Samples/6_Performance/transpose && ./transpose)
(cd build/Samples/6_Performance/transpose && ./transpose -dimX=1024 -dimY=1024)
(cd build/Samples/6_Performance/transpose && ./transpose -device=0 -dimX=2048 -dimY=2048)
```

每个命令在子 Shell 中切换到可执行文件目录，因此运行后当前终端仍留在仓库根目录。指定 `-dimX` 与 `-dimY` 时应使用相等且是 16 倍数的值；矩阵过大时可能超出可用显存。

相关文件：[transpose 源码](../Samples/6_Performance/transpose/transpose.cu)、[transpose 中文 README](../Samples/6_Performance/transpose/README.zh-CN.md)、[转置白皮书](../Samples/6_Performance/transpose/doc/MatrixTranspose.pdf)。
