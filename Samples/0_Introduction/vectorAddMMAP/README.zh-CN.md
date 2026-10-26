# vectorAddMMAP - 使用 cuMemMap 的向量加法

## 描述

此示例将 vectorAddDrv 示例中的设备内存分配替换为通过 cuMemMap 分配的内存。它展示 cuMemMap API 允许用户指定内存的物理属性，同时保留访问的连续性，因此无需更改程序结构。

## 关键概念

CUDA Driver API, 向量加法, MMAP

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, ppc64le

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuDeviceCanAccessPeer, cuModuleGetFunction, cuMemSetAccess, cuMemRelease, cuInit, cuMemcpyHtoD, cuLaunchKernel, cuMemCreate, cuModuleLoadData, cuCtxDestroy, cuDeviceGetCount, cuMemMap, cuDeviceGetAttribute, cuMemGetAllocationGranularity, cuMemAddressFree, cuMemUnmap, cuCtxCreate, cuMemAddressReserve

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）

