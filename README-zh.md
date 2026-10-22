# CUDA Samples（CUDA 示例）

CUDA Samples 是面向 CUDA 开发者的示例集合，用于演示 CUDA Toolkit 的功能。本版本支持 [CUDA Toolkit 13.4](https://developer.nvidia.com/cuda-downloads)。原英文文档见 [README.md](./README.md)。

## 发布说明

本节说明 GitHub 上 CUDA Samples 的发布信息。变更记录见 [CHANGELOG.md](./CHANGELOG.md)，修订历史同样参见 [Revision History](./CHANGELOG.md)。

## 开始使用

### 前置条件

请为目标平台下载并安装 [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)。系统要求和安装方式请参考 [Linux 安装指南](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/) 与 [Windows 安装指南](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html)。

### 获取 CUDA Samples

```bash
git clone https://github.com/NVIDIA/cuda-samples.git
```

也可以在仓库页面选择 **Download ZIP**，解压后直接使用。

## 构建 CUDA Samples

示例使用 CMake 构建。除非特别说明，顶层构建默认编译所有受本次发布支持的 GPU 架构。

### Linux

需要 CMake 3.20 或更高版本，例如：

```bash
sudo apt install cmake
mkdir build && cd build
cmake ..
make -j$(nproc)
```

如只面向一个 GPU 架构，可指定其 SM 版本以缩短构建时间，例如 `89 `表示 `sm_89`：

```bash
cmake -DCMAKE_CUDA_ARCHITECTURES=89 ..
```

构建后可从 `build` 中各示例对应的目录运行程序。

### Windows

Visual Studio 2019 16.5 或更高版本提供 CMake 语言服务，可从仓库根目录、分类目录或单个示例目录导入。命令行构建时打开 Visual Studio 提供的 `x64 Native Tools Command Prompt for VS`：

```cmd
mkdir build && cd build
cmake .. -G "Visual Studio 16 2019" -A x64
```

随后在 Visual Studio 中打开生成的 `CUDA_Samples.sln`，选择 Debug 或 Release，按 F7 构建；从 Visual Studio 指定的输出目录运行示例。

### 构建单个示例

单独构建时必须显式指定 GPU 架构：

```bash
cd cpp/1_Utilities/deviceQuery
mkdir -p build && cd build
cmake -DCMAKE_CUDA_ARCHITECTURES=89 ..
make
```

### Tegra、DriveOS 与平台专用示例

平台专用示例默认不启用。使用 `-DBUILD_TEGRA=True` 构建 Tegra 示例：

```bash
cmake -DBUILD_TEGRA=True ..
```

交叉编译 Tegra 时，安装 NVIDIA 工具链并使用：

```bash
cmake .. \
  -DCMAKE_TOOLCHAIN_FILE=../cmake/toolchains/toolchain-aarch64-linux.cmake \
  -DTARGET_FS=/path/to/target/system/file/system
make -j$(nproc)
```

其中 `TARGET_FS` 指向目标文件系统，构建出的程序需传输到 Tegra 设备执行。DriveOS Docker 容器构建方式、挂载 RFS、Vulkan/libdrm 缺失时的处理方式，请参见 [原英文说明](./README.md#cross-building-for-automotive-linux-platforms-from-the-driveos-docker-containers)。

### QNX

CUDA 13.0 及更新版本支持用 CMake 为 QNX 交叉编译。先设置 QNX SDP：

```bash
export QNX_HOST=/path/to/qnx/host
export QNX_TARGET=/path/to/qnx/target
mkdir -p build && cd build
cmake .. \
  -DBUILD_TEGRA=True \
  -DCMAKE_CUDA_COMPILER=/usr/local/cuda-13.3/bin/nvcc \
  -DCMAKE_TOOLCHAIN_FILE=../cmake/toolchains/toolchain-aarch64-qnx.cmake \
  -DTARGET_FS=/path/to/qnx/targetfs \
  -DCMAKE_LIBRARY_PATH=/usr/local/cuda-13.3/thor/targets/aarch64-qnx/lib/stubs/ \
  -DCMAKE_INCLUDE_PATH=/usr/local/cuda-13.3/thor/targets/aarch64-qnx/include/
cmake --build .
```

`TARGET_FS` 是 QNX 板卡文件系统；`cudaNvSci` 需要其中的 NvSci 头文件和库。若布局不同，可设置 `NVSCIBUF_INCLUDE_DIR`、`NVSCISYNC_INCLUDE_DIR`、`NVSCIBUF_LIBRARY`、`NVSCISYNC_LIBRARY`。QNX Safety 使用 CUDA Safe 工具包和 `toolchain-aarch64-qnx-safe.cmake`；支持的 Safety 示例为 `matrixMul` 与 `cudaNvSci`。

### GPU 端调试

GPU 端调试由 `cuda-gdb` 支持，但会因关闭部分编译优化而显著影响性能。配置时启用：

```bash
cmake -DENABLE_CUDA_DEBUG=True ...
```

### 向前兼容

使用 CUDA Toolkit 13.0+、UMD 580+ 与旧 KMD 550 或更早版本时，可指定新驱动存根库：

```bash
cmake -DCMAKE_PREFIX_PATH=/usr/local/cuda/lib64/stubs/ ..
```

## CUDA Python 示例

仓库还包含 [`python/`](./python) 下的 Python 示例，重点使用 [CUDA Python](https://nvidia.github.io/cuda-python/) 和 [`cuda.core`](https://nvidia.github.io/cuda-python/cuda-core/latest/)，并按示例与 NumPy、CuPy、PyTorch 或 TensorFlow 互操作。

| 目录                                                                   | 功能概览                                                                                |
| -------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [`python/1_GettingStarted/`](./python/1_GettingStarted/)             | 入门脚本：`vectorAdd`、`deviceQuery`、`systemInfo`、统一内存图像模糊、NumPy/CuPy 对比等                 |
| [`python/2_CoreConcepts/`](./python/2_CoreConcepts/)                 | 归约、直方图、FFT、流重叠、`memoryResources`、`cudaGraphs`、`jitLtoLinking`、`tmaTensorMap` 等算法与技术 |
| [`python/3_FrameworkInterop/`](./python/3_FrameworkInterop/)         | PyTorch、TensorFlow 框架互操作                                                            |
| [`python/4_DistributedComputing/`](./python/4_DistributedComputing/) | 多 GPU、P2P 与 IPC（包括 `ipcMemoryPool`）                                                 |
| [`python/Utilities/`](./python/Utilities/)                           | 示例共用的辅助工具                                                                           |

顶层 CMake 不会编译 Python 示例。每个示例使用 Python 3.10+ 环境及匹配的 CUDA Toolkit，并按其目录中的 `requirements.txt` 安装依赖：

```bash
cd python/<分类>/<示例名>
pip install -r requirements.txt
python <示例脚本>.py
```

具体前置条件、命令行选项和预期输出请以各示例自己的 `README.md` 为准。

## 示例目录与功能总览

下面按 [C++ 示例分类](./cpp) 列出分类 README 中的全部条目。示例名链接保持指向原目录，描述为中文概览；平台或第三方依赖以示例目录中的说明为准。

### [0. Introduction](./cpp/0_Introduction/README.md)

基础 CUDA Runtime/Driver API、内存、线程、流、纹理和多 GPU 入门示例。

- [asyncAPI](./cpp/0_Introduction/asyncAPI)：用 CUDA Event 计时，并重叠 CPU/GPU 执行与异步拷贝。
- [clock](./cpp/0_Introduction/clock)：用 `clock` 精确测量 kernel 线程块性能。
- [cudaOpenMP](./cpp/0_Introduction/cudaOpenMP)：使用 OpenMP 编写多 GPU 应用。
- [fp16ScalarProduct](./cpp/0_Introduction/fp16ScalarProduct)：计算两个 FP16 向量的内积。
- [matrixMul](./cpp/0_Introduction/matrixMul)：基础矩阵乘法，并对比 cuBLAS 高性能实现。
- [matrixMulDrv](./cpp/0_Introduction/matrixMulDrv)：使用 CUDA Driver API 启动矩阵乘法 kernel。
- [matrixMulDynlinkJIT](./cpp/0_Introduction/matrixMulDynlinkJIT)：运行时链接 Driver API，并从 PTX JIT 编译矩阵乘法。
- [simpleAssert](./cpp/0_Introduction/simpleAssert)：在设备代码中使用 `assert`。
- [simpleAtomicIntrinsics](./cpp/0_Introduction/simpleAtomicIntrinsics)：全局内存原子指令。
- [simpleAttributes](./cpp/0_Introduction/simpleAttributes)：使用影响 L2 局部性的 stream 属性。
- [simpleAWBarrier](./cpp/0_Introduction/simpleAWBarrier)：到达-等待（arrive-wait）屏障。
- [simpleCallback](./cpp/0_Introduction/simpleCallback)：CUDA stream/event 的 CPU 回调与异构多线程工作负载。
- [simpleCooperativeGroups](./cpp/0_Introduction/simpleCooperativeGroups)：线程块内 Cooperative Groups。
- [simpleCubemapTexture](./cpp/0_Introduction/simpleCubemapTexture)：立方体纹理。
- [simpleCUDA2GL](./cpp/0_Introduction/simpleCUDA2GL)：高效地把 CUDA 图像复制到 OpenGL。
- [simpleDrvRuntime](./cpp/0_Introduction/simpleDrvRuntime)：Driver/Runtime API 协同加载 fatbinary 并做向量加法。
- [simpleIPC](./cpp/0_Introduction/simpleIPC)：每 GPU 一个进程的 CUDA 进程间通信。
- [simpleLayeredTexture](./cpp/0_Introduction/simpleLayeredTexture)：分层纹理。
- [simpleMPI](./cpp/0_Introduction/simpleMPI)：MPI 与 CUDA 结合。
- [simpleMultiCopy](./cpp/0_Introduction/simpleMultiCopy)：用 streams 重叠 kernel 和主机到/从设备的数据拷贝。
- [simpleMultiGPU](./cpp/0_Introduction/simpleMultiGPU)：多线程、多 GPU 上下文和 kernel 执行。
- [simpleP2P](./cpp/0_Introduction/simpleP2P)：多 GPU P2P 拷贝、寻址和 UVA。
- [simplePitchLinearTexture](./cpp/0_Introduction/simplePitchLinearTexture)：pitch-linear 纹理。
- [simplePrintf](./cpp/0_Introduction/simplePrintf)：设备代码中的 `printf`。
- [simpleStreams](./cpp/0_Introduction/simpleStreams)：用 streams 重叠 kernel 与内存拷贝。
- [simpleSurfaceWrite](./cpp/0_Introduction/simpleSurfaceWrite)：二维 surface 引用写入纹理。
- [simpleTexture](./cpp/0_Introduction/simpleTexture)：CUDA 纹理基础。
- [simpleTexture3D](./cpp/0_Introduction/simpleTexture3D)：三维纹理。
- [simpleTextureDrv](./cpp/0_Introduction/simpleTextureDrv)：通过 Driver API 启动纹理示例。
- [simpleVoteIntrinsics](./cpp/0_Introduction/simpleVoteIntrinsics)：`__any_sync`、`__all_sync` vote intrinsic。
- [simpleZeroCopy](./cpp/0_Introduction/simpleZeroCopy)：kernel 直接读写 pinned 主机内存。
- [systemWideAtomics](./cpp/0_Introduction/systemWideAtomics)：系统范围原子指令。
- [UnifiedMemoryStreams](./cpp/0_Introduction/UnifiedMemoryStreams)：单 GPU 上结合 OpenMP、streams 与统一内存。
- [vectorAdd](./cpp/0_Introduction/vectorAdd)：最基础的逐元素向量加法。
- [vectorAdd\_nvrtc](./cpp/0_Introduction/vectorAdd_nvrtc)：使用 NVRTC 运行时编译向量加法 kernel。
- [vectorAddDrv](./cpp/0_Introduction/vectorAddDrv)：使用 Driver API 启动向量加法。
- [vectorAddMMAP](./cpp/0_Introduction/vectorAddMMAP)：用 `cuMemMap` 分配替代传统设备分配。

### [1. Utilities](./cpp/1_Utilities/README.md)

- [deviceQuery](./cpp/1_Utilities/deviceQuery)：枚举 CUDA 设备属性。
- [deviceQueryDrv](./cpp/1_Utilities/deviceQueryDrv)：通过 Driver API 枚举设备属性。
- [topologyQuery](./cpp/1_Utilities/topologyQuery)：查询多 GPU 系统拓扑。
- `bandwidthTest`：CUDA Samples 12.9 起已移除；带宽测量请使用 [NVBandwidth](https://github.com/nvidia/nvbandwidth)。

### [2. Concepts and Techniques](./cpp/2_Concepts_and_Techniques/README.md)

并行算法、图像处理、纹理、同步、内存池和常用 CUDA 技术。

- [boxFilter](./cpp/2_Concepts_and_Techniques/boxFilter)：CUDA 快速图像 box filter 与 OpenGL 渲染。
- [convolutionSeparable](./cpp/2_Concepts_and_Techniques/convolutionSeparable)：二维高斯可分离卷积。
- [convolutionTexture](./cpp/2_Concepts_and_Techniques/convolutionTexture)：基于纹理的高斯可分离卷积并用于性能比较。
- [dct8x8](./cpp/2_Concepts_and_Techniques/dct8x8)：8×8 像素块离散余弦变换。
- [EGLStream\_CUDA\_CrossGPU](./cpp/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU)：跨 GPU、跨进程 CUDA/EGL Stream 互操作。
- [EGLStream\_CUDA\_Interop](./cpp/2_Concepts_and_Techniques/EGLStream_CUDA_Interop)：CUDA 与 EGL Stream 数据交换。
- [eigenvalues](./cpp/2_Concepts_and_Techniques/eigenvalues)：三对角对称矩阵特征值的并行二分算法。
- [FunctionPointers](./cpp/2_Concepts_and_Techniques/FunctionPointers)：设备函数指针与 Sobel 边缘检测。
- [histogram](./cpp/2_Concepts_and_Techniques/histogram)：64/256 桶高效直方图。
- [imageDenoising](./cpp/2_Concepts_and_Techniques/imageDenoising)：KNN 与 NLM 自适应图像去噪。
- [inlinePTX](./cpp/2_Concepts_and_Techniques/inlinePTX)：在 CUDA kernel 中嵌入 PTX。
- [interval](./cpp/2_Concepts_and_Techniques/interval)：区间算术、模板与递归。
- [MC\_EstimatePiInlineP](./cpp/2_Concepts_and_Techniques/MC_EstimatePiInlineP)：内联伪随机数 Monte Carlo 估算 π。
- [MC\_EstimatePiInlineQ](./cpp/2_Concepts_and_Techniques/MC_EstimatePiInlineQ)：内联拟随机数 Monte Carlo 估算 π。
- [MC\_EstimatePiP](./cpp/2_Concepts_and_Techniques/MC_EstimatePiP)：批量伪随机数 Monte Carlo 估算 π。
- [MC\_EstimatePiQ](./cpp/2_Concepts_and_Techniques/MC_EstimatePiQ)：批量拟随机数 Monte Carlo 估算 π。
- [MC\_SingleAsianOptionP](./cpp/2_Concepts_and_Techniques/MC_SingleAsianOptionP)：用 CURAND Monte Carlo 模拟亚洲期权。
- [particles](./cpp/2_Concepts_and_Techniques/particles)：粒子物理模拟、均匀网格及原子操作/基数排序。
- [radixSortThrust](./cpp/2_Concepts_and_Techniques/radixSortThrust)：使用 Thrust 的并行基数排序。
- [reduction](./cpp/2_Concepts_and_Techniques/reduction)：共享内存、warp shuffle、同步归约和 Cooperative Groups 归约。
- [reductionMultiBlockCG](./cpp/2_Concepts_and_Techniques/reductionMultiBlockCG)：多 block Cooperative Groups 单次归约。
- [scalarProd](./cpp/2_Concepts_and_Techniques/scalarProd)：输入向量对的标量积。
- [scan](./cpp/2_Concepts_and_Techniques/scan)：并行前缀和（scan）。
- [segmentationTreeThrust](./cpp/2_Concepts_and_Techniques/segmentationTreeThrust)：基于 Borůvka MST 的图像分割树。
- [shfl\_scan](./cpp/2_Concepts_and_Techniques/shfl_scan)：用 `__shfl_up_sync` 跨线程块执行 scan。
- [sortingNetworks](./cpp/2_Concepts_and_Techniques/sortingNetworks)：bitonic sort 与 odd-even merge sort。
- [streamOrderedAllocation](./cpp/2_Concepts_and_Techniques/streamOrderedAllocation)：`cudaMallocAsync`/内存池的 stream 有序分配。
- [streamOrderedAllocationIPC](./cpp/2_Concepts_and_Techniques/streamOrderedAllocationIPC)：stream 有序内存池的 IPC。
- [streamOrderedAllocationP2P](./cpp/2_Concepts_and_Techniques/streamOrderedAllocationP2P)：stream 有序内存的 P2P 访问。
- [threadFenceReduction](./cpp/2_Concepts_and_Techniques/threadFenceReduction)：使用 thread fence 和原子操作的单 kernel 归约。
- [threadMigration](./cpp/2_Concepts_and_Techniques/threadMigration)：CUDA 上下文管理与线程间迁移。

### [3. CUDA Features](./cpp/3_CUDA_Features/README.md)

- [bf16TensorCoreGemm](./cpp/3_CUDA_Features/bf16TensorCoreGemm)：BF16 Tensor Core GEMM、WMMA 与异步拷贝。
- [binaryPartitionCG](./cpp/3_CUDA_Features/binaryPartitionCG)：binary partition Cooperative Groups 与 block 内归约。
- [bindlessTexture](./cpp/3_CUDA_Features/bindlessTexture)：surface/texture object 与 mipmap。
- [cdpAdvancedQuicksort](./cpp/3_CUDA_Features/cdpAdvancedQuicksort)：CUDA Dynamic Parallelism 高级快速排序。
- [cdpBezierTessellation](./cpp/3_CUDA_Features/cdpBezierTessellation)：动态并行 Bézier 曲线细分。
- [cdpQuadtree](./cpp/3_CUDA_Features/cdpQuadtree)：动态并行四叉树。
- [cdpSimplePrint](./cpp/3_CUDA_Features/cdpSimplePrint)：动态并行中的 `printf`。
- [cdpSimpleQuicksort](./cpp/3_CUDA_Features/cdpSimpleQuicksort)：动态并行简单快速排序。
- [cudaCompressibleMemory](./cpp/3_CUDA_Features/cudaCompressibleMemory)：通过 `cuMemMap` 分配可压缩内存。
- [cudaTensorCoreGemm](./cpp/3_CUDA_Features/cudaTensorCoreGemm)：Volta Tensor Core WMMA GEMM 与扩展动态共享内存。
- [dmmaTensorCoreGemm](./cpp/3_CUDA_Features/dmmaTensorCoreGemm)：双精度 Tensor Core GEMM、异步拷贝和 Cooperative Groups 拷贝。
- [globalToShmemAsyncCopy](./cpp/3_CUDA_Features/globalToShmemAsyncCopy)：全局到共享内存异步拷贝及 arrive-wait 屏障。
- [graphMemoryFootprint](./cpp/3_CUDA_Features/graphMemoryFootprint)：CUDA Graph 内虚拟地址与物理内存复用。
- [graphMemoryNodes](./cpp/3_CUDA_Features/graphMemoryNodes)：Graph/Stream Capture 中的内存分配与释放节点。
- [immaTensorCoreGemm](./cpp/3_CUDA_Features/immaTensorCoreGemm)：整数 Tensor Core WMMA GEMM。
- [jacobiCudaGraphs](./cpp/3_CUDA_Features/jacobiCudaGraphs)：Jacobi 迭代中的 CUDA Graph 实例更新。
- [localityDomains](./cpp/3_CUDA_Features/localityDomains)：Runtime API 的局部化 green context 和内存池。
- [localityDomainsDrv](./cpp/3_CUDA_Features/localityDomainsDrv)：Driver API 的局部化上下文和内存。
- [memMapIPCDrv](./cpp/3_CUDA_Features/memMapIPCDrv)：使用 `cuMemMap` 的 Driver API IPC。
- [newdelete](./cpp/3_CUDA_Features/newdelete)：设备 C++ `new`/`delete` 与虚函数。
- [ptxjit](./cpp/3_CUDA_Features/ptxjit)：Driver API 从 PTX JIT 编译并链接 kernel。
- [simpleCudaGraphs](./cpp/3_CUDA_Features/simpleCudaGraphs)：CUDA Graph 创建、实例化、捕获和启动。
- [StreamPriorities](./cpp/3_CUDA_Features/StreamPriorities)：stream 优先级。
- [tf32TensorCoreGemm](./cpp/3_CUDA_Features/tf32TensorCoreGemm)：TF32 Tensor Core GEMM 与异步拷贝。
- [warpAggregatedAtomicsCG](./cpp/3_CUDA_Features/warpAggregatedAtomicsCG)：用 Cooperative Groups 聚合 warp 原子操作。
- [graphConditionalNodes](./cpp/3_CUDA_Features/graphConditionalNodes)：CUDA 12.4 起支持的 Graph 条件节点。

### [4. CUDA Libraries](./cpp/4_CUDA_Libraries/README.md)

- [conjugateGradientCudaGraphs](./cpp/4_CUDA_Libraries/conjugateGradientCudaGraphs)：用 cuBLAS/cuSPARSE 与 CUDA Graph 求解共轭梯度。
- [conjugateGradientMultiBlockCG](./cpp/4_CUDA_Libraries/conjugateGradientMultiBlockCG)：多 block Cooperative Groups 共轭梯度与统一内存。
- [conjugateGradientMultiDeviceCG](./cpp/4_CUDA_Libraries/conjugateGradientMultiDeviceCG)：多 GPU Cooperative Groups 共轭梯度。
- [cudaNvSci](./cpp/4_CUDA_Libraries/cudaNvSci)：CUDA 与 NvSciBuf/NvSciSync 互操作图像处理。
- [lineOfSight](./cpp/4_CUDA_Libraries/lineOfSight)：基于 Thrust 的高度图视线算法。
- [oceanFFT](./cpp/4_CUDA_Libraries/oceanFFT)：用 CUFFT 模拟海洋高度场并用 OpenGL 渲染。
- [randomFog](./cpp/4_CUDA_Libraries/randomFog)：CURAND 伪随机数与拟随机数。

### [5. Domain Specific](./cpp/5_Domain_Specific/README.md)

图像、金融、科学计算及 CUDA 图形互操作示例。

- [bicubicTexture](./cpp/5_Domain_Specific/bicubicTexture)：纹理实现高效双三次 B-spline 插值。
- [bilateralFilter](./cpp/5_Domain_Specific/bilateralFilter)：保边双边滤波与 OpenGL 渲染。
- [binomialOptions](./cpp/5_Domain_Specific/binomialOptions)：二叉树模型的欧式期权定价。
- [BlackScholes](./cpp/5_Domain_Specific/BlackScholes)：Black-Scholes 欧式看涨/看跌期权定价。
- [convolutionFFT2D](./cpp/5_Domain_Specific/convolutionFFT2D)：FFT 实现大卷积核二维卷积。
- [dwtHaar1D](./cpp/5_Domain_Specific/dwtHaar1D)：一维 Haar 小波分解。
- [dxtc](./cpp/5_Domain_Specific/dxtc)：CUDA 并行高质量 DXT 压缩。
- [fastWalshTransform](./cpp/5_Domain_Specific/fastWalshTransform)：批量 Fast Walsh/Hadamard 变换。
- [FDTD3d](./cpp/5_Domain_Specific/FDTD3d)：三维有限差分时域 stencil。
- [fluidsGL](./cpp/5_Domain_Specific/fluidsGL)：CUDA/CUFFT 流体模拟与 OpenGL 渲染。
- [HSOpticalFlow](./cpp/5_Domain_Specific/HSOpticalFlow)：Horn-Schunck 变分光流估计。
- [Mandelbrot](./cpp/5_Domain_Specific/Mandelbrot)：交互式 Mandelbrot/Julia 集与高精度缩放。
- [marchingCubes](./cpp/5_Domain_Specific/marchingCubes)：marching cubes 体数据等值面提取。
- [MonteCarloMultiGPU](./cpp/5_Domain_Specific/MonteCarloMultiGPU)：利用所有 GPU 的 Monte Carlo 期权定价。
- [nbody](./cpp/5_Domain_Specific/nbody)：多 GPU 引力 N-body 模拟。
- [nbody\_screen](./cpp/5_Domain_Specific/nbody_screen)：无交互界面的 N-body 模拟。
- [NV12toBGRandResize](./cpp/5_Domain_Specific/NV12toBGRandResize)：批量 NV12 转 BGR planar 并缩放。
- [postProcessGL](./cpp/5_Domain_Specific/postProcessGL)：CUDA 后处理 OpenGL 渲染图像。
- [quasirandomGenerator](./cpp/5_Domain_Specific/quasirandomGenerator)：Niederreiter 拟随机序列与正态分布生成。
- [recursiveGaussian](./cpp/5_Domain_Specific/recursiveGaussian)：Deriche 递归高斯模糊。
- [simpleD3D11](./cpp/5_Domain_Specific/simpleD3D11)：CUDA/D3D11 外部资源互操作与 keyed mutex 同步。
- [simpleD3D11Texture](./cpp/5_Domain_Specific/simpleD3D11Texture)：CUDA 写入 D3D11 二维、三维和立方体纹理。
- [simpleD3D12](./cpp/5_Domain_Specific/simpleD3D12)：CUDA/D3D12 vertex buffer 与 fence 互操作。
- [simpleGL](./cpp/5_Domain_Specific/simpleGL)：CUDA 修改顶点并由 OpenGL 渲染。
- [simpleGLES\_screen](./cpp/5_Domain_Specific/simpleGLES_screen)：CUDA 与 OpenGL ES 图形互操作。
- [simpleVulkan](./cpp/5_Domain_Specific/simpleVulkan)：Vulkan vertex buffer、CUDA kernel 与 semaphore 互操作。
- [simpleVulkanMMAP](./cpp/5_Domain_Specific/simpleVulkanMMAP)：使用 `cuMemMap` 的 Vulkan/CUDA 互操作。
- [smokeParticles](./cpp/5_Domain_Specific/smokeParticles)：半角切片体积阴影烟雾模拟。
- [SobelFilter](./cpp/5_Domain_Specific/SobelFilter)：8 位灰度图 Sobel 边缘检测。
- [SobolQRNG](./cpp/5_Domain_Specific/SobolQRNG)：Sobol 拟随机数生成器。
- [stereoDisparity](./cpp/5_Domain_Specific/stereoDisparity)：用 SIMD SAD 计算立体视差图。
- [volumeFiltering](./cpp/5_Domain_Specific/volumeFiltering)：三维纹理与 surface 写入的体积滤波。
- [volumeRender](./cpp/5_Domain_Specific/volumeRender)：三维纹理基础体绘制。
- [vulkanImageCUDA](./cpp/5_Domain_Specific/vulkanImageCUDA)：Vulkan Image 与 CUDA box filter 互操作。

### [6. Performance](./cpp/6_Performance/README.md)

- [alignedTypes](./cpp/6_Performance/alignedTypes)：比较对齐/非对齐结构的访问吞吐。
- [transpose](./cpp/6_Performance/transpose)：矩阵转置及性能优化策略。
- [UnifiedMemoryPerf](./cpp/6_Performance/UnifiedMemoryPerf)：比较统一内存提示、zero-copy、pageable 与 pinned 内存传输。
- [cudaGraphsPerfScaling](./cpp/6_Performance/cudaGraphsPerfScaling)：分析 CUDA Graph 随图规模变化的性能。

### [7. libNVVM](./cpp/7_libNVVM/README.md)

这些示例展示 libNVVM 与 NVVM IR，用于将 IR 编译为 PTX、链接、验证并通过 Driver API 执行；运行需兼容当前 CUDA Toolkit 的 NVIDIA 驱动。

- [cuda-shared-memory](./cpp/7_libNVVM/cuda-shared-memory)：NVVM IR 中的 CUDA shared memory。
- [cuda-c-linking](./cpp/7_libNVVM/cuda-c-linking)：用 LLVM IR 构建 API 生成 IR，与 nvcc 生成的 PTX 链接后执行。
- [device-side-launch](./cpp/7_libNVVM/device-side-launch)：kernel 内启动 kernel，即动态并行。
- [ptxgen](./cpp/7_libNVVM/ptxgen)：独立 NVVM IR→PTX 编译器，链接 libDevice 并验证 IR。
- [simple](./cpp/7_libNVVM/simple)：读取 NVVM IR、编译为 PTX 并通过 Driver API 启动。
- [syscalls](./cpp/7_libNVVM/syscalls)：设备端 malloc/free/vprintf。
- [uvmlite](./cpp/7_libNVVM/uvmlite)：统一虚拟内存。

libNVVM 示例可用 `CUDA_HOME`、`LIBNVVM_HOME`、`LLVM_HOME` 指定工具链路径；Linux 使用 `utils/build.sh`，Windows 使用 `utils/build.bat`，也可用顶层 Makefile。`cuda-c-linking` 需要 LLVM 7+ 开发包，具体版本与 CUDA 13+ 存根库设置见 [原分类说明](./cpp/7_libNVVM/README.md)。

### [8. Platform Specific](./cpp/8_Platform_Specific/Tegra/README.md)

Tegra、DLA、NvSci/NvMedia 与 OpenGL ES 平台示例：

- [cudaNvSciNvMedia](./cpp/8_Platform_Specific/Tegra/cudaNvSciNvMedia)：CUDA 与 NvMedia 经 NvSciBuf/NvSciSync 互操作。
- [cudaNvSciBufMultiplanar](./cpp/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar)：NvSciBuf 多平面 YUV 420 图像与 CUDA surface 互操作。
- [cuDLAErrorReporting](./cpp/8_Platform_Specific/Tegra/cuDLAErrorReporting)：通过 CUDA 检测 DLA 错误。
- [cuDLAHybridMode](./cpp/8_Platform_Specific/Tegra/cuDLAHybridMode)：cuDLA hybrid mode 下用 CUDA 编程 DLA。
- [cuDLALayerwiseStatsHybrid](./cpp/8_Platform_Specific/Tegra/cuDLALayerwiseStatsHybrid)：hybrid mode 的逐层统计。
- [cuDLALayerwiseStatsStandalone](./cpp/8_Platform_Specific/Tegra/cuDLALayerwiseStatsStandalone)：standalone mode 的逐层统计。
- [cuDLAStandaloneMode](./cpp/8_Platform_Specific/Tegra/cuDLAStandaloneMode)：不使用 CUDA 的 DLA standalone mode。
- [EGLSync\_CUDAEvent\_Interop](./cpp/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop)：CUDA Event 与 EGL Sync/EGL Image 的 GPU 同步。
- [fluidsGLES](./cpp/8_Platform_Specific/Tegra/fluidsGLES)：CUDA/CUFFT 流体模拟与 OpenGL ES 渲染。
- [nbody\_opengles](./cpp/8_Platform_Specific/Tegra/nbody_opengles)：无交互 OpenGL ES N-body 模拟。
- [simpleGLES](./cpp/8_Platform_Specific/Tegra/simpleGLES)：CUDA 修改顶点并用 OpenGL ES 渲染。
- [simpleGLES\_EGLOutput](./cpp/8_Platform_Specific/Tegra/simpleGLES_EGLOutput)：OpenGL ES/CUDA 互操作并通过 EGLOutput/DRM 直接显示。

### [9. CUDA Tile](./cpp/9_CUDA_Tile/README.md)

- [helloTile](./cpp/9_CUDA_Tile/helloTile)：启动 tile kernel，并演示 SIMT 与 Tile kernel 经全局内存传递数据。
- [tileVectorAdd](./cpp/9_CUDA_Tile/tileVectorAdd)：用 `partition_view` 分块执行向量加法，处理边界掩码。
- [tileTranspose](./cpp/9_CUDA_Tile/tileTranspose)：分块加载、局部转置二维矩阵并写回。
- [tileMatmul](./cpp/9_CUDA_Tile/tileMatmul)：FP16 tile/FP32 累加矩阵乘法，并比较朴素与优化实现。
- [tileMatmulAutotuner](./cpp/9_CUDA_Tile/tileMatmulAutotuner)：使用 nvrtc/nvcc 为矩阵乘法自动选择 tile 大小和优化提示。
- [tileBmm](./cpp/9_CUDA_Tile/tileBmm)：静态持久化批量矩阵乘法（BMM）。
- [tileLayerNorm](./cpp/9_CUDA_Tile/tileLayerNorm)：持久化 LayerNorm 前向计算。
- [tileRope](./cpp/9_CUDA_Tile/tileRope)：split-half（GPT-NeoX 风格）旋转位置编码 RoPE。
- [tileSpMV](./cpp/9_CUDA_Tile/tileSpMV)：稀疏矩阵-向量乘法 SpMV。

## 安装示例

安装目录按目标架构、目标操作系统和构建类型组织，默认路径为 `build/bin/${TARGET_ARCH}/${TARGET_OS}/${BUILD_TYPE}`，例如 `build/bin/x64/linux/release`、`build/bin/aarch64/linux/release`、`build/bin/amd64/windows/release`。

可通过以下 CMake 变量修改路径：

- `CMAKE_INSTALL_PREFIX`：修改安装根目录（默认 `build/bin`），例如 `cmake -DCMAKE_INSTALL_PREFIX=/custom/path ..`。
- `CUDA_SAMPLES_INSTALL_DIR`：指定最终目录并覆盖结构化路径，例如 `cmake -DCUDA_SAMPLES_INSTALL_DIR=/exact/install/path ..`。

Linux 配置并构建后执行：

```bash
cd build
make install
```

Windows 命令行执行：

```cmd
cd build
cmake --build . --config Release
cmake --install . --config Release
```

也可在 Visual Studio 中构建解决方案，然后在 `CMakePredefinedTargets` 下右键 `INSTALL` 构建。多配置生成器用 `--config` 选择 Debug 或 Release。

## 运行全部示例作为测试

这些示例不是 CUDA 验证套件，也不覆盖所有边界情况、Runtime/Driver API，亦不用于性能基准。仓库提供 `run_tests.py` 做快速 sanity check：

```bash
mkdir build && cd build
cmake ..
make -j$(nproc)
cd ..
python3 run_tests.py --output ./test --dir ./build/cpp --config test_args.json
```

主要参数：`--dir` 指定递归搜索可执行文件的目录；`--config` 指定参数 JSON；`--output` 保存 stdout；`--parallel` 指定并行应用数；`--args` 为所有程序传递全局参数。`test_args.json` 支持 `skip`、单次 `args` 和多次 `runs`。脚本成功返回 0；失败时打印失败示例，并可在输出目录查看 `APM_<application>.txt` 或 `.run<n>.txt` 日志。

## 依赖

若第三方依赖可检测但未安装，对应示例通常会在构建时自动跳过。每个示例的具体依赖以其 README 的 Dependencies 为准。

常见依赖包括：FreeImage（图像）、MPI（分布式进程通信）、64 位操作系统、DirectX/DirectX 12、OpenGL、OpenGL ES、Freeglut、Vulkan、GLEW、GLFW、OpenMP、QNX Screen、X11、EGL、EGLOutput、EGLSync、NvSci、NvMedia，以及 CUDA 提供的 cuFFT callback、动态并行、多 block/多设备 Cooperative Groups、cuBLAS、CUDA IPC、cuFFT、cuRAND、cuSPARSE、cuSOLVER、NPP、NVGRAPH、NVJPEG、NVRTC、NVJITLINK、stream priorities、统一虚拟内存、FP16、C++11 CUDA 和 CMake 等功能。Windows 图形示例常需相应 Windows SDK；Vulkan 示例需 [Vulkan SDK](https://www.lunarg.com/vulkan-sdk/)，GLFW 可从其[官网](https://www.glfw.org/download.html)获取。

## 贡献指南

欢迎提交问题和示例建议。目前暂不接受公众代码贡献，贡献模式后续可能调整。源代码遵循 [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)。

## FAQ

CUDA 常见问题请参见 [CUDA FAQ](http://developer.nvidia.com/cuda-faq) 与 [CUDA Toolkit Release Notes](http://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)。

## 参考

- [CUDA Programming Guide](https://docs.nvidia.com/cuda/cuda-programming-guide/index.html)
- [Accelerated Computing Blog](https://developer.nvidia.com/blog/?tags=accelerated-computing)

## 致谢与版权

Teapot 图片来自 [Wikimedia](https://en.wikipedia.org/wiki/File:Original_Utah_Teapot.jpg)，依据 Creative Commons [Attribution-Share Alike 2.0](https://creativecommons.org/licenses/by-sa/2.0/deed.en) 通用许可证授权，并针对本示例进行了修改。
