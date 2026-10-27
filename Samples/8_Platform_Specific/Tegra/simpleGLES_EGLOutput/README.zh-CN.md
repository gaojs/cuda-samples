# simpleGLES_EGLOutput - 简单 OpenGLES EGLOutput

## 描述

演示 CUDA 与 OpenGL ES（又称 Graphics interop）之间的数据交换。程序使用 CUDA 修改顶点位置，并使用 OpenGL ES 渲染几何图形；同时展示如何通过 EGLOutput 机制和 DRM 库直接向显示器渲染。

```
注意：在 Orin 平台上，运行示例前请执行此命令：
$ sudo modprobe nvidia-drm modeset=1
```

## 关键概念

图形互操作、Vertex Buffers、三维图形

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaMemcpy, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaDeviceSynchronize, cudaGraphicsUnregisterResource, cudaMalloc, cudaGraphicsGLRegisterBuffer

## 构建/运行所需依赖
[EGLOutput](../../../README.md#egloutput), [GLES](../../../README.md#gles)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文 [Dependencies]() 部分提到的依赖。

## 参考资料（更多详情）
