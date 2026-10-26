# simpleCallback - Simple CUDA Callbacks

## 描述

此示例使用 CUDA 5.0 引入的 CUDA 流和事件 CPU 回调，实现多线程异构计算工作负载。

## 关键概念

CUDA 流, 回调函数, 多线程

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaHostAlloc, cudaStreamDestroy, cudaFree, cudaSetDevice, cudaGetDeviceCount, cudaFreeHost, cudaStreamAddCallback, cudaMalloc, cudaMemcpyAsync, cudaStreamCreate, cudaGetDeviceProperties

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
