# conjugateGradientUM - ConjugateGradientUM

## 描述

此示例使用 CUBLAS 和 CUSPARSE 库以及统一内存，在 GPU 上实现共轭梯度求解器。

## 核心概念

统一内存, 线性代数, CUBLAS 库, CUSPARSE 库

## 支持的 SM 架构

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaFree, cudaMallocManaged, cudaDeviceSynchronize, cudaMalloc, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[UVM](../../../README.md#uvm), [CUBLAS](../../../README.md#cublas), [CUSPARSE](../../../README.md#cusparse)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
