# 3. CUDA 特性


### [bf16TensorCoreGemm](./bf16TensorCoreGemm)
一个 CUDA 示例，演示如何使用 CUDA 11 在 Ampere 芯片系列 Tensor Core 中引入的 Warp Matrix Multiply and Accumulate (WMMA) API 执行 __nv_bfloat16 (e8m7) GEMM 计算，以加速矩阵运算。本示例还使用 cuda pipeline 接口提供的异步复制执行从 gmem 到 shmem 的异步加载，从而提升内核性能并减少寄存器压力。

### [binaryPartitionCG](./binaryPartitionCG)
这是一个简单示例代码，用于演示二分区 Cooperative Groups 以及线程块内的规约。

### [bindlessTexture](./bindlessTexture)
本示例演示 CUDA 中 cudaSurfaceObject、cudaTextureObject 和 MipMap 的用法。运行此示例需要 Compute Capability 为 SM 3.0 的 GPU。

### [cdpAdvancedQuicksort](./cdpAdvancedQuicksort)
本示例演示使用 CUDA Dynamic Parallelism 实现的高级快速排序。此示例需要计算能力为 3.5 或更高的设备。

### [cdpBezierTessellation](./cdpBezierTessellation)
本示例演示使用 CUDA Dynamic Parallelism 实现的贝塞尔曲线细分。此示例需要计算能力为 3.5 或更高的设备。

### [cdpQuadtree](./cdpQuadtree)
本示例演示使用 CUDA Dynamic Parallelism 实现的四叉树。此示例需要计算能力为 3.5 或更高的设备。

### [cdpSimplePrint](./cdpSimplePrint)
本示例演示使用 CUDA Dynamic Parallelism 实现的简单 printf。此示例需要计算能力为 3.5 或更高的设备。

### [cdpSimpleQuicksort](./cdpSimpleQuicksort)
本示例演示使用 CUDA Dynamic Parallelism 实现的简单快速排序。此示例需要计算能力为 3.5 或更高的设备。

### [cudaCompressibleMemory](./cudaCompressibleMemory)
本示例演示使用 cuMemMap API 分配可压缩内存。

### [cudaTensorCoreGemm](./cudaTensorCoreGemm)
CUDA 示例，演示使用 CUDA 9 引入的 Warp Matrix Multiply and Accumulate (WMMA) API 执行 GEMM 计算。

本示例演示使用 CUDA WMMA API 和 Volta 芯片系列中引入的 Tensor Core 加速矩阵运算。

此外，本示例还演示新的 CUDA 函数属性 cudaFuncAttributeMaxDynamicSharedMemorySize 的用法，该属性允许应用程序预留比默认可用量更多的共享内存。

### [dmmaTensorCoreGemm](./dmmaTensorCoreGemm)
CUDA 示例，演示使用 CUDA 11 在 Ampere 芯片系列 Tensor Core 中引入的双精度 Warp Matrix Multiply and Accumulate (WMMA) API 执行双精度 GEMM 计算，以加速矩阵运算。本示例还使用 cuda pipeline 接口提供的异步复制执行从 gmem 到 shmem 的异步加载，从而提升内核性能并减少寄存器压力。此外，本示例还演示如何使用 Cooperative Groups 异步复制接口在一个组内执行从 gmem 到 shmem 的异步加载。

### [globalToShmemAsyncCopy](./globalToShmemAsyncCopy)
当计算能力为 8.0 或更高时，本示例实现使用从全局内存到共享内存异步复制数据的矩阵乘法。同时还演示用于同步的 arrive-wait barrier。

### [graphMemoryFootprint](./graphMemoryFootprint)
本示例演示图内存节点如何重用虚拟地址和物理内存。

### [graphMemoryNodes](./graphMemoryNodes)
演示如何使用 Graph API 和 Stream Capture API 在 CUDA 图中分配和释放内存。

### [immaTensorCoreGemm](./immaTensorCoreGemm)
CUDA 示例，演示使用 CUDA 10 引入的整数版 Warp Matrix Multiply and Accumulate (WMMA) API 执行整数 GEMM 计算。本示例演示使用 CUDA WMMA API 和 Volta 芯片系列中引入的 Tensor Core 加速矩阵运算。此外，还演示新的 CUDA 函数属性 cudaFuncAttributeMaxDynamicSharedMemorySize 的用法，该属性允许应用程序预留比默认可用量更多的共享内存。

### [jacobiCudaGraphs](./jacobiCudaGraphs)
演示使用 cudaGraphExecKernelNodeSetParams() 和 cudaGraphExecUpdate() 方法，通过 Jacobi 迭代法更新实例化的 CUDA 图。

### [memMapIPCDrv](./memMapIPCDrv)
这是一个非常基础的 CUDA Driver API 示例，演示通过 cuMemMap API 实现进程间通信，每个 GPU 使用一个进程进行计算。需要计算能力 3.0 或更高的设备，以及 Linux 或 Windows 操作系统。

### [newdelete](./newdelete)
本示例演示通过设备端 C++ new 和 delete 运算符进行动态全局内存分配，以及 CUDA 4.0 提供的虚函数声明。

### [ptxjit](./ptxjit)
本示例使用 Driver API 从 PTX 代码即时编译 (JIT) 一个内核。此外，本示例还演示 CUDA Runtime API 与 CUDA Driver API 调用之间的无缝互操作能力。对于 CUDA 5.5，本示例展示如何在运行时使用 cuLink* 函数通过 CUDA driver 链接 PTX 汇编代码。

### [simpleCudaGraphs](./simpleCudaGraphs)
演示使用 Graph API 和 Stream Capture API 创建、实例化并启动 CUDA Graphs。

### [StreamPriorities](./StreamPriorities)
本示例演示 stream priorities 的基本用法。

### [tf32TensorCoreGemm](./tf32TensorCoreGemm)
一个 CUDA 示例，演示使用 CUDA 11 在 Ampere 芯片系列 Tensor Core 中引入的 Warp Matrix Multiply and Accumulate (WMMA) API 执行 tf32 (e8m10) GEMM 计算，以加速矩阵运算。本示例还使用 cuda pipeline 接口提供的异步复制执行从 gmem 到 shmem 的异步加载，从而提升内核性能并减少寄存器压力。

### [warpAggregatedAtomicsCG](./warpAggregatedAtomicsCG)
本示例演示如何使用 Cooperative Groups (CG) 对单个或多个计数器执行 warp 聚合原子操作。当许多线程对一个或多个计数器执行原子加法时，这是提升性能的有效技术。

### [graphConditionalNodes](./graphConditionalNodes)
演示 CUDA 12.4 起提供的 CUDA Graph 条件节点的用法。
