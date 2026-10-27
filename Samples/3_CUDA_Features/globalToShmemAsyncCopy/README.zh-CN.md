# globalToShmemAsyncCopy - 全局内存到共享内存的异步复制

## 描述

此示例实现矩阵乘法：当计算能力为 8.0 或更高时，使用从全局内存到共享内存的数据异步复制。还演示了用于同步的 arrive-wait barrier。

## 关键概念

CUDA Runtime API、线性代数、CPP11 CUDA

## 支持的 SM 架构

[SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows, QNX

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamCreateWithFlags, cudaMalloc, cudaDeviceGetAttribute, cudaFree, cudaMallocHost, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaStreamSynchronize, cudaEventDestroy, cudaEventElapsedTime, cudaMemsetAsync, cudaMemcpyAsync, cudaEventCreate

## 构建/运行所需依赖项
[CPP11](../../../README.md#cpp11)

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
