# transpose - 矩阵转置

## 描述

本示例演示矩阵转置，并展示多种实现策略及其性能差异。

## 核心概念

性能策略、线性代数。

## 支持的 SM 架构

SM 5.0、5.2、5.3、6.0、6.1、7.0、7.2、7.5、8.0、8.6、8.7、8.9、9.0。

## 支持的操作系统

Linux、Windows。

## 支持的 CPU 架构

x86_64、armv7l。

## 涉及的 CUDA API

### CUDA Runtime API

`cudaMemcpy`、`cudaMalloc`、`cudaFree`、`cudaGetLastError`、`cudaEventSynchronize`、`cudaEventRecord`、`cudaGetDevice`、`cudaEventDestroy`、`cudaEventElapsedTime`、`cudaGetDeviceProperties`、`cudaEventCreate`。

## 前置条件

针对相应平台下载并安装 [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)。本仓库分支基于 CUDA Samples 12.8，构建环境要求请参阅仓库编译指导。

## 参考资料

[白皮书](./doc/MatrixTranspose.pdf)
