# threadMigration - CUDA 上下文线程管理

## 描述

这是一个简单程序，用于演示 CUDA 上下文管理 API，以及 CUDA 4.0 新增的参数传递和 CUDA 启动 API。CUDA 上下文可以单独创建，并分别附加到不同线程。

## 核心概念

CUDA Driver API

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuLaunchKernel, cuModuleLoadData, cuDeviceGetName, cuDeviceGet, cuDeviceGetAttribute, cuMemAlloc, cuMemFree, cuCtxDestroy, cuCtxPopCurrent, cuModuleUnload, cuDeviceGetCount, cuModuleGetFunction, cuCtxCreate, cuCtxPushCurrent, cuInit

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（详情）
