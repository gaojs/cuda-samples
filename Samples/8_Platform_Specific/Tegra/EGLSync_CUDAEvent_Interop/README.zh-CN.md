# EGLSync_CUDAEvent_Interop - EGLSync CUDA Event 互操作

## 描述

演示 CUDA Event 与 EGL Sync/EGL Image 之间的互操作。借此可以在 GPU 本身上对 GL-EGL-CUDA 操作进行同步，而无需阻塞 CPU 来进行同步。

## 关键概念

EGLSync-CUDAEvent Interop、EGLImage-CUDA Interop

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuEventRecord, cuDeviceGetAttribute, cuEventCreate, cuCtxSynchronize, cuEventDestroy, cuGraphicsEGLRegisterImage, cuGraphicsSubResourceGetMappedArray, cuStreamCreate, cuStreamWaitEvent, cuGraphicsUnregisterResource, cuCtxCreate, cuSurfObjectCreate, cuEventCreateFromEGLSync, cuCtxPushCurrent, cuInit

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaGetErrorString, cudaFree, cudaDeviceSynchronize, cudaGetValueMismatch, cudaMalloc

## 构建/运行所需依赖
[EGL](../../../README.md#egl), [EGLSync](../../../README.md#eglsync), [X11](../../../README.md#x11), [GLES](../../../README.md#gles)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文 [Dependencies]() 部分提到的依赖。

## 参考资料（更多详情）
