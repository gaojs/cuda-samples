# simpleCUFFT_2d_MGPU - SimpleCUFFT_2d_MGPU

## 描述

使用 CUFFT。在此示例中，使用 CUFFT 在多个 GPU 上将信号和滤波器转换到频域后相乘，再将信号转换回时域，以计算二维卷积。

## 核心概念

图像处理, CUFFT 库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaXtFree, cudaMemcpy, cudaFree, cudaSetDevice, cudaGetDeviceCount, cudaDeviceSynchronize, cudaMalloc, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[CUFFT](../../../README.md#cufft)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
