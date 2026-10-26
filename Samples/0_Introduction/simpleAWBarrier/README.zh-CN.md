# simpleAWBarrier - Simple Arrive Wait Barrier

## 描述

简单演示到达-等待（arrive-wait）屏障。

## 关键概念

到达-等待屏障

## 支持的 SM 架构

[SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows, QNX

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamCreateWithFlags, cudaFree, cudaDeviceGetAttribute, cudaMallocHost, cudaFreeHost, cudaStreamSynchronize, cudaLaunchCooperativeKernel, cudaMalloc, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaMemcpyAsync, cudaOccupancyMaxPotentialBlockSize

## 构建/运行所需依赖
[CPP11](../../../README.md#cpp11), [MBCG](../../../README.md#mbcg)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）
