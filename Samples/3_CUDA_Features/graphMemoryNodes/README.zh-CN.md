# graphMemoryNodes - 图内存节点

## 描述

演示在 CUDA graphs 中使用 Graph APIs 和 Stream Capture APIs 进行内存分配与释放。

## 关键概念

CUDA Graphs、Stream Capture

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaDeviceGetAttribute, cudaDriverGetVersion, cudaGraphLaunch, cudaEventDestroy, cudaMallocAsync, cudaStreamEndCapture, cudaMallocManaged, cudaGraphCreate, cudaMemcpyAsync, cudaFreeAsync, cudaStreamCreateWithFlags, cudaGraphInstantiate, cudaStreamDestroy, cudaStreamBeginCapture, cudaStreamWaitEvent, cudaEventCreate, cudaGraphAddMemAllocNode, cudaFree, cudaGraphAddKernelNode, cudaGraphAddMemFreeNode, cudaGraphDestroy, cudaEventRecord, cudaStreamSynchronize, cudaMalloc, cudaGraphExecDestroy

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
