# simpleVulkanMMAP - Vulkan CUDA 互操作 PI 近似

## 描述

此示例通过 cuMemMap API 演示 Vulkan CUDA 互操作。CUDA 导出 buffer，Vulkan 将其导入为 vertex buffer。CUDA 调用 kernel 操作 vertices，并通过由 CUDA 导入的 vulkan semaphores 与 Vulkan 同步。此示例依赖 Vulkan SDK、GLFW3 libraries；构建此示例请参阅该示例目录中提供的“Build_instructions.txt”。

## 关键概念

cuMemMap IPC, MMAP, 图形互操作, CUDA Vulkan Interop, 数据并行算法

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemCreate, cuMemAddressReserve, cuMemGetAllocationGranularity, cuMemAddressFree, cuMemUnmap, cuMemMap, cuMemRelease, cuMemExportToShareableHandle, cuMemSetAccess

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaWaitExternalSemaphoresAsync, cudaImportExternalSemaphore, cudaDeviceGetAttribute, cudaSetDevice, cudaLaunchHostFunc, cudaMallocHost, cudaSignalExternalSemaphoresAsync, cudaFreeHost, cudaMemsetAsync, cudaMemcpyAsync, cudaGetDeviceCount, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaDestroyExternalSemaphore, cudaSignalSemaphore, cudaWaitSemaphore, cudaFree, cudaStreamSynchronize, cudaMalloc, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[X11](../../../README.md#x11), [VULKAN](../../../README.md#vulkan)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）