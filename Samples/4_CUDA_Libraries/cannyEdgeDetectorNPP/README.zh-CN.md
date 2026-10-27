# cannyEdgeDetectorNPP - Canny Edge Detector NPP

## 描述

一个 NPP CUDA 示例，演示 nppiFilterCannyBorder_8u_C1R Canny 边缘检测图像滤波函数的推荐参数。此函数需要单通道 8 位灰度输入图像。可以先调用 nppiColorToGray() 或 nppiRGBToGray()，将彩色图像转换为灰度图像。Canny 边缘检测函数结合并改进了通过多个步骤生成边缘检测图像所需的技术。

## 核心概念

性能策略, 图像处理, NPP 库

## 支持的 SM 架构

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## 支持的操作系统

Linux、Windows

## 支持的 CPU 架构

x86_64、armv7l

## 涉及的 CUDA API

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaRuntimeGetVersion, cudaFree, cudaSetDevice, cudaGetDeviceCount, cudaDeviceInit, cudaDriverGetVersion, cudaMalloc, cudaGetDeviceProperties

## 构建/运行所需的依赖项
[FreeImage](../../../README.md#freeimage), [NPP](../../../README.md#npp)

## 前置条件

下载并安装适用于相应平台的 [CUDA Toolkit 12.5](https://developer.nvidia.com/cuda-downloads)。
请确保已安装上文[依赖项]()部分中提到的依赖项。

## 参考资料（更多详情）
