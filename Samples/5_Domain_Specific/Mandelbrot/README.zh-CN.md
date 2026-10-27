# Mandelbrot - Mandelbrot 集

## 描述

此示例使用 CUDA 交互式计算并显示 Mandelbrot 集或 Julia 集。它还展示如何使用“双单精度”算术，在深入图案进行高倍缩放时提高精度。此示例使用双精度。感谢 NewTek 的 Mark Granger 提交此代码示例！

## 核心概念

图形互操作、数据并行算法

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGLUnmapBufferObject, cudaGraphicsUnmapResources, cudaMemcpy, cudaFree, cudaGLMapBufferObject, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaGLUnregisterBufferObject, cudaDeviceSynchronize, cudaGLRegisterBufferObject, cudaGraphicsUnregisterResource, cudaMalloc, cudaGraphicsGLRegisterBuffer, cudaGetDeviceProperties

## 构建/运行所需依赖项
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文[依赖项]()部分提到的依赖项。

## 参考资料（更多详情）
