# cdpAdvancedQuicksort - 高级快速排序（CUDA Dynamic Parallelism）

## 描述

本示例演示使用 CUDA Dynamic Parallelism 实现的高级快速排序。此示例需要计算能力为 3.5 或更高的设备。

## 关键概念

Cooperative Groups、CUDA Dynamic Parallelism

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamCreateWithFlags, cudaMemcpy, cudaMemcpyAsync, cudaFree, cudaGetErrorString, cudaGetLastError, cudaPeekAtLastError, cudaDeviceSynchronize, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 构建/运行所需依赖
[CDP](../../../README.md#cdp)

## 先决条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分提到的依赖项。

## 参考资料（详情）
