# warpAggregatedAtomicsCG - 使用 Cooperative Groups 聚合 Warp 原子操作

## 描述

此示例演示如何使用 Cooperative Groups (CG) 对单个和多个计数器执行 warp 聚合原子操作。当许多线程以原子方式递增一个或多个计数器时，这是一种有助于提升性能的技术。

## 关键概念

Cooperative Groups、原子操作内建函数

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaDeviceGetAttribute, cudaMemset, cudaMalloc

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
