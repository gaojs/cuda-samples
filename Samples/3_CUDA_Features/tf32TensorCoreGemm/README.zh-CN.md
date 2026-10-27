# tf32TensorCoreGemm - tf32 Tensor Core GEMM

## 描述

此 CUDA 示例演示使用 CUDA 11 引入的 Warp Matrix Multiply and Accumulate (WMMA) API，在 Ampere 芯片系列 Tensor Cores 上执行 tf32 (e8m10) GEMM 计算，以加速矩阵运算。此示例还使用 cuda pipeline interface 提供的异步复制执行从 gmem 到 shmem 的异步加载，从而提升 kernel 性能并减少寄存器压力。

## 关键概念

矩阵乘法、WMMA、Tensor Cores

## 支持的 SM 架构

[SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaGetErrorString, cudaGetLastError, cudaEventSynchronize, cudaFuncSetAttribute, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 构建/运行所需依赖项
[CPP11](../../../README.md#cpp11)

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
