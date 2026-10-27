# oceanFFT - CUDA FFT Ocean Simulation

## 描述

此示例使用 CUFFT 生成海洋高度场，并使用 OpenGL 渲染结果。

## 核心概念

图形互操作, 图像处理, CUFFT 库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaMemcpy, cudaMalloc, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaCalculateSlopeKernel, cudaGraphicsMapResources, cudaUpdateHeightmapKernel, cudaGraphicsUnregisterResource, cudaGenerateSpectrumKernel, cudaGraphicsGLRegisterBuffer, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[X11](../../../README.md#x11), [GL](../../../README.md#gl), [CUFFT](../../../README.md#cufft)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）

