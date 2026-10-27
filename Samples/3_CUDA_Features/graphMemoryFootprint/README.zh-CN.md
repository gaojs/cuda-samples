# graphMemoryFootprint - 图内存占用

## 描述

此示例演示图内存节点如何重用虚拟地址和物理内存。

## 关键概念

CUDA Runtime API、性能策略、CUDA Graphs

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphAddMemAllocNode, cudaStreamCreateWithFlags, cudaGraphInstantiate, cudaStreamDestroy, cudaFree, cudaDeviceGetAttribute, cudaGraphAddKernelNode, cudaGraphAddMemFreeNode, cudaDeviceGetGraphMemAttribute, cudaGraphCreate, cudaGraphDestroy, cudaDriverGetVersion, cudaGraphLaunch, cudaStreamSynchronize, cudaDeviceGraphMemTrim, cudaGetDeviceProperties, cudaGraphExecDestroy

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
