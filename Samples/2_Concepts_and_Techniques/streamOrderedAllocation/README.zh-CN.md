# streamOrderedAllocation - 流序内存分配

## 描述

此示例演示如何使用 cudaMallocAsync 和 cudaMemPool 系列 API 在 GPU 上执行流序内存分配。

## 核心概念

性能优化策略

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceGetDefaultMemPool, cudaFreeAsync, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaDeviceGetAttribute, cudaMallocAsync, cudaSetDevice, cudaEventSynchronize, cudaEventRecord, cudaStreamSynchronize, cudaMemPoolSetAttribute, cudaEventElapsedTime, cudaMemcpyAsync, cudaEventCreate

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。

## 参考资料（详情）
