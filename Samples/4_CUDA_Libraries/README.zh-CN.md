# 4. CUDA 库


### [batchCUBLAS](./batchCUBLAS)
一个 CUDA 示例，演示如何使用批量 CUBLAS API 调用来提升整体性能。

### [batchedLabelMarkersAndLabelCompressionNPP](./batchedLabelMarkersAndLabelCompressionNPP)
一个 NPP CUDA 示例，演示如何使用基于并查集（UF）算法的 NPP 标记生成和标记压缩函数，包括单图像和批量图像版本。

### [boxFilterNPP](./boxFilterNPP)
一个 NPP CUDA 示例，演示如何使用 NPP FilterBox 函数执行方框滤波。

### [cannyEdgeDetectorNPP](./cannyEdgeDetectorNPP)
一个 NPP CUDA 示例，演示 nppiFilterCannyBorder_8u_C1R Canny 边缘检测图像滤波函数的推荐参数。此函数需要单通道 8 位灰度输入图像。可以先调用 nppiColorToGray() 或 nppiRGBToGray()，将彩色图像转换为灰度图像。Canny 边缘检测函数结合并改进了通过多个步骤生成边缘检测图像所需的技术。

### [conjugateGradient](./conjugateGradient)
此示例使用 CUBLAS 和 CUSPARSE 库在 GPU 上实现共轭梯度求解器。

### [conjugateGradientCudaGraphs](./conjugateGradientCudaGraphs)
此示例在 GPU 上实现共轭梯度求解器，并使用 CUDA Graph API 捕获和调用 CUBLAS、CUSPARSE 库调用。

### [conjugateGradientMultiBlockCG](./conjugateGradientMultiBlockCG)
此示例在 GPU 上使用多块协作组实现共轭梯度求解器，同时使用统一内存。

### [conjugateGradientMultiDeviceCG](./conjugateGradientMultiDeviceCG)
此示例在多个 GPU 上使用多设备协作组实现共轭梯度求解器，同时使用统一内存，并通过预取和使用提示进行优化。

### [conjugateGradientPrecond](./conjugateGradientPrecond)
此示例使用 CUBLAS 和 CUSPARSE 库在 GPU 上实现预条件共轭梯度求解器。

### [conjugateGradientUM](./conjugateGradientUM)
此示例使用 CUBLAS 和 CUSPARSE 库以及统一内存，在 GPU 上实现共轭梯度求解器。

### [cudaNvSci](./cudaNvSci)
此示例演示 CUDA-NvSciBuf/NvSciSync 互操作。两个 CPU 线程将 NvSciBuf 和 NvSciSync 导入 CUDA，并在 ppm 图像上执行两种图像处理算法：第一个线程旋转图像，第二个线程将旋转后的图像从 rgba 转换为灰度图像。目前仅支持 Ubuntu 18.04。

### [cudaNvSciNvMedia](./cudaNvSciNvMedia)
此示例通过 NvSciBuf/NvSciSync API 演示 CUDA-NvMedia 互操作。请注意，此示例仅支持从 x86_64 到 aarch64 的交叉构建，不支持 aarch64 原生构建。有关示例的详细工作流程，请参阅示例目录中的 cudaNvSciNvMedia_Readme.pdf。

### [cuDLAErrorReporting](./cuDLAErrorReporting)
此示例演示如何通过 CUDA 检测 DLA 错误。

### [cuDLAHybridMode](./cuDLAHybridMode)
此示例演示 cuDLA 混合模式，其中可以使用 CUDA 对 DLA 进行编程。

### [cuDLASt和aloneMode](./cuDLASt和aloneMode)
此示例演示 cuDLA 独立模式，其中无需使用 CUDA 即可对 DLA 进行编程。

### [cuSolverDn_LinearSolver](./cuSolverDn_LinearSolver)
一个演示 cuSolverDN's LU, QR 和 Cholesky 分解.

### [cuSolverRf](./cuSolverRf)
一个演示 cuSolver's re分解 library - CUSOLVERRF.

### [cuSolverSp_LinearSolver](./cuSolverSp_LinearSolver)
一个演示 cuSolverSP's LU, QR 和 Cholesky 分解.

### [cuSolverSp_LowlevelCholesky](./cuSolverSp_LowlevelCholesky)
一个演示 Cholesky 分解 使用 cuSolverSP's low level APIs.

### [cuSolverSp_LowlevelQR](./cuSolverSp_LowlevelQR)
一个演示 QR 分解 使用 cuSolverSP's low level APIs.

### [FilterBorderControlNPP](./FilterBorderControlNPP)
此示例演示如何在最常见的模式下使用启用边界控制的 NPP 滤波函数边界版本。所述函数可用于复现相应 NPP 非边界版本函数的结果。还可以根据源图像作为输入的区域，对源图像的不同边缘启用或禁用边界控制。

### [freeImageInteropNPP](./freeImageInteropNPP)
一个简单的 CUDA 示例，演示 如何使用 FreeImage 库与 NPP。

### [histEqualizationNPP](./histEqualizationNPP)
此 CUDA 示例演示 如何使用 NPP 对图像数据进行直方图均衡化。

### [lineOfSight](./lineOfSight)
此示例实现一个简单的视线算法：给定高度图和从某个观察点发出的射线，计算沿射线从该观察点可见的所有点。该实现基于 Thrust 库。

### [matrixMulCUBLAS](./matrixMulCUBLAS)
此示例实现 矩阵乘法 from Chapter 3 of the programming guide. To illustrate GPU performance for matrix multiply, this sample also shows how to use the new CUDA 4.0 interface for CUBLAS to demonstrate high-performance performance for 矩阵乘法.

### [MersenneTwisterGP11213](./MersenneTwisterGP11213)
此示例演示 cuRAND 中的 Mersenne Twister GP11213 随机数生成器。

### [nvJPEG](./nvJPEG)
一个 CUDA 示例，演示使用 NVJPEG 库对 JPEG 图像进行单张和批量解码。

### [nvJPEG_encoder](./nvJPEG_encoder)
一个 CUDA 示例，演示使用 NVJPEG 库对 JPEG 图像进行单张编码。

### [oceanFFT](./oceanFFT)
此示例使用 CUFFT 生成海洋高度场，并使用 OpenGL 渲染结果。

### [r和omFog](./r和omFog)
此示例展示 CURAND 生成的伪随机数和准随机数。

### [simpleCUBLAS](./simpleCUBLAS)
使用 CUBLAS API 接口执行 GEMM 运算.

### [simpleCUBLAS_LU](./simpleCUBLAS_LU)
CUDA 示例，演示 cuBLAS API cublasDgetrfBatched() 用于矩阵的下三角-上三角（LU）分解。

### [simpleCUBLASXT](./simpleCUBLASXT)
使用 CUBLAS-XT 库，可在多个 GPU 上执行 GEMM 运算。

### [simpleCUFFT](./simpleCUFFT)
使用 CUFFT. 在此示例中，使用 CUFFT 将信号和滤波器转换到频域后相乘，再将信号转换回时域，以计算一维卷积。cuFFT 计划通过简单和高级 API 函数创建。

### [simpleCUFFT_2d_MGPU](./simpleCUFFT_2d_MGPU)
使用 CUFFT. 在此示例中，使用 CUFFT 在多个 GPU 上将信号和滤波器转换到频域后相乘，再将信号转换回时域，以计算二维卷积。

### [simpleCUFFT_callback](./simpleCUFFT_callback)
使用 CUFFT。在此示例中，使用 CUFFT 将信号和滤波器转换到频域后相乘，再将信号转换回时域，以计算一维卷积。此示例与 Simple CUFFT 示例的区别在于：乘法步骤由 CUFFT 内核和用户提供的 CUFFT 回调例程完成，而不是通过单独的内核调用完成。

### [simpleCUFFT_MGPU](./simpleCUFFT_MGPU)
使用 CUFFT。在此示例中，使用 CUFFT 在多个 GPU 上将信号和滤波器转换到频域后相乘，再将信号转换回时域，以计算一维卷积。

### [watershedSegmentationNPP](./watershedSegmentationNPP)
一个 NPP CUDA 示例，演示如何使用 NPP 分水岭分割函数。

