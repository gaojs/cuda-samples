# simpleIPC - simpleIPC

## 描述

这是一个基础的 CUDA Runtime API 示例，演示如何为计算任务使用每个 GPU 对应一个进程的进程间通信。需要 Compute Capability 3.0 或更高版本，以及 Linux 或 Windows 操作系统。

## 关键概念

CUDA 系统集成, 点对点, 进程间通信

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, ppc64le

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaSetDevice, cudaIpcCloseMemHandle, cudaEventDestroy, cudaGetDeviceCount, cudaMemcpyAsync, cudaDeviceCanAccessPeer, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaGetLastError, cudaIpcOpenEventHandle, cudaIpcOpenMemHandle, cudaIpcGetEventHandle, cudaStreamWaitEvent, cudaEventCreate, cudaFree, cudaEventSynchronize, cudaEventRecord, cudaIpcGetMemHandle, cudaStreamSynchronize, cudaDeviceEnablePeerAccess, cudaMalloc, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaGetDeviceProperties

## 构建/运行所需依赖
[IPC](../../../README.md#ipc)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）

