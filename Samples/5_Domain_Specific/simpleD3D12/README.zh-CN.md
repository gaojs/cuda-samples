# simpleD3D12 - 简单 D3D12 CUDA 互操作

## 描述

演示 Direct3D12 与 CUDA 互操作的程序。该程序在使用 CUDA kernel 创建的 DX12 vertex buffer 中生成正弦波。DX12 和 CUDA 使用 DirectX12 Fences 进行同步。随后 Direct3D 将结果渲染到屏幕上。在 Windows10 或更高版本操作系统上需要具备 DirectX12 能力的 NVIDIA GPU。

## 关键概念

图形互操作, CUDA DX12 Interop, 图像处理

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Windows

## 支持的 CPU 架构

x86_64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaWaitExternalSemaphoresAsync, cudaExternalMemoryGetMappedBuffer, cudaImportExternalSemaphore, cudaFree, cudaSetDevice, cudaSignalExternalSemaphoresAsync, cudaGetDeviceProperties, cudaStreamSynchronize, cudaDestroyExternalMemory, cudaStreamCreate, cudaImportExternalMemory, cudaGetDeviceCount, cudaDestroyExternalSemaphore

## 构建/运行所需的依赖项
[DirectX12](../../../README.md#directx12)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）