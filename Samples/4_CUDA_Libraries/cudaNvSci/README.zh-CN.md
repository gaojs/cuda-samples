# cudaNvSci - CUDA NvSciBuf/NvSciSync Interop

## 描述

此示例演示 CUDA-NvSciBuf/NvSciSync 互操作。两个 CPU 线程将 NvSciBuf 和 NvSciSync 导入 CUDA，在 ppm 图像上执行两个图像处理算法：第 1 个线程旋转图像，第 2 个线程将旋转后的图像从 rgba 转换为灰度。目前仅支持 Ubuntu 18.04

## 核心概念

CUDA NvSci Interop, 数据并行算法, 图像处理

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

x86_64、aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetUuid

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaExternalMemoryGetMappedBuffer, cudaImportExternalSemaphore, cudaDeviceGetAttribute, cudaNvSciSignal, cudaGetMipmappedArrayLevel, cudaImportNvSciRawBuf, cudaSetDevice, cudaImportNvSciImage, cudaNvSciApp, cudaDeviceId, cudaMallocHost, cudaSignalExternalSemaphoresAsync, cudaCreateTextureObject, cudaFreeHost, cudaNvSci, cudaNvSciWait, cudaGetDeviceCount, cudaMemcpyAsync, cudaStreamCreateWithFlags, cudaExternalMemoryGetMappedMipmappedArray, cudaStreamDestroy, cudaDeviceGetNvSciSyncAttributes, cudaDestroyTextureObject, cudaDestroyExternalMemory, cudaImportExternalMemory, cudaDestroyExternalSemaphore, cudaFreeMipmappedArray, cudaFree, cudaStreamSynchronize, cudaWaitExternalSemaphoresAsync, cudaImportNvSciSemaphore

## 构建/运行所需的依赖项
[NVSCI](../../../README.md#nvsci)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）

