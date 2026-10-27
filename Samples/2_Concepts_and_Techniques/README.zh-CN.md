# 2. 概念与技术


### [boxFilter](./boxFilter)
使用 CUDA 实现快速图像盒式滤波，并通过 OpenGL 渲染。

### [convolutionSeparable](./convolutionSeparable)
此示例使用高斯核对二维信号实现可分离卷积滤波器。

### [convolutionTexture](./convolutionTexture)
使用纹理实现基于高斯核的二维可分离卷积，用于与 convolutionSeparable 进行性能比较。

### [cuHook](./cuHook)
此示例演示如何使用 CUDA 构建和使用拦截库。该库必须通过 LD_PRELOAD 加载，例如 LD_PRELOAD=<full_path>/libcuhook.so.1 ./cuHook

### [dct8x8](./dct8x8)
此示例演示如何使用 CUDA 对 8×8 像素块执行离散余弦变换（DCT）：包括按定义实现的朴素方法，以及许多库中使用的更传统方法。与在片段着色器中实现 DCT 相比，CUDA 能够以更简单、高效的方式实现。

### [EGLStream_CUDA_CrossGPU](./EGLStream_CUDA_CrossGPU)
演示 CUDA 与 EGL Streams 的互操作：消费者的 EGL Stream 位于一块 GPU 上，生产者的 EGL Stream 位于另一块 GPU 上，且消费者和生产者分别属于不同进程。

### [EGLStream_CUDA_Interop](./EGLStream_CUDA_Interop)
演示 CUDA 与 EGL Streams 之间的数据交换。

### [EGLSync_CUDAEvent_Interop](./EGLSync_CUDAEvent_Interop)
演示 CUDA Event 与 EGL Sync/EGL Image 之间的互操作，从而可在 GPU 上实现 GL-EGL-CUDA 操作的同步，而无需阻塞 CPU 进行同步。

### [eigenvalues](./eigenvalues)
计算全部或部分特征值是线性代数、统计学、物理学及许多其他领域中的重要问题。此示例演示使用 CUDA 并行实现二分算法，以计算任意大小对称三对角矩阵的全部特征值。

### [FunctionPointers](./FunctionPointers)
此示例说明如何使用函数指针，并为 8 位单色图像实现 Sobel 边缘检测滤波器。

### [histogram](./histogram)
此示例演示 64 桶和 256 桶直方图的高效实现。

### [imageDenoising](./imageDenoising)
此示例演示两种自适应图像去噪技术：KNN 和 NLM，它们基于纹素之间几何距离和颜色距离的计算。虽然这两种技术都通过着色器在 DirectX SDK 中实现，但此示例还实现了充分利用共享内存、速度大幅提升的后一种技术变体，与 DirectX 实现相对应。

### [inlinePTX](./inlinePTX)
这是一个简单的测试应用，演示 CUDA 4.0 引入的在 CUDA 内核中嵌入 PTX 的功能。

### [inlinePTX_nvrtc](./inlinePTX_nvrtc)
这是一个简单的测试应用，演示 CUDA 4.0 引入的在 CUDA 内核中嵌入 PTX 的功能。

### [interval](./interval)
区间算术运算符示例。使用了多种 C++ 特性（模板和递归）。递归模式需要计算能力 SM 2.0。

### [MC_EstimatePiInlineP](./MC_EstimatePiInlineP)
此示例使用蒙特卡洛模拟估算圆周率（使用内联 PRNG）。此示例还使用 NVIDIA CURAND 库。

### [MC_EstimatePiInlineQ](./MC_EstimatePiInlineQ)
此示例使用蒙特卡洛模拟估算圆周率（使用内联 QRNG）。此示例还使用 NVIDIA CURAND 库。

### [MC_EstimatePiP](./MC_EstimatePiP)
此示例使用蒙特卡洛模拟估算圆周率（使用批量 PRNG）。此示例还使用 NVIDIA CURAND 库。

### [MC_EstimatePiQ](./MC_EstimatePiQ)
此示例使用蒙特卡洛模拟估算圆周率（使用批量 QRNG）。此示例还使用 NVIDIA CURAND 库。

### [MC_SingleAsianOptionP](./MC_SingleAsianOptionP)
此示例使用 NVIDIA CURAND 库通过蒙特卡洛方法模拟单一亚式期权。

### [particles](./particles)
此示例使用 CUDA 模拟并可视化大量粒子及其物理交互。在命令行中添加 "-particles=<N>" 可设置要模拟的粒子数量。此示例实现了均匀网格数据结构，可使用原子操作或 Thrust 库中的快速基数排序。

### [radixSortThrust](./radixSortThrust)
此示例演示使用 Thrust 库实现的快速高效并行基数排序。所含的 RadixSort 类既可对键值对排序（键为浮点数或无符号整数），也可仅对键排序。

### [reduction](./reduction)
并行求和归约，用于计算大型数组中各值的总和。此示例演示数据并行算法（如归约）的若干重要优化策略，包括使用共享内存、__shfl_down_sync、__reduce_add_sync 和 cooperative_groups reduce。

### [reductionMultiBlockCG](./reductionMultiBlockCG)
此示例演示使用 Multi Block Cooperative Groups 执行单遍归约。此示例要求设备的计算能力为 6.0 或更高，并支持计算抢占。

### [scalarProd](./scalarProd)
此示例计算给定输入向量对的标量积。

### [scan](./scan)
此示例演示 CUDA 并行前缀和的高效实现，也称为“scan”。给定一个数值数组，scan 会计算一个新数组，其中每个元素是输入数组中该元素之前所有元素的和。

### [segmentationTreeThrust](./segmentationTreeThrust)
此示例演示一种构建图像分割树的方法，该方法基于 Borůvka 最小生成树算法。

### [shfl_scan](./shfl_scan)
此示例演示如何使用 shuffle intrinsic __shfl_up_sync 在一个线程块内执行扫描操作。

### [sortingNetworks](./sortingNetworks)
此示例实现双调排序和奇偶归并排序（也称 Batcher 排序），这两种算法都属于排序网络。与归并排序或基数排序等渐近复杂度更优的算法相比，排序网络通常效率较低；但在对大量短到中等长度的（键、值）数组对进行批量排序时，它可能是更合适的选择。请参阅 H. W. Lang 编写的优秀教程 http://www.iti.fh-flensburg.de/lang/algorithmen/sortieren/networks/indexen.htm

### [streamOrderedAllocation](./streamOrderedAllocation)
此示例演示如何使用 cudaMallocAsync 和 cudaMemPool 系列 API 在 GPU 上执行流序内存分配。

### [streamOrderedAllocationIPC](./streamOrderedAllocationIPC)
此示例演示使用 cudaMallocAsync 和 cudaMemPool 系列 API 分配的流序内存 IPC 池。

### [streamOrderedAllocationP2P](./streamOrderedAllocationP2P)
此示例演示对使用 cudaMallocAsync 和 cudaMemPool 系列 API 分配的流序内存进行点对点访问。

### [threadFenceReduction](./threadFenceReduction)
此示例展示如何使用线程 Fence intrinsic 对数组中的值执行归约，在单个内核中得到单个结果（而不是像 CUDA Sample “reduction” 中那样调用两个或更多内核）。单遍归约需要全局原子指令（计算能力 2.0 或更高）和 _threadfence() intrinsic（CUDA 2.2 或更高）。

### [threadMigration](./threadMigration)
这是一个简单程序，演示 CUDA Context Management API，并使用 CUDA 4.0 新增的参数传递和 CUDA 启动 API。CUDA 上下文可单独创建，并分别附加到不同线程。
