# nbody - CUDA N 体模拟

## 描述

此示例演示如何在 CUDA 中高效地进行引力 n 体全对全模拟。本示例对应 GPU Gems 3 章节“Fast N-Body Simulation with CUDA”。在 CUDA 5.5 中，Tesla K20c 的单精度性能提升至超过 1.8TFLOP/s。所有 Kepler 和 Fermi GPU 架构上的双精度性能也有所提升。从 CUDA 4.0 开始，nBody 示例已更新，可利用新功能轻松将 n 体模拟扩展到单台 PC 中的多个 GPU。在命令行中添加 `-numbodies=<bodies>` 可设置模拟的物体数量。添加命令行选项 `-numdevices=<N>` 将使示例使用 N 个设备（如果可用）进行模拟。在此模式下，所有物体的位置和速度数据都通过“zero copy”从系统内存读取，而非从设备内存读取。对于数量较少（4 个或更少）且物体数量足够大的设备，带宽不会成为瓶颈，因此可在这些设备上实现良好的扩展性。

## 核心概念

图形互操作、数据并行算法、基于物理的模拟

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaSetDeviceFlags, cudaGraphicsResourceSetMapFlags, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaSetDevice, cudaEventSynchronize, cudaGetDeviceCount, cudaGetDeviceProperties, cudaDeviceSynchronize, cudaEventRecord, cudaGetDevice, cudaMemcpyToSymbol, cudaStreamQuery, cudaEventDestroy, cudaEventElapsedTime, cudaDeviceCanAccessPeer, cudaEventCreate

## 构建/运行所需依赖项
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文[依赖项]()部分提到的依赖项。

## 参考资料（更多详情）

[白皮书](./doc/nbody_gems3_ch31.pdf)
