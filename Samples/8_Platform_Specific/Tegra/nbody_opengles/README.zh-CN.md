# nbody_opengles - 使用 GLES 的 CUDA N 体模拟

## 描述

此示例演示在 CUDA 中高效地进行全对全引力 N 体模拟。与 OpenGL nbody 示例不同，此示例不提供用户交互。

## 关键概念

图形互操作、数据并行算法、基于物理的模拟

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaSetDeviceFlags, cudaGraphicsResourceSetMapFlags, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaSetDevice, cudaEventSynchronize, cudaGetDeviceProperties, cudaDeviceSynchronize, cudaEventRecord, cudaGetDevice, cudaMemcpyToSymbol, cudaStreamQuery, cudaEventDestroy, cudaEventElapsedTime, cudaGetDeviceCount, cudaEventCreate

## 构建/运行所需依赖
[X11](../../../README.md#x11), [GLES](../../../README.md#gles)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文 [Dependencies]() 部分提到的依赖。

## 参考资料（更多详情）
