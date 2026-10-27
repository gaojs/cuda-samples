# MonteCarloMultiGPU - 多 GPU Monte Carlo 期权定价

## 描述

此示例使用 Monte Carlo 方法评估一组欧式期权的公平看涨期权价格，并利用系统中安装的所有支持 CUDA 的 GPU。如果存在 GTX 200 系列 GPU，此示例会使用双精度硬件。此示例还利用 CUDA 4.0 的能力，通过单个 CPU 线程控制多个 GPU。

## 核心概念

随机数生成器、计算金融、CURAND 库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaStreamDestroy, cudaMalloc, cudaFree, cudaMallocHost, cudaSetDevice, cudaEventSynchronize, cudaGetDeviceProperties, cudaDeviceSynchronize, cudaEventRecord, cudaFreeHost, cudaMemset, cudaStreamSynchronize, cudaEventDestroy, cudaMemcpyAsync, cudaStreamCreate, cudaGetDeviceCount, cudaEventCreate

## 构建/运行所需依赖项
[CURAND](../../../README.md#curand)

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文[依赖项]()部分提到的依赖项。

## 参考资料（更多详情）

[白皮书](./doc/MonteCarlo.pdf)
