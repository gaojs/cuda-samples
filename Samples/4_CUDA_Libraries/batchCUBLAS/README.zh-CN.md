# batchCUBLAS - batchCUBLAS

## 描述

一个 CUDA 示例，演示如何使用批量 CUBLAS API 调用来提升整体性能。

## 核心概念

线性代数, CUBLAS 库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuR和, cuEqual

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaGetErrorString, cudaFree, cudaGetLastError, cudaDeviceSynchronize, cudaGetDevice, cudaMalloc, cudaStreamCreate, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[CUBLAS](../../../README.md#cublas)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
