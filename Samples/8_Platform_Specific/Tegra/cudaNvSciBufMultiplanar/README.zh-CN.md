# cudaNvSciBufMultiplanar - CUDA NvSciBuf 多平面图像示例

## 描述

此示例演示 CUDA-NvSciBuf 多平面图像互操作。使用 NvSciBuf API 翻转并分配一幅 YUV 420 多平面图像，然后通过 CUDA External Resource Interoperability 将其导入 CUDA。随后从对应的映射 CUDA array 创建 CUDA surface，并再次对 surface 执行位翻转。结果被复制回 YUV 图像，并与输入图像进行比较。

## 关键概念

CUDA NvSci Interop、数据并行算法、图像处理

## 支持的 SM 架构

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 10.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 10.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 12.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux

## 支持的 CPU 架构

aarch64

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceGetAttribute, cudaNvSciBufMultiplanar, cudaDestroyExternalMemory, cuDriverGetVersion, cuDeviceGetUuid, cudaSetDevice, cudaGetMipmappedArrayLevel, cudaFreeMipmappedArray, cudaImportExternalMemory, cudaCreateChannelDesc, cudaExternalMemoryGetMappedMipmappedArray, cuCtxSynchronize, cudaMemcpy2DToArray, cudaMemcpy2DFromArray

## 构建/运行所需依赖
[NVSCI](../../../README.md#nvsci)

## 前置条件

为对应平台下载并安装 [CUDA Toolkit 12.8](https://developer.nvidia.com/cuda-downloads)。
确保已安装上文 [Dependencies]() 部分提到的依赖。

## 参考资料（更多详情）
