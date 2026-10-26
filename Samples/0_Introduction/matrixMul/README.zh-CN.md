# matrixMul - Matrix Multiplication (CUDA Runtime API Version)

## 描述

此示例实现矩阵乘法，内容与《编程指南》第 6 章完全相同。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。为展示 GPU 上的矩阵乘法性能，该示例还使用新的 CUDA 4.0 CUBLAS 接口演示高性能矩阵乘法。

## 关键概念

CUDA Runtime API, 线性代数

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamCreateWithFlags, cudaProfilerStop, cudaMalloc, cudaFree, cudaMallocHost, cudaProfilerStart, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaStreamSynchronize, cudaEventDestroy, cudaEventElapsedTime, cudaMemcpyAsync, cudaEventCreate

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
