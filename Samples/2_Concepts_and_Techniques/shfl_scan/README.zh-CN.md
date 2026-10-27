# shfl_scan - 使用 Shuffle 内置函数实现 CUDA 并行前缀和（SHFL_Scan）

## 描述

此示例演示如何使用 Shuffle 内置函数 __shfl_up_sync 在一个线程块内执行扫描操作。

## 核心概念

数据并行算法、性能优化策略

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaMallocHost, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaGetDevice, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 构建/运行所需依赖
[CPP11](../../../README.md#cpp11)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上文 [Dependencies]() 一节中提到的依赖。

## 参考资料（详情）
