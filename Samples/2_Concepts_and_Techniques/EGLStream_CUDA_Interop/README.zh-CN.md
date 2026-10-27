# EGLStream_CUDA_Interop - EGLStream CUDA 互操作

## 描述

演示 CUDA 与 EGL Streams 之间的数据交换。

## 核心概念

EGLStreams 互操作

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

x86_64, aarch64

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuDeviceGetName, cuEGLStreamConsumerReleaseFrame, cuEGLStreamConsumerConnect, cuEGLStreamConsumerDisconnect, cuCtxPushCurrent, cuArrayDestroy, cuEGLStreamProducerReturnFrame, cuEGLStreamProducerPresentFrame, cuMemFree, cuGraphicsResourceGetMappedEglFrame, cuInit, cuEGLStreamConsumerAcquireFrame, cuEGLStreamProducerDisconnect, cuDeviceGetCount, cuEGLStreamProducerConnect, cuDeviceGetAttribute, cuCtxSynchronize, cuMemAlloc, cuCtxPopCurrent, cuCtxCreate, cuMemcpy

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaProducerReadYUVFrame, cudaProducerTest, cudaProducerDeinit, cudaDeviceCreateProducer, cudaProducerReadARGBFrame, cudaDeviceCreateConsumer, cudaConsumerTest, cudaProducerInit

## 构建/运行所需依赖
[EGL](../../../README.md#egl)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads) 。
请确保已安装上文 [依赖项]() 一节中提到的依赖。

## 参考资料（详情）
