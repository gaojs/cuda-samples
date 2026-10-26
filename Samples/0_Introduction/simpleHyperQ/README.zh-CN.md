# simpleHyperQ - simpleHyperQ

## 描述

此示例演示如何使用 CUDA 流，在支持 HyperQ（SM 3.5）的设备上并发执行多个 kernel。不支持 HyperQ 的设备（SM 2.0 和 SM 3.0）最多会并发运行两个 kernel。

## 关键概念

CUDA 系统集成, 性能策略

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaStreamDestroy, cudaMalloc, cudaFree, cudaMallocHost, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaGetDevice, cudaEventDestroy, cudaEventElapsedTime, cudaStreamCreate, cudaGetDeviceProperties, cudaEventCreate

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）

[whitepaper](./doc/HyperQ.pdf)
