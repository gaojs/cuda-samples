# 0. 简介


### [asyncAPI](./asyncAPI)
此示例演示如何使用 CUDA 事件进行 GPU 计时，以及实现 CPU 与 GPU 执行重叠。事件会插入 CUDA 调用流中。由于 CUDA 流调用是异步的，GPU 执行任务（包括主机与设备之间的 DMA 内存复制）时，CPU 可以同时进行计算。CPU 可以查询 CUDA 事件，以确定 GPU 是否已完成任务。

### [clock](./clock)
此示例展示如何使用 clock 函数准确测量 kernel 中线程块的性能。

### [clock_nvrtc](./clock_nvrtc)
此示例展示如何使用 libNVRTC 和 clock 函数准确测量 kernel 中线程块的性能。

### [cudaOpenMP](./cudaOpenMP)
此示例演示如何使用 OpenMP API 编写多 GPU 应用程序。

### [fp16ScalarProduct](./fp16ScalarProduct)
计算两个 FP16 数值向量的标量积。

### [matrixMul](./matrixMul)
此示例实现矩阵乘法，内容与《编程指南》第 6 章完全相同。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。为展示 GPU 上的矩阵乘法性能，该示例还使用新的 CUDA 4.0 CUBLAS 接口演示高性能矩阵乘法。

### [matrixMul_nvrtc](./matrixMul_nvrtc)
此示例实现矩阵乘法，内容与《编程指南》第 6 章完全相同。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。为展示 GPU 上的矩阵乘法性能，该示例还使用新的 CUDA 4.0 CUBLAS 接口演示高性能矩阵乘法。

### [matrixMulDrv](./matrixMulDrv)
此示例实现矩阵乘法，并使用新的 CUDA 4.0 kernel 启动 Driver API。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。CUBLAS 可提供高性能矩阵乘法。

### [matrixMulDynlinkJIT](./matrixMulDynlinkJIT)
此示例使用 CUDA Driver API 重新演示矩阵乘法，展示如何在运行时链接 CUDA driver，以及如何对 PTX 代码进行 JIT（即时）编译。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。CUBLAS 可提供高性能矩阵乘法。

### [mergeSort](./mergeSort)
此示例实现归并排序（也称 Batcher 排序），属于排序网络算法。与渐进复杂度更优的算法（如归并排序或基数排序）相比，它通常不适用于大型序列；但对于成批的短到中等长度的（键，值）数组对，可能是合适的选择。请参阅 H. W. Lang 编写的优秀教程：http://www.iti.fh-flensburg.de/lang/algorithmen/sortieren/networks/indexen.htm

### [simpleAssert](./simpleAssert)
这是一个非常基础的 CUDA Runtime API 示例，演示如何在设备代码中使用 assert 函数。需要 Compute Capability 2.0。

### [simpleAssert_nvrtc](./simpleAssert_nvrtc)
这是一个非常基础的 CUDA Runtime API 示例，演示如何在设备代码中使用 assert 函数。需要 Compute Capability 2.0。

### [simpleAtomicIntrinsics](./simpleAtomicIntrinsics)
简单演示全局内存原子指令。

### [simpleAtomicIntrinsics_nvrtc](./simpleAtomicIntrinsics_nvrtc)
简单演示全局内存原子指令。此示例使用 NVRTC 进行运行时编译。

### [simpleAttributes](./simpleAttributes)
这是一个非常基础的 CUDA Runtime API 示例，演示如何使用影响 L2 局部性的流属性。只有在 Compute Capability 8.0 或更高版本上，才能观察到使用 L2 access policy window 带来的性能提升。

### [simpleAWBarrier](./simpleAWBarrier)
简单演示到达-等待（arrive-wait）屏障。

### [simpleCallback](./simpleCallback)
此示例使用 CUDA 5.0 引入的 CUDA 流和事件 CPU 回调，实现多线程异构计算工作负载。

### [simpleCooperativeGroups](./simpleCooperativeGroups)
此示例通过简单代码演示 cooperative groups 在线程块中的基本用法。

### [simpleCubemapTexture](./simpleCubemapTexture)
简单示例，演示如何使用 CUDA 4.1 中新增的 CUDA C cubemap 纹理支持功能。

### [simpleCUDA2GL](./simpleCUDA2GL)
此示例展示如何使用最高效的方法将 CUDA 图像复制回 OpenGL。

### [simpleDrvRuntime](./simpleDrvRuntime)
简单示例，演示 CUDA Driver API 和 Runtime API 如何协同工作，以加载向量加法 kernel 的 CUDA fatbinary 并执行向量加法。

### [simpleHyperQ](./simpleHyperQ)
此示例演示如何使用 CUDA 流，在支持 HyperQ（SM 3.5）的设备上并发执行多个 kernel。不支持 HyperQ 的设备（SM 2.0 和 SM 3.0）最多会并发运行两个 kernel。

### [simpleIPC](./simpleIPC)
这是一个基础的 CUDA Runtime API 示例，演示如何为计算任务使用每个 GPU 对应一个进程的进程间通信。需要 Compute Capability 3.0 或更高版本，以及 Linux 操作系统；Windows 系统则需要启用了 TCC 的 GPU。

### [simpleLayeredTexture](./simpleLayeredTexture)
简单示例，演示如何使用 CUDA 4.0 中新增的 CUDA C 分层纹理支持功能。

### [simpleMPI](./simpleMPI)
简单示例，演示如何将 MPI 与 CUDA 结合使用。

### [simpleMultiCopy](./simpleMultiCopy)
对于 Compute Capability 1.1 的 GPU，可以从主机系统执行计算并与一次内存复制重叠。对于 Compute Capability 2.0 的 Quadro 和 Tesla GPU，还可以以全速进行第二次重叠复制，方向不限（PCI-e 是对称的）。此示例演示如何使用 CUDA 流，使 kernel 执行与往返设备的数据复制重叠。

### [simpleMultiGPU](./simpleMultiGPU)
此应用演示如何使用新的 CUDA 4.0 API 管理 CUDA context，并通过多线程访问在多个 GPU 上运行 CUDA kernel。

### [simpleOccupancy](./simpleOccupancy)
此示例演示 CUDA occupancy calculator 和基于 occupancy 的启动配置器 API 的基本用法：使用启动配置器启动 kernel，并测量其利用率与手动配置启动之间的差异。

### [simpleP2P](./simpleP2P)
此应用演示支持多个 GPU 之间点对点（P2P）复制、点对点寻址和统一虚拟内存寻址（UVA）的 CUDA API。通常，同型号 GPU 之间支持 P2P，但某些 Tesla 和 Quadro GPU 等情况除外。

### [simplePitchLinearTexture](./simplePitchLinearTexture)
演示 Pitch Linear 纹理的用法。

### [simplePrintf](./simplePrintf)
此基础 CUDA Runtime API 示例演示如何在设备代码中使用 printf 函数。

### [simpleStreams](./simpleStreams)
此示例使用 CUDA 流，使 kernel 执行与主机和 GPU 设备之间的内存复制重叠。它使用 CUDA 4.0 中新增的通用主机内存固定功能。需要 Compute Capability 2.0 或更高版本。

### [simpleSurfaceWrite](./simpleSurfaceWrite)
简单示例，演示 2D surface 引用的用法（写入纹理）。

### [simpleTemplates](./simpleTemplates)
此示例是 template 项目的模板化版本，同时展示如何正确模板化动态分配的共享内存数组。

### [simpleTexture](./simpleTexture)
简单示例，演示 CUDA 中纹理的用法。

### [simpleTexture3D](./simpleTexture3D)
简单示例，演示 CUDA 中 3D 纹理的用法。

### [simpleTextureDrv](./simpleTextureDrv)
简单示例，演示 CUDA 中纹理的用法。此示例使用新的 CUDA 4.0 kernel 启动 Driver API。

### [simpleVoteIntrinsics](./simpleVoteIntrinsics)
简单程序，演示如何在 CUDA kernel 中使用 Vote（__any_sync、__all_sync）内建指令。

### [simpleZeroCopy](./simpleZeroCopy)
此示例演示如何使用零拷贝内存：kernel 可以直接读写固定的系统内存。

### [systemWideAtomics](./systemWideAtomics)
简单演示系统范围原子指令。

### [template](./template)
一个简单的模板项目，可作为创建新 CUDA 项目的起点。

### [UnifiedMemoryStreams](./UnifiedMemoryStreams)
此示例演示如何在单个 GPU 上结合使用 OpenMP、流和统一内存。

### [vectorAdd](./vectorAdd)
这是一个非常基础的 CUDA Runtime API 示例，实现逐元素向量加法。它与《编程指南》第 3 章中的示例相同，并增加了错误检查等内容。

### [vectorAdd_nvrtc](./vectorAdd_nvrtc)
此 CUDA Driver API 示例使用 NVRTC 在运行时编译向量加法 kernel。所演示的向量加法 kernel 与《编程指南》第 3 章中的示例相同。

### [vectorAddDrv](./vectorAddDrv)
此向量加法示例是一个基础示例，按元素实现向量加法。它与《编程指南》第 3 章中的示例相同，并增加了错误检查等内容。此外，此示例还使用新的 CUDA 4.0 kernel 启动 Driver API。

### [vectorAddMMAP](./vectorAddMMAP)
此示例将 vectorAddDrv 示例中的设备内存分配替换为通过 cuMemMap 分配的内存。它展示 cuMemMap API 允许用户指定内存的物理属性，同时保留访问的连续性，因此无需更改程序结构。
