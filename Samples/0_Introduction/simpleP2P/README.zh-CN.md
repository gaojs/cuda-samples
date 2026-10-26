# simpleP2P - Simple Peer-to-Peer Transfers with Multi-GPU

## 描述

此应用演示支持多个 GPU 之间点对点（P2P）复制、点对点寻址和统一虚拟内存寻址（UVA）的 CUDA API。通常，同型号 GPU 之间支持 P2P，但某些 Tesla 和 Quadro GPU 等情况除外。

## 关键概念

性能策略, 异步数据传输, Unified Virtual Address Space, Peer to Peer Data Transfers, 多 GPU

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, ppc64le

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaMalloc, cudaFree, cudaMallocHost, cudaEventCreateWithFlags, cudaSetDevice, cudaEventSynchronize, cudaDeviceDisablePeerAccess, cudaGetDeviceCount, cudaDeviceSynchronize, cudaEventRecord, cudaFreeHost, cudaGetDeviceProperties, cudaDeviceEnablePeerAccess, cudaEventDestroy, cudaEventElapsedTime, cudaDeviceCanAccessPeer

## 构建/运行所需依赖
[only-64-bit](../../../README.md#only-64-bit)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）

