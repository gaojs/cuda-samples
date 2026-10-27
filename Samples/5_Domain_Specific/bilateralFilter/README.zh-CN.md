# bilateralFilter - 双边滤波

## 描述

双边滤波器是一种边缘保留型非线性平滑滤波器，使用 CUDA 实现并通过 OpenGL 渲染。它可用于图像恢复和去噪。每个像素的权重同时考虑其邻域像素的空间距离和颜色距离。参考文献："C. Tomasi, R. Manduchi, Bilateral Filtering for Gray and Color Images, proceeding of the ICCV, 1998, http://users.soe.ucsc.edu/~manduchi/Papers/ICCV98.pdf"

## 核心概念

图形互操作、图像处理

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaRuntimeGetVersion, cudaGraphicsUnmapResources, cudaMallocPitch, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaDestroyTextureObject, cudaDeviceSynchronize, cudaCreateTextureObject, cudaMemcpyToSymbol, cudaGraphicsUnregisterResource, cudaGraphicsGLRegisterBuffer, cudaGetDeviceProperties

## 构建/运行所需依赖项
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文[依赖项]()部分提到的依赖项。

## 参考资料（更多详情）
