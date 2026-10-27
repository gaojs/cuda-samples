# immaTensorCoreGemm - Tensor Core 整数 GEMM

## 描述

此 CUDA 示例演示使用 CUDA 10 引入的整数 Warp Matrix Multiply and Accumulate (WMMA) API 执行整数 GEMM 计算。该示例展示了如何使用 Volta 芯片系列引入的 Tensor Cores 和 CUDA WMMA API 来加速矩阵运算。此外，它还演示了新的 CUDA 函数属性 cudaFuncAttributeMaxDynamicSharedMemorySize 的用法，该属性允许应用程序预留超过默认可用量的共享内存。

## 关键概念

矩阵乘法、WMMA、Tensor Cores

## 支持的 SM 架构

[SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaGetErrorString, cudaGetLastError, cudaEventSynchronize, cudaFuncSetAttribute, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
