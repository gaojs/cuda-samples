# simpleD3D11 - 简单 D3D11

## 描述

演示如何使用 CUDA D3D11 External Resource Interoperability API 从 CUDA 更新 D3D11 buffer，并通过 Keyed Mutexes 在 D3D11 与 CUDA 之间进行同步的简单程序。

## 关键概念

图形互操作, 图像处理

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Windows

## 支持的 CPU 架构

x86_64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaImportKeyedMutex, cudaExternalMemoryGetMappedBuffer, cudaStreamCreateWithFlags, cudaWaitExternalSemaphoresAsync, cudaImportExternalSemaphore, cudaFree, cudaImportVertexBuffer, cudaReleaseSync, cudaSetDevice, cudaSignalExternalSemaphoresAsync, cudaAcquireSync, cudaDestroyExternalMemory, cudaImportExternalMemory, cudaGetDeviceCount, cudaDestroyExternalSemaphore

## 构建/运行所需的依赖项
[DirectX](../../../README.md#directx)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）