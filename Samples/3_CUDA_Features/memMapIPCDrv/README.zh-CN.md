# memMapIPCDrv - Memmap IPC Driver API

## 描述

此 CUDA Driver API 示例非常基础，演示了使用 cuMemMap APIs 进行进程间通信，并由每个 GPU 上的一个进程执行计算。要求计算能力为 3.0 或更高，并使用 Linux 或 Windows 操作系统。

## 关键概念

CUDA Driver API、cuMemMap IPC、MMAP

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows, QNX

## 支持的 CPU 架构

x86_64, armv7l, aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceCanAccessPeer, cuMemImportFromShareableHandle, cuModuleLoadDataEx, cuModuleGetFunction, cuMemSetAccess, cuModuleLoad, cuStreamCreate, cuMemRelease, cuInit, cuLaunchKernel, cuMemcpyDtoHAsync, cuMemCreate, cuDeviceGet, cuCtxDestroy, cuDeviceGetCount, cuMemMap, cuMemExportToShareableHandle, cuStreamSynchronize, cuCtxEnablePeerAccess, cuDeviceGetAttribute, cuOccupancyMaxActiveBlocksPerMultiprocessor, cuCtxSetCurrent, cuMemGetAllocationGranularity, cuMemAddressFree, cuMemUnmap, cuCtxCreate, cuStreamDestroy, cuMemAddressReserve

## 构建/运行所需依赖项
[IPC](../../../README.md#ipc)

## 先决条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上述[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
