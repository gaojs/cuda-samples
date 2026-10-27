# p2pBandwidthLatencyTest - 多 GPU 点对点带宽与延迟测试

## 描述

此应用演示成对 GPU 之间的 CUDA Peer-To-Peer (P2P) 数据传输，并计算延迟和带宽。测试将分别针对启用和未启用 P2P 的 GPU 对进行。

## 核心概念

性能策略、异步数据传输、统一虚拟地址空间、点对点数据传输、多 GPU

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaSetDevice, cudaEventDestroy, cudaOccupancyMaxPotentialBlockSize, cudaCheckError, cudaFreeHost, cudaGetDeviceCount, cudaDeviceCanAccessPeer, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaGetLastError, cudaMemset, cudaStreamWaitEvent, cudaEventElapsedTime, cudaEventCreate, cudaHostAlloc, cudaFree, cudaGetErrorString, cudaMemcpyPeerAsync, cudaDeviceDisablePeerAccess, cudaEventRecord, cudaStreamSynchronize, cudaDeviceEnablePeerAccess, cudaMalloc, cudaGetDeviceProperties

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
