# simpleTexture3D - Simple Texture 3D

## 描述

简单示例，演示 CUDA 中 3D 纹理的用法。

## 关键概念

图形互操作, 图像处理, 3D Textures, Surface 写入

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaMemcpy, cudaFreeArray, cudaFree, cudaPitchedPtr, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaDestroyTextureObject, cudaExtent, cudaDeviceSynchronize, cudaCreateTextureObject, cudaGraphicsUnregisterResource, cudaMalloc, cudaGraphicsGLRegisterBuffer

## 构建/运行所需依赖
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）

