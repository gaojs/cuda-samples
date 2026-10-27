# simpleD3D11Texture - 简单 D3D11 纹理

## 描述

演示 Direct3D11 Texture 与 CUDA 互操作的简单程序。该程序创建多个 D3D11 Texture（2D、3D 和 CubeMap），由 CUDA kernel 写入。随后 Direct3D 将结果渲染到屏幕上。需要具备 Direct3D 能力的设备。

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
cudaGraphicsUnmapResources, cudaMalloc, cudaMallocPitch, cudaGetErrorString, cudaFree, cudaGetLastError, cudaGraphicsMapResources, cudaGetDeviceCount, cudaMemset, cudaGraphicsUnregisterResource, cudaGraphicsSubResourceGetMappedArray, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[DirectX](../../../README.md#directx)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）