# StreamPriorities - Stream 优先级

## 描述

此示例演示 Stream 优先级的基本用法。

## 关键概念

CUDA Streams 和 Events

## 支持的 SM 架构

## 支持的操作系统

Linux

## 支持的 CPU 架构

x86_64, ppc64le

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaStreamCreateWithPriority, cudaDeviceGetStreamPriorityRange, cudaEventSynchronize, cudaEventRecord, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 构建/运行所需依赖项
[Stream-Priorities](../../../README.md#stream-priorities)

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
