# CUDA 优化学习指导（CUDA Samples 12.8 / hill 分支）

本文根据当前 `hill` 分支（CUDA Samples v12.8）实际目录结构整理。学习时以仓库中真实存在的 Sample 为准，不按旧版或新版目录路径照搬。

## 1. 总体学习路线

```text
deviceQuery / bandwidthTest
  ↓
vectorAdd / asyncAPI / simpleStreams
  ↓
matrixMul / matrixMulCUBLAS
  ↓
transpose / alignedTypes
  ↓
reduction / scan / histogram
  ↓
globalToShmemAsyncCopy / simpleCudaGraphs
  ↓
Nsight Systems / Nsight Compute
  ↓
GEMM 或自己的实际项目
```

每个 Sample 建议做四件事：

1. 编译并运行，确认结果正确；
2. 阅读 kernel、内存管理、数据传输和同步代码；
3. 测量基线，并用性能分析工具找到瓶颈；
4. 一次只修改一个因素，再比较性能和结果。

## 2. CUDA 编程典型功能路线（精简版）

以下每个主题只保留 1–2 个代表性 Sample，避免把 CUDA Samples 当成必须逐个通关的清单。必做项放在第 10 节“推荐动手任务”；本节其他示例是按需查阅，不要求全部学习。

### 2.1 设备与基础 Kernel

- `Samples/1_Utilities/deviceQuery`：了解 GPU 型号、Compute Capability 和设备限制。
- `Samples/0_Introduction/vectorAdd`：掌握线程索引、内存分配/拷贝、kernel 启动与结果校验。

### 2.2 数据传输与异步执行

- `Samples/1_Utilities/bandwidthTest`：测量 Host↔Device 带宽，判断传输是否可能成为瓶颈。
- `Samples/0_Introduction/asyncAPI`：学习异步拷贝、CUDA Events 和 CPU/GPU 工作重叠。

### 2.3 并发与任务提交

- `Samples/0_Introduction/simpleStreams`：理解 Stream 顺序和拷贝/计算重叠。
- `Samples/3_CUDA_Features/simpleCudaGraphs`：学习 Graph 捕获、实例化和重复执行，判断是否能降低短任务的 CPU 提交开销。

### 2.4 访存优化

- `Samples/6_Performance/transpose`：观察合并访存、shared memory 和 bank conflict。
- `Samples/6_Performance/alignedTypes`：了解数据对齐对访存效率的影响；作为选修对照。

### 2.5 并行算法与同步

- `Samples/2_Concepts_and_Techniques/reduction`：理解并行归约、warp/block 协作及访存效率。
- `Samples/2_Concepts_and_Techniques/histogram`：观察原子竞争与并行计数；遇到原子冲突优化时再学。

### 2.6 计算优化与库对照

- `Samples/0_Introduction/matrixMul`：教学版矩阵乘法，学习 tiling、数据复用和 shared memory。
- `Samples/4_CUDA_Libraries/matrixMulCUBLAS`：与成熟库实现对照。实际项目通常优先评估 cuBLAS/cuBLASLt，而不是从零维护通用 GEMM。

### 2.7 精度与 Tensor Core（有矩阵计算需求时）

- `Samples/3_CUDA_Features/tf32TensorCoreGemm`：观察 Tensor Core 路径及精度/吞吐权衡。
- `Samples/0_Introduction/fp16ScalarProduct`：作为低精度数据类型的入门补充。

### 2.8 进阶主题（按项目需要选学）

- 统一内存：`Samples/6_Performance/UnifiedMemoryPerf`。
- 多 GPU/P2P：`Samples/0_Introduction/simpleP2P` 或 `Samples/5_Domain_Specific/p2pBandwidthLatencyTest`。
- 线程协作与原子优化：`Samples/3_CUDA_Features/warpAggregatedAtomicsCG`。
- 图形互操作、IPC、NVRTC、PTX、动态并行等只在项目涉及相应需求时再查阅，不作为通用优化入门的必修内容。

学习原则：先建立正确性和性能基线，再用 Nsight Systems 定位系统级瓶颈、用 Nsight Compute 分析热点 Kernel；只有测量显示某类瓶颈时，才扩展到对应选修 Sample。

## 3. CUDA 优化学习路线（详细版）

### deviceQuery

目录：`Samples/1_Utilities/deviceQuery`

它枚举设备属性，可用于确认 GPU 型号、Compute Capability、SM 数、线程限制和内存属性。构建后的可执行文件位于 build 目录中的对应 Sample 输出目录；具体路径以该次 CMake 生成结果为准，可用 `find build -type f -executable` 查找。

### bandwidthTest

目录：`Samples/1_Utilities/bandwidthTest`

用于测量 Device-to-Device、Host-to-Device、Device-to-Host 带宽，并区分页锁定与 pageable Host 内存。用于判断数据搬运是否可能成为瓶颈。

## 4. 基础执行与访存模式（详细参考）

### vectorAdd、asyncAPI、simpleStreams

- `Samples/0_Introduction/vectorAdd`：最小 Runtime API kernel、grid/block/thread、Host/Device 内存和结果校验。
- `Samples/0_Introduction/asyncAPI`：CUDA Events 计时以及 CPU/GPU 工作重叠。
- `Samples/0_Introduction/simpleStreams`：通过 streams 重叠 kernel 与内存拷贝。

先区分端到端耗时、Host/Device 拷贝耗时和 kernel 耗时。小输入时，launch 和传输开销可能超过计算本身。

### matrixMul 与 matrixMulCUBLAS

- `Samples/0_Introduction/matrixMul`：教学实现，适合观察二维线程块、tiling、shared memory 和合并访存；其目标是讲解 CUDA 原理，并非通用最高性能 GEMM。
- `Samples/4_CUDA_Libraries/matrixMulCUBLAS`：对照使用 cuBLAS 的矩阵乘法。

学习手写 GEMM 的 tile、数据复用和资源权衡；实际应用优先评估 cuBLAS/cuBLASLt 等成熟库。

### transpose 与 alignedTypes

- `Samples/6_Performance/transpose`：比较矩阵转置中的访存方式，重点观察合并访问、shared memory 和 bank conflict。
- `Samples/6_Performance/alignedTypes`：比较对齐与未对齐数据布局的访问吞吐。

可以改变 tile/block 配置并用 Nsight Compute 检查 DRAM 吞吐、缓存行为和访存效率。

## 5. 并行算法模式（详细参考）

- `Samples/2_Concepts_and_Techniques/reduction`：并行求和、shared memory、warp shuffle、warp reduce 与 Cooperative Groups reduce。
- `Samples/2_Concepts_and_Techniques/reductionMultiBlockCG`：Multi-Block Cooperative Groups 单次归约；需要符合要求的 GPU 能力。
- `Samples/2_Concepts_and_Techniques/scan`：并行前缀和。
- `Samples/2_Concepts_and_Techniques/histogram`：直方图与并行计数。
- `Samples/2_Concepts_and_Techniques/radixSortThrust`：基于 Thrust 的基数排序。

重点比较访存模式、同步和原子冲突、warp divergence、工作划分以及不同输入规模下的性能。

## 6. 异步工作与 CUDA Graphs

- `Samples/3_CUDA_Features/globalToShmemAsyncCopy`：Global Memory 到 Shared Memory 的异步拷贝与同步。
- `Samples/3_CUDA_Features/simpleCudaGraphs`：CUDA Graph 创建、实例化和执行，也演示 Stream Capture。
- `Samples/6_Performance/cudaGraphsPerfScaling`：观察 Graph API 随图规模变化的性能特性。
- `Samples/2_Concepts_and_Techniques/streamOrderedAllocation`：`cudaMallocAsync` 和内存池。

重复执行许多短 kernel 时，比较普通 launch、streams 与 CUDA Graph，区分 CPU launch 开销和 GPU 执行时间。

## 7. 性能分析工具

### Nsight Systems

用于系统级时间线分析：CPU/GPU 活动、kernel launch、内存传输、stream 并发、同步等待及计算/传输重叠。

### Nsight Compute

用于单 kernel 分析：occupancy、SM/DRAM 吞吐、缓存命中、寄存器和 shared memory 使用、warp stall 原因、访存合并情况。

### CUDA Events

测 GPU 时间时应预热 kernel，使用多轮迭代并取稳健统计，区分 kernel 时间与端到端时间，确保计时 stream 的同步方式正确。性能比较应使用相同数据、编译选项和 GPU 状态。

## 8. 按瓶颈选择 Sample

| 瓶颈线索 | 优先查看的 Samples |
|---|---|
| PCIe/Host-Device 传输占比高 | `bandwidthTest`、`asyncAPI`、`simpleStreams` |
| 非合并访存或带宽利用低 | `transpose`、`alignedTypes` |
| GEMM 计算效率低 | `matrixMul`、`matrixMulCUBLAS`、`cudaTensorCoreGemm`、`tf32TensorCoreGemm` |
| 归约/前缀和效率低 | `reduction`、`reductionMultiBlockCG`、`scan` |
| 直方图/原子冲突 | `histogram`、`warpAggregatedAtomicsCG` |
| CPU launch 开销明显 | `simpleCudaGraphs`、`cudaGraphsPerfScaling` |
| 多 GPU 通信 | `simpleMultiGPU`、`simpleP2P`、`p2pBandwidthLatencyTest`（先检查所在 Sample 和平台要求） |

## 9. 面向实际项目的优化流程

```text
验证正确性
  ↓
测量端到端时间
  ↓
区分 CPU、传输、kernel 时间
  ↓
Nsight Systems 定位时间线瓶颈
  ↓
Nsight Compute 分析热点 kernel
  ↓
选择相应 Sample 学习模式
  ↓
一次修改一个因素
  ↓
重新测量并验证结果
```

每次优化都确认输出正确、性能确实改善、输入规模与实际任务一致，并记录 GPU、Toolkit、编译参数、测试规模和重复次数。不要把降低精度或改变工作量导致的更快误认为等价优化。

## 10. 推荐动手任务（按顺序完成）

以下是建议的必做主线，直接对应前面的精简路线。每项都按“先运行原版并确认正确 → 记录基线 → 做一个小改动 → 重测并解释结果”完成；不要一次同时改多个因素。

1. **确认设备与基线**：运行 `deviceQuery`，记录 GPU、Compute Capability 和驱动信息；运行 `bandwidthTest`，记录 Host↔Device 带宽。
2. **理解并测量基础 Kernel**：阅读并运行 `vectorAdd`；分别测数据分配、拷贝、kernel 和端到端耗时，改变输入规模，观察耗时变化。
3. **理解异步与并发**：运行 `asyncAPI` 和 `simpleStreams`；比较同步执行与异步拷贝/多 Stream 执行，确认是否实现真实重叠，而不只看 API 是否异步。
4. **优化访存**：运行 `transpose`；先记录原始实现性能，再调整 tile/block 或访存实现，用 Nsight Compute 检查合并访存、DRAM 吞吐和 bank conflict。
5. **学习并行归约**：运行 `reduction`；比较不同输入规模下的正确性和吞吐，观察线程组织、访存与同步对结果的影响。
6. **从教学实现到库实现**：对比 `matrixMul` 与 `matrixMulCUBLAS`；使用相同输入规模和精度记录耗时，并检查数值误差。实际应用优先考虑成熟库。
7. **判断 Graph 是否值得使用**：选取重复执行的短 kernel，比较普通 launch 与 `simpleCudaGraphs`；分别记录 CPU 提交开销和 GPU 执行时间。若工作负载没有明显提交开销，可将此项作为选做。

`alignedTypes`、`histogram`、`tf32TensorCoreGemm` 和多 GPU/P2P 等均为选修：只有当自己的项目遇到相应的数据对齐、原子冲突、矩阵精度/吞吐或多卡通信问题时再学习，不必为了完成路线而逐一运行。

每项记录至少包括：GPU/Toolkit、编译参数、输入规模、正确性检查方式、计时方法、重复次数及结果。性能测试前预热，多次测量并使用稳健统计；有性能变化时用 Nsight Systems/Compute 找证据解释。

构建命令、当前分支版本及依赖/跳过说明见 [00_CUDA示例编译指导.md](00_CUDA示例编译指导.md)。样例路径以当前 v12.8 的 `Samples/` 目录为准；构建后实际二进制位置以 CMake 输出为准。
