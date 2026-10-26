# asyncAPI - asyncAPI

## 描述

此示例演示如何使用 CUDA 事件进行 GPU 计时，以及实现 CPU 与 GPU 执行重叠。事件会插入 CUDA 调用流中。由于 CUDA 流调用是异步的，GPU 执行任务（包括主机与设备之间的 DMA 内存复制）时，CPU 可以同时进行计算。CPU 可以查询 CUDA 事件，以确定 GPU 是否已完成任务。

## 关键概念

异步数据传输, CUDA 流和事件

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaProfilerStop, cudaMalloc, cudaMemcpyAsync, cudaFree, cudaMallocHost, cudaProfilerStart, cudaDeviceSynchronize, cudaEventRecord, cudaFreeHost, cudaMemset, cudaEventDestroy, cudaEventQuery, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
