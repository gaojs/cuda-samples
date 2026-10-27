# cudaNvSciNvMedia - NvMedia CUDA 互操作

## 描述

此示例通过 NvSciBuf/NvSciSync API 演示 CUDA-NvMedia 互操作。请注意，此示例仅支持从 x86_64 到 aarch64 的交叉构建，不支持 aarch64 原生构建。有关此示例的详细工作流程，请参阅示例目录中的 cudaNvSciNvMedia_Readme.pdf。

## 关键概念

CUDA NvSci Interop、数据并行算法、图像处理

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, QNX

## 支持的 CPU 架构

aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetUuid

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaImportExternalSemaphore, cudaGetMipmappedArrayLevel, cudaSetDevice, cudaDestroySurfaceObject, cudaCreateSurfaceObject, cudaImportNvSciImage, cudaCreateChannelDesc, cudaMallocHost, cudaSignalExternalSemaphoresAsync, cudaFreeHost, cudaMemcpyAsync, cudaStreamCreateWithFlags, cudaExternalMemoryGetMappedMipmappedArray, cudaMallocArray, cudaFreeArray, cudaStreamDestroy, cudaDeviceGetNvSciSyncAttributes, cudaDestroyExternalMemory, cudaImportExternalMemory, cudaDestroyExternalSemaphore, cudaFreeMipmappedArray, cudaImportNvSciSync, cudaFree, cudaStreamSynchronize, cudaMalloc, cudaWaitExternalSemaphoresAsync

## 构建/运行所需依赖
[NVSCI](../../../README.md#nvsci), [NvMedia](../../../README.md#nvmedia)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文 [Dependencies]() 部分提到的依赖。

## 参考资料（更多详情）
