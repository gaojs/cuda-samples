# simpleCudaGraphs - 简单 CUDA Graphs

## 描述

演示使用 Graphs APIs 和 Stream Capture APIs 创建、实例化和启动 CUDA Graphs。

## 关键概念

CUDA Graphs、Stream Capture

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphClone, cudaExtent, cudaGraphLaunch, cudaStreamCreate, cudaLaunchHostFunc, cudaGraphAddMemcpyNode, cudaMallocHost, cudaPitchedPtr, cudaStreamEndCapture, cudaGraphCreate, cudaFreeHost, cudaGraphGetNodes, cudaMemsetAsync, cudaMemcpyAsync, cudaGraphAddHostNode, cudaGraphInstantiate, cudaStreamDestroy, cudaStreamBeginCapture, cudaStreamWaitEvent, cudaEventCreate, cudaMalloc, cudaFree, cudaPos, cudaGraphAddKernelNode, cudaGraphDestroy, cudaEventRecord, cudaGraphsManual, cudaStreamSynchronize, cudaGraphAddMemsetNode, cudaGraphsUsingStreamCapture, cudaGraphExecDestroy

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
