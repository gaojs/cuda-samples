# volumeFiltering - 使用 三维纹理 和 表面写入 的体数据滤波

## 描述

此示例演示使用 三维纹理 和 3D 表面写入 进行 3D 体数据滤波。

## 关键概念

图形互操作, 图像处理, 三维纹理, 表面写入

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaGraphicsMapResources, cudaDestroySurfaceObject, cudaExtent, cudaDeviceSynchronize, cudaCreateSurfaceObject, cudaMemcpyToSymbol, cudaPitchedPtr, cudaGraphicsResourceGetMappedPointer, cudaCreateTextureObject, cudaGraphicsUnmapResources, cudaMallocArray, cudaFreeArray, cudaDestroyTextureObject, cudaMemset, cudaGraphicsGLRegisterBuffer, cudaFree, cudaGraphicsUnregisterResource, cudaMalloc

## 构建/运行所需的依赖项
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上方[依赖项]()部分中提到的依赖项。

## 参考资料（了解更多详情）