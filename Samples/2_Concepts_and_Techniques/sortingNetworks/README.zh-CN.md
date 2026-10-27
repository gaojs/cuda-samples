# sortingNetworks - CUDA Sorting Networks

## 描述

此示例实现双调排序和奇偶归并排序（也称 Batcher 排序），这两种算法都属于排序网络。与归并排序或基数排序等渐近复杂度更优的算法相比，排序网络通常效率较低；但在对大量短到中等长度的（键、值）数组对进行批量排序时，它可能是更合适的选择。请参阅 H. W. Lang 编写的优秀教程 http://www.iti.fh-flensburg.de/lang/algorithmen/sortieren/networks/indexen.htm

## 核心概念

Data-Parallel Algorithms

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMalloc, cudaDeviceSynchronize, cudaMemcpy, cudaFree

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（详情）
