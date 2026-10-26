# LargeKernelParameter - 大型 Kernel 参数

## 描述

简单测试，演示 CUDA 12.1 引入的大型 Kernel 参数在性能和易用性方面的改进。

## 核心概念

性能策略。

## 支持的 SM 架构

SM 7.0、7.2、7.5、8.0、8.6、8.7、8.9、9.0。

## 支持的操作系统

Linux、Windows。

## 支持的 CPU 架构

x86_64、armv7l。

## 涉及的 CUDA API

### CUDA Runtime API

`cudaMemcpyToSymbol`、`cudaMemcpy`、`cudaFree`、`cudaDeviceSynchronize`、`cudaMalloc`。

## 前置条件

针对相应平台下载并安装 [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)。本仓库分支基于 CUDA Samples 12.8，构建环境要求请参阅仓库编译指导。

## 参考资料
