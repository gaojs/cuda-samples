# clock_nvrtc - 时钟 libNVRTC

## 描述

此示例展示如何使用 libNVRTC 和 clock 函数准确测量 kernel 中线程块的性能。

## 关键概念

性能策略, 运行时编译

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows, QNX

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuLaunchKernel, cuMemcpyHtoD, cuCtxSynchronize, cuMemAlloc, cuMemFree, cuModuleGetFunction

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaBlockSize, cudaGridSize

## 构建/运行所需依赖
[NVRTC](../../../README.md#nvrtc)

## 前置条件

下载并安装适用于对应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上方[依赖](#dependencies)部分所述的依赖。

## 参考资料（更多详情）
