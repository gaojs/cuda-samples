# UnifiedMemoryPerf - 统一内存及其他 CUDA 内存性能

## 描述

本示例使用矩阵乘法 Kernel，在单 GPU 上比较多种内存类型和传输方式的性能：带或不带提示的统一内存、零拷贝缓冲区、可分页内存和页锁定内存，以及同步和异步传输。

## 核心概念

CUDA 系统集成、统一内存、CUDA Streams 与 Events、页锁定系统内存。

## 支持的 SM 架构

原 README 未列出。

## 支持的操作系统

Linux、Windows。

## 支持的 CPU 架构

x86_64、armv7l、aarch64。

## 涉及的 CUDA API

### CUDA Runtime API

`cudaMemcpy`、`cudaStreamDestroy`、`cudaMemPrefetchAsync`、`cudaFree`、`cudaMallocHost`、`cudaMallocManaged`、`cudaStreamAttachMemAsync`、`cudaHostGetDevicePointer`、`cudaFreeHost`、`cudaStreamSynchronize`、`cudaMalloc`、`cudaMemcpyAsync`、`cudaStreamCreate`、`cudaGetDeviceProperties`。

## 构建/运行所需依赖

[UVM](../../../README.md#uvm)。

## 前置条件

针对相应平台下载并安装 [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)，并确保已安装上述依赖。本仓库分支基于 CUDA Samples 12.8，具体构建环境要求请参阅仓库编译指导。

## 参考资料
