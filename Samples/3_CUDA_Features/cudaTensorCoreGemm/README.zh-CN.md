# cudaTensorCoreGemm - CUDA Tensor Core GEMM

## 描述

CUDA 示例，演示使用 CUDA 9 引入的 Warp Matrix Multiply and Accumulate (WMMA) API 执行 GEMM 计算。

本示例演示使用 CUDA WMMA API 和 Volta 芯片系列中引入的 Tensor Core 加速矩阵运算。

此外，本示例还演示新的 CUDA 函数属性 cudaFuncAttributeMaxDynamicSharedMemorySize 的用法，该属性允许应用程序预留比默认可用量更多的共享内存。

## 关键概念

矩阵乘法、WMMA、Tensor Core

## 支持的 SM 架构

[SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaGetErrorString, cudaGetLastError, cudaEventSynchronize, cudaFuncSetAttribute, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 先决条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（详情）
