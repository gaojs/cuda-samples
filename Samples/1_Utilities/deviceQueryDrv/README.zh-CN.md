# deviceQueryDrv - Driver API 设备查询

## 描述

此示例使用 CUDA Driver API 调用枚举系统中 CUDA 设备的属性。

## 关键概念

CUDA Driver API、设备查询

## 支持的 SM 架构

[SM 5.0](https://developer.nvidia.com/cuda-gpus)  [SM 5.2](https://developer.nvidia.com/cuda-gpus)  [SM 5.3](https://developer.nvidia.com/cuda-gpus)  [SM 6.0](https://developer.nvidia.com/cuda-gpus)  [SM 6.1](https://developer.nvidia.com/cuda-gpus)  [SM 7.0](https://developer.nvidia.com/cuda-gpus)  [SM 7.2](https://developer.nvidia.com/cuda-gpus)  [SM 7.5](https://developer.nvidia.com/cuda-gpus)  [SM 8.0](https://developer.nvidia.com/cuda-gpus)  [SM 8.6](https://developer.nvidia.com/cuda-gpus)  [SM 8.7](https://developer.nvidia.com/cuda-gpus)  [SM 8.9](https://developer.nvidia.com/cuda-gpus)  [SM 9.0](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)

`cuDeviceGetName`、`cuDeviceGetAttribute`、`cuDeviceTotalMem`、`cuDeviceCanAccessPeer`、`cuDeviceGetCount`、`cuDriverGetVersion`、`cuInit`

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)

`cudaSetDevice`

## 前置条件

为对应的平台下载并安装 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。

## 参考资料（更多详情）
