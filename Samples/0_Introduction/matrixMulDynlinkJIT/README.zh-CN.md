# matrixMulDynlinkJIT - Matrix Multiplication (CUDA Driver API version with Dynamic Linking Version)

## 描述

此示例使用 CUDA Driver API 重新演示矩阵乘法，展示如何在运行时链接 CUDA driver，以及如何对 PTX 代码进行 JIT（即时）编译。该示例旨在清晰说明各种 CUDA 编程原则，而非提供性能最高的通用矩阵乘法 kernel。CUBLAS 可提供高性能矩阵乘法。

## 关键概念

CUDA Driver API, CUDA 动态链接库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuDeviceGetName, cuParamSeti, cuModuleLoadDataEx, cuModuleGetFunction, cuLaunchGrid, cuFuncSetSharedSize, cuMemFree, cuParamSetSize, cuParamSetv, cuInit, cuMemcpyHtoD, cuLaunchKernel, cuDeviceGet, cuFuncSetBlockShape, cuCtxDestroy, cuDeviceGetCount, cuDeviceComputeCapability, cuCtxSynchronize, cuMemAlloc, cuCtxCreate

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
