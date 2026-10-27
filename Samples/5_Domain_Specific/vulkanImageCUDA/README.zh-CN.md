# vulkanImageCUDA - Vulkan Image - CUDA 互操作

## 描述

此示例演示 Vulkan Image 与 CUDA 互操作。CUDA 导入 Vulkan 图像缓冲区，对其执行方框滤波，并通过 CUDA 导入的 Vulkan 信号量与 Vulkan 同步。此示例依赖 Vulkan SDK、GLFW3 库；构建此示例请参阅该示例目录中提供的“Build_instructions.txt”。

## 关键概念

图形互操作, CUDA Vulkan Interop, 数据并行算法

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaVkSemaphoreSignal, cudaWaitExternalSemaphoresAsync, cudaMemcpy, cudaVkImportSemaphore, cudaImportExternalSemaphore, cudaGetMipmappedArrayLevel, cudaSetDevice, cudaDestroySurfaceObject, cudaExtent, cudaMallocMipmappedArray, cudaCreateSurfaceObject, cudaStreamCreate, cudaSignalExternalSemaphoresAsync, cudaUpdateVkImage, cudaCreateTextureObject, cudaGetDeviceCount, cudaExternalMemoryGetMappedMipmappedArray, cudaDestroyTextureObject, cudaVkImportImageMem, cudaDestroyExternalMemory, cudaVkSemaphoreWait, cudaImportExternalMemory, cudaDestroyExternalSemaphore, cudaFreeMipmappedArray, cudaFree, cudaMalloc, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[X11](../../../README.md#x11), [VULKAN](../../../README.md#vulkan)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）