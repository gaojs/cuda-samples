# alignedTypes - 对齐类型

## 描述

通过简单测试展示结构体对齐与非对齐访问之间的速度差异。它针对大块数据测量逐元素拷贝吞吐量。

## 核心概念

性能策略。

## 支持的 SM 架构

SM 5.0、5.2、5.3、6.0、6.1、7.0、7.2、7.5、8.0、8.6、8.7、8.9、9.0。

## 支持的操作系统

Linux、Windows。

## 支持的 CPU 架构

x86_64、armv7l。

## 涉及的 CUDA API

### CUDA Runtime API

`cudaMemcpy`、`cudaFree`、`cudaDeviceSynchronize`、`cudaMemset`、`cudaMalloc`、`cudaGetDeviceProperties`。

## 前置条件

针对相应平台下载并安装 [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)。本仓库分支基于 CUDA Samples 12.8，构建环境要求请参阅仓库编译指导。

## 参考资料
