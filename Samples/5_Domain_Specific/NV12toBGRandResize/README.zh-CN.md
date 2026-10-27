# NV12toBGRandResize - NV12 转 BGR 并缩放

## 描述

此代码展示使用 CUDA 批量将 NV12 帧转换并缩放为 BGR 三平面帧的两种方法。方法一：将 NV12 输入转换为 BGR（分辨率为输入分辨率 #1），然后缩放至分辨率 #2。方法二：先将 NV12 输入缩放至分辨率 #2，再转换为 BGR 输出。NVIDIA 硬件解码器（包括 dGPU 和 Tegra）通常输出 NV12 pitch 格式帧。使用 TensorRT 进行推理时，输入帧需要为 BGR 平面格式，且尺寸可能不同。因此，解码后通常需要将 NV12 转换并缩放为 BGR 平面格式，以供后续推理使用。此 CUDA 代码提供转换和缩放的参考实现。

## 核心概念

图形互操作、图像处理、视频处理

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux, Windows

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaStreamDestroy, cudaMalloc, cudaFree, cudaMallocManaged, cudaStreamAttachMemAsync, cudaDestroyTextureObject, cudaEventSynchronize, cudaDeviceSynchronize, cudaCreateTextureObject, cudaEventRecord, cudaEventDestroy, cudaEventElapsedTime, cudaStreamCreate, cudaEventCreate

## 先决条件

为相应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
