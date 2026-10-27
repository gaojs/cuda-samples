# bindlessTexture - 无绑定纹理

## 描述

本示例演示 CUDA 中 cudaSurfaceObject、cudaTextureObject 和 MipMap 的用法。运行此示例需要 Compute Capability 为 SM 3.0 的 GPU。

## 关键概念

图形互操作、纹理

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaGetMipmappedArrayLevel, cudaGraphicsMapResources, cudaDestroySurfaceObject, cudaExtent, cudaDeviceSynchronize, cudaCreateSurfaceObject, cudaMallocMipmappedArray, cudaPitchedPtr, cudaGraphicsResourceGetMappedPointer, cudaCreateTextureObject, cudaGraphicsUnmapResources, cudaMallocArray, cudaFreeArray, cudaArrayGetInfo, cudaGetLastError, cudaDestroyTextureObject, cudaGraphicsGLRegisterBuffer, cudaFreeMipmappedArray, cudaFree, cudaGraphicsUnregisterResource, cudaMalloc

## 构建/运行所需依赖
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 先决条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分提到的依赖项。

## 参考资料（详情）
