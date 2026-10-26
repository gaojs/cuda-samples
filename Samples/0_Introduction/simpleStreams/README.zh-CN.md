# simpleStreams - simpleStreams

## 描述

此示例使用 CUDA 流，使 kernel 执行与主机和 GPU 设备之间的内存复制重叠。它使用 CUDA 4.0 中新增的通用主机内存固定功能。需要 Compute Capability 2.0 或更高版本。

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
cudaMemcpy, cudaSetDeviceFlags, cudaSetDevice, cudaEventDestroy, cudaStreamCreate, cudaMallocHost, cudaEventCreateWithFlags, cudaFreeHost, cudaMemcpyAsync, cudaGetDeviceCount, cudaStreamDestroy, cudaMemset, cudaEventElapsedTime, cudaHostAlloc, cudaFree, cudaHostRegister, cudaEventSynchronize, cudaEventRecord, cudaMalloc, cudaGetDeviceProperties, cudaHostUnregister

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
