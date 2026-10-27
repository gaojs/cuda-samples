# streamOrderedAllocationIPC - 流序分配 IPC 池

## 描述

此示例演示使用 cudaMallocAsync 和 cudaMemPool 系列 API 分配的流序内存 IPC 池。

## 核心概念

性能优化策略

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

x86_64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetAttribute, cuDeviceGet

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceGetAttribute, cudaMemPoolImportFromShareableHandle, cudaSetDevice, cudaMemPoolExportPointer, cudaMemPoolGetAccess, cudaMemPoolDestroy, cudaMemPoolSetAccess, cudaMallocAsync, cudaMemPoolImportPointer, cudaGetDeviceCount, cudaMemcpyAsync, cudaDeviceCanAccessPeer, cudaFreeAsync, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaGetLastError, cudaMemPoolCreate, cudaMemPoolExportToShareableHandle, cudaStreamSynchronize, cudaDeviceEnablePeerAccess, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaGetDeviceProperties

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。

## 参考资料（详情）
