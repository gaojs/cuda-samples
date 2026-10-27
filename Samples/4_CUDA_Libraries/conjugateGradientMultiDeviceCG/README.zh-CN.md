# conjugateGradientMultiDeviceCG - conjugateGradient 使用 MultiDevice 协作组

## 描述

此示例实现 a 共轭梯度求解器 在多个 GPU 上 使用 Multi Device 协作组, also uses 统一内存 optimized 使用 prefetching 和 usage hints.

## 核心概念

统一内存, 线性代数, 协作组, MultiDevice 协作组, CUBLAS 库, CUSPARSE 库

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaHostAlloc, cudaMemPrefetchAsync, cudaFree, cudaLaunchCooperativeKernel, cudaMallocManaged, cudaSetDevice, cudaGetDeviceCount, cudaGetDeviceProperties, cudaFreeHost, cudaMemset, cudaStreamCreate, cudaStreamSynchronize, cudaDeviceEnablePeerAccess, cudaMemAdvise, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaDeviceCanAccessPeer

## 构建/运行所需的依赖项
[UVM](../../../README.md#uvm), [MDCG](../../../README.md#mdcg), [CPP11](../../../README.md#cpp11)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
