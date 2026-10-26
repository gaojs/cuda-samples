# simpleDrvRuntime - Simple Driver-Runtime Interaction

## 描述

简单示例，演示 CUDA Driver API 和 Runtime API 如何协同工作，以加载向量加法 kernel 的 CUDA fatbinary 并执行向量加法。

## 关键概念

CUDA Driver API, CUDA Runtime API, 向量加法

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuLaunchKernel, cuModuleLoadData, cuCtxDestroy, cuModuleUnload, cuModuleGetFunction, cuCtxCreate, cuInit

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamCreateWithFlags, cudaFree, cudaMallocHost, cudaFreeHost, cudaStreamSynchronize, cudaMalloc, cudaMemcpyAsync

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
