# dmmaTensorCoreGemm - 双精度 Tensor Core GEMM

## 描述

CUDA 示例，演示使用 CUDA 11 在 Ampere 芯片系列 Tensor Core 中引入的双精度 Warp Matrix Multiply and Accumulate (WMMA) API 执行双精度 GEMM 计算，以加速矩阵运算。本示例还使用 cuda pipeline 接口提供的异步复制执行从 gmem 到 shmem 的异步加载，从而提升内核性能并减少寄存器压力。此外，本示例还演示如何使用 Cooperative Groups 异步复制接口在一个组内执行从 gmem 到 shmem 的异步加载。

## 关键概念

矩阵乘法、WMMA、Tensor Core

## 支持的 SM 架构

[SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaGetErrorString, cudaGetLastError, cudaEventSynchronize, cudaFuncSetAttribute, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 构建/运行所需依赖
[CPP11](../../../README.md#cpp11)

## 先决条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分提到的依赖项。

## 参考资料（详情）
