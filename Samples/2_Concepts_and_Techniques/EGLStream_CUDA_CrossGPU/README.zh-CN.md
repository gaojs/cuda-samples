# EGLStream_CUDA_CrossGPU - EGLStream_CUDA_CrossGPU

## 描述

此示例演示 CUDA 与 EGL Streams 的互操作：消费者的 EGL Stream 位于一块 GPU 上，生产者的 EGL Stream 位于另一块 GPU 上，且消费者和生产者分别运行在不同进程中。

## 核心概念

EGLStreams 互操作

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

x86_64, armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetName, cuEGLStreamConsumerReleaseFrame, cuEGLStreamConsumerConnect, cuEGLStreamConsumerDisconnect, cuCtxPushCurrent, cuEGLStreamProducerReturnFrame, cuStreamCreate, cuEGLStreamProducerPresentFrame, cuMemFree, cuGraphicsResourceGetMappedEglFrame, cuInit, cuMemcpyHtoD, cuDeviceGet, cuEGLStreamConsumerAcquireFrame, cuEGLStreamProducerDisconnect, cuEGLStreamProducerConnect, cuDeviceGetAttribute, cuCtxSynchronize, cuMemAlloc, cuCtxPopCurrent, cuCtxCreate

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaMalloc, cudaProducerPresentFrame, cudaFree, cudaGetErrorString, cudaConsumerReleaseFrame, cudaProducerReturnFrame, cudaDeviceSynchronize, cudaDeviceCreateProducer, cudaProducerDeinit, cudaProducerPrepareFrame, cudaGetValueMismatch, cudaConsumerAcquireFrame, cudaProducerInit, cudaDeviceCreateConsumer

## 构建/运行所需依赖
[EGL](../../../README.md#egl)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上文 [Dependencies]() 一节中提到的依赖。

## 参考资料（详情）

