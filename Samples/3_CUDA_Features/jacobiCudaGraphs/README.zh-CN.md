# jacobiCudaGraphs - Jacobi CUDA Graphs

## 描述

演示使用 Jacobi 迭代方法更新已实例化的 CUDA Graph，采用 cudaGraphExecKernelNodeSetParams() 和 cudaGraphExecUpdate() 方式。

## 关键概念

CUDA Graphs、Stream Capture、已实例化 CUDA Graph 更新、Cooperative Groups

## 支持的 SM 架构

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaExtent, cudaGraphLaunch, cudaGraphAddMemcpyNode, cudaMallocHost, cudaPitchedPtr, cudaStreamEndCapture, cudaGraphCreate, cudaFreeHost, cudaMemsetAsync, cudaMemcpyAsync, cudaGraphExecKernelNodeSetParams, cudaStreamCreateWithFlags, cudaGraphInstantiate, cudaStreamBeginCapture, cudaFree, cudaGraphExecUpdate, cudaGraphAddKernelNode, cudaPos, cudaStreamSynchronize, cudaGraphAddMemsetNode, cudaMalloc, cudaGraphExecDestroy

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
