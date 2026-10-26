# bandwidthTest - 带宽测试

## 描述

这是一个简单的测试程序，用于测量 GPU 内存拷贝带宽以及 PCIe 上的拷贝带宽。该程序可以测量设备到设备的拷贝带宽，也可以测量使用可分页内存和页锁定内存时，主机到设备及设备到主机的拷贝带宽。

## 关键概念

内存拷贝带宽测量、可分页与页锁定主机内存、CUDA Events（事件）计时、性能测量策略。程序使用异步拷贝和事件计时来测量页锁定内存传输；这里涉及 CUDA 异步操作，但该示例的重点是带宽测试，不是 Stream 并发或计算与传输重叠。

## 支持的 SM 架构

[SM 5.0](https://developer.nvidia.com/cuda-gpus)  [SM 5.2](https://developer.nvidia.com/cuda-gpus)  [SM 5.3](https://developer.nvidia.com/cuda-gpus)  [SM 6.0](https://developer.nvidia.com/cuda-gpus)  [SM 6.1](https://developer.nvidia.com/cuda-gpus)  [SM 7.0](https://developer.nvidia.com/cuda-gpus)  [SM 7.2](https://developer.nvidia.com/cuda-gpus)  [SM 7.5](https://developer.nvidia.com/cuda-gpus)  [SM 8.0](https://developer.nvidia.com/cuda-gpus)  [SM 8.6](https://developer.nvidia.com/cuda-gpus)  [SM 8.7](https://developer.nvidia.com/cuda-gpus)  [SM 8.9](https://developer.nvidia.com/cuda-gpus)  [SM 9.0](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)

`cudaHostAlloc`、`cudaMemcpy`、`cudaMalloc`、`cudaMemcpyAsync`、`cudaFree`、`cudaGetErrorString`、`cudaMallocHost`、`cudaSetDevice`、`cudaGetDeviceProperties`、`cudaDeviceSynchronize`、`cudaEventRecord`、`cudaFreeHost`、`cudaEventDestroy`、`cudaEventElapsedTime`、`cudaGetDeviceCount`、`cudaEventCreate`

## 前置条件

为对应的平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
