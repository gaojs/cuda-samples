# UnifiedMemoryStreams - 统一内存流

## 描述

此示例演示如何在单个 GPU 上结合使用 OpenMP、流和统一内存。

## 关键概念

CUDA 系统集成, OpenMP, CUBLAS, 多线程, 统一内存, CUDA 流和事件

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamDestroy, cudaFree, cudaMallocManaged, cudaStreamAttachMemAsync, cudaSetDevice, cudaDeviceSynchronize, cudaStreamSynchronize, cudaStreamCreate, cudaGetDeviceProperties

## 构建/运行所需依赖
[OpenMP](../../../README.md#openmp), [UVM](../../../README.md#uvm), [CUBLAS](../../../README.md#cublas)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）
