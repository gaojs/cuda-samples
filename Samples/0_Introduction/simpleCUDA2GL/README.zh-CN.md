# simpleCUDA2GL - CUDA and OpenGL Interop of Images

## 描述

此示例展示如何使用最高效的方法将 CUDA 图像复制回 OpenGL。

## 关键概念

图形互操作, 图像处理, 性能策略

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaHostAlloc, cudaGraphicsUnmapResources, cudaMalloc, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaMemcpyToArray, cudaDeviceSynchronize, cudaProcess, cudaGraphicsUnregisterResource, cudaGraphicsSubResourceGetMappedArray, cudaGraphicsGLRegisterBuffer, cudaGraphicsGLRegisterImage

## 构建/运行所需依赖
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）

