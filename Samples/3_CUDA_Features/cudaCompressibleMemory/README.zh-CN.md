# cudaCompressibleMemory - CUDA 可压缩内存

## 描述

本示例演示使用 cuMemMap API 分配可压缩内存。

## 关键概念

CUDA Driver API、可压缩内存、MMAP

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows, QNX

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemGetAllocationPropertiesFromHandle, cuMemCreate, cuDeviceGetAttribute, cuCtxGetDevice, cuMemGetAllocationGranularity, cuMemAddressFree, cuMemUnmap, cuMemMap, cuMemRelease, cuMemAddressReserve, cuMemSetAccess

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaEventSynchronize, cudaEventRecord, cudaEventElapsedTime, cudaOccupancyMaxPotentialBlockSize, cudaEventCreate

## 先决条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（详情）
