# simpleMultiCopy - Simple Multi Copy and Compute

## 描述

对于 Compute Capability 1.1 的 GPU，可以从主机系统执行计算并与一次内存复制重叠。对于 Compute Capability 2.0 的 Quadro 和 Tesla GPU，还可以以全速进行第二次重叠复制，方向不限（PCI-e 是对称的）。此示例演示如何使用 CUDA 流，使 kernel 执行与往返设备的数据复制重叠。

## 关键概念

CUDA 流和事件, 异步数据传输, 计算与复制重叠, GPU 性能

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaHostAlloc, cudaStreamDestroy, cudaMalloc, cudaMemcpyAsync, cudaFree, cudaSetDevice, cudaEventSynchronize, cudaDeviceSynchronize, cudaEventRecord, cudaFreeHost, cudaMemset, cudaEventDestroy, cudaEventElapsedTime, cudaStreamCreate, cudaGetDeviceProperties, cudaEventCreate

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
