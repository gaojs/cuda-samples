# CUDA Samples 12.8 构建指南与当前环境记录

## 1. 当前版本

当前工作分支为 `hill`，HEAD 为 CUDA Samples 官方 CUDA 12.8 发布提交：

```text
branch: hill
commit: db3eea23946bca2e90a75eca2b5b3e07158a9e11
```

该提交和变更记录对应 CUDA Samples 12.8 发布，构建系统已迁移至 CMake。仓库根目录 README 的简介称支持 CUDA Toolkit 12.6，但前置条件又要求 CUDA Toolkit 12.8，原文存在不一致；本指南按该分支提交及变更记录，将其作为 CUDA Samples 12.8 版本记录。分支核对信息来自当前本地 Git 仓库；可对照 [README.md](../README.md)、[CHANGELOG.md](../CHANGELOG.md) 及官方 [v12.8 源码标签](https://github.com/NVIDIA/cuda-samples/tree/v12.8)。

## 2. Linux 配置与全量构建

在仓库根目录运行：

```bash
cmake -S . -B build \
  -DCMAKE_CUDA_ARCHITECTURES=89

cmake --build build -j"$(nproc)"
```

`89` 对应 Compute Capability 8.9。仓库根 [CMakeLists.txt](../CMakeLists.txt) 声明需要 CMake 3.20 及以上、启用 C/C++/CUDA，并将各 Samples 分类加入构建。

若要尽量在某个目标失败后继续构建其余目标、便于收集错误，可使用 GNU Make 的 keep-going 选项：

```bash
cmake --build build -j16 -- -k 2>&1 | tee /tmp/cuda-samples-build.log
```

`-k` 不会修复或忽略错误；全量构建只有所有已纳入构建的目标都成功完成才算通过。查看日志中的失败/跳过信息：

```bash
grep -nE 'error:|Error [0-9]|FAILED|not found|will not build sample' \
  /tmp/cuda-samples-build.log
```

列出生成的可执行文件：

```bash
find build -type f -executable -print | sort
```

## 3. 当前 hill 分支的依赖特点

CUDA Samples 12.8 仓库已迁移至 CMake。当前分支 CMakeLists 中的架构列表包含 `89`，同时也列有 100、101、120 等较新架构编号。使用 CUDA 12.8 时建议只为本机 GPU 指定 `89`，以避免为其他架构生成代码时遇到工具链不支持的问题。

各示例的构建条件不同：

- 第三方依赖未安装时，对应 Sample 可能会在配置阶段跳过；
- Tegra 示例需通过 `-DBUILD_TEGRA=True` 显式启用，并要求 Tegra 工具链/平台依赖；
- Vulkan、FreeImage、MPI、OpenGL/EGL、NvSci 等示例有额外依赖；
- libNVVM 的 `cuda-c-linking` 示例还需 LLVM 开发组件（README 提到 LLVM 7–14）。

每个示例的具体依赖请查看该目录 README 的 Dependencies 说明。**配置阶段跳过不等于编译失败**，应分别记录。

## 4. CCCL 和其他依赖

CCCL 是 CUDA C++ Core Libraries，包含 CUB、Thrust 和 libcudacxx 等。CPM（CMake Package Manager）是通用 CMake 依赖管理工具，不是 CUDA 专用包管理器。

本版本的构建实际使用了 CCCL，但干净构建结果确认它使用的是 CUDA Toolkit 安装目录内自带的 CCCL（例如 Thrust 头文件位于 `/usr/local/cuda/targets/x86_64-linux/include/thrust`），并未使用独立的 `cccl-source`。重新配置和编译没有触发 CCCL 下载；仓库外的 `cccl-source` 目录不是本分支构建所必需的。

## 5. 当前 hill 分支干净构建结果

本次先删除整个 `build` 目录，再从空构建目录重新配置和全量编译。环境为 CUDA Toolkit/NVCC 12.8.93、CMake 3.28.3、GNU C/C++ 13.3.0、NVIDIA GeForce RTX 4090 D（Compute Capability 8.9）。

```text
源码分支：hill
GPU 架构参数：-DCMAKE_CUDA_ARCHITECTURES=89
干净配置：成功
干净全量构建：成功，退出码 0
Built target 数：188
编译错误 / FAILED：未发现
构建日志：/tmp/cuda-samples-hill-clean-build.log
```

干净构建实际使用 CUDA Toolkit 安装目录内的 CCCL（Thrust 头文件来自 `/usr/local/cuda/targets/x86_64-linux/include/thrust`）；本次并未使用仓库外部的 `cccl-source` 或 `cccl-3.3.3.tar.gz`。配置和构建均未触发 CCCL 下载。

### NvSCI 是什么，为什么没有影响其他目标

NvSCI（NVIDIA SCI）是 NVIDIA 面向 DRIVE 等嵌入式平台的系统软件接口族，包含用于跨进程/硬件模块共享缓冲区、同步与数据流的 NvSciBuf、NvSciSync、NvSciStream 等组件。CUDA Samples 中的 `cudaNvSci` 演示 CUDA 与这些接口的互操作。

配置日志显示 `NvSCI not found - will not build sample 'cudaNvSci'`，表示 CMake 在探测该示例的专用头文件/库时未找到 NvSCI，于是只将 `cudaNvSci` 这个可选目标排除。Samples 的 CMake 对各示例分别配置依赖；NvSCI 不是其他 CUDA 示例共同依赖的构建前置条件，因此该目标被跳过不会使其他已配置目标失败。这次其余纳入构建的目标全部成功。

不建议在这台普通 Linux + RTX 4090 D 主机上仅凭 sudo 尝试安装 NvSCI：它不是 Ubuntu 通用开发包，也不是标准 CUDA Toolkit 组件，通常随特定 NVIDIA DRIVE OS/SDK 和受支持的 DRIVE 平台提供。sudo 只授予系统安装权限，不会补上 SDK 获取权限、目标平台及硬件。若没有匹配 DRIVE 平台和授权 SDK，就应保留该目标跳过；不要从不明来源安装同名库或把其他版本的头文件/库路径强行加入构建。可参考 NVIDIA 官方 [DRIVE OS NvSciBuf 文档](https://docs.nvidia.com/drive/drive-os-5.2.6.0L/drive-os/DRIVE_OS_Linux_SDK_NGC_Development_Guide/Graphics/nvsci_nvscibuf.html) 与 [SDK Manager DRIVE 安装指南](https://docs.nvidia.com/sdk-manager/1.9/install-with-sdkm-drive/index.html)。

### 本次未纳入构建的其他项目

- `simpleD3D11`、`simpleD3D11Texture`、`simpleD3D12`：仅支持 Windows，Linux 下跳过。
- `cuda-c-linking`：配置阶段明确跳过；它有独立的 LLVM/libNVVM 与平台要求。
- Vulkan 探测提示缺少可选的 `glslc`、`glslangValidator` 组件，但已纳入构建的 `simpleVulkan`、`simpleVulkanMMAP`、`vulkanImageCUDA` 均成功生成。

### 构建产物清单及用途

下列为 `build/Samples` 下实际生成的 **180 个可执行文件**，按 Samples 分类完整列出；CMake 构建统计另有 8 个非独立可执行文件的目标，因此 `Built target` 总数为 188。路径均相对于仓库根目录。每项后的描述概括该示例主要演示的技术主题，并非运行正确性结论。可运行 `deviceQuery` 检查 GPU/驱动是否可用；桌面图形、多 GPU、IPC、MPI、EGL/Vulkan 等示例还要求相应运行时条件，编译成功不保证当前机器可以实际运行。

#### 0_Introduction（45 个）

- `UnifiedMemoryStreams` — 统一内存与 CUDA Streams 的并发/数据迁移。
- `asyncAPI` — 异步内存拷贝与内核执行。
- `clock` — 使用设备时钟测量内核执行周期。
- `clock_nvrtc` — NVRTC 动态编译版本的设备时钟示例。
- `cudaOpenMP` — CUDA 与 OpenMP 协作。
- `fp16ScalarProduct` — 半精度浮点向量点积。
- `matrixMul` — CUDA Runtime 矩阵乘法基础实现。
- `matrixMulDrv` — 使用 CUDA Driver API 的矩阵乘法。
- `matrixMulDynlinkJIT` — Driver API 动态链接与 PTX JIT 矩阵乘法。
- `matrixMul_nvrtc` — NVRTC 运行时编译矩阵乘法。
- `mergeSort` — GPU 归并排序。
- `simpleAWBarrier` — 异步工作/线程屏障同步。
- `simpleAssert` — 设备端断言。
- `simpleAssert_nvrtc` — NVRTC 编译的设备端断言示例。
- `simpleAtomicIntrinsics` — CUDA 原子操作内建函数。
- `simpleAtomicIntrinsics_nvrtc` — NVRTC 编译的原子操作示例。
- `simpleAttributes` — 查询设备及内核属性。
- `simpleCUDA2GL` — CUDA 与 OpenGL 资源互操作。
- `simpleCallback` — CUDA Stream 回调。
- `simpleCooperativeGroups` — Cooperative Groups 协作线程组。
- `simpleCubemapTexture` — Cubemap 纹理访问。
- `simpleDrvRuntime` — CUDA Driver API 与 Runtime API 互操作。
- `simpleHyperQ` — Hyper-Q 与多 Stream 并发。
- `simpleIPC` — CUDA 进程间通信与共享资源。
- `simpleLayeredTexture` — 分层纹理访问。
- `simpleMPI` — CUDA 与 MPI 多进程协作。
- `simpleMultiCopy` — 多拷贝引擎/异步拷贝行为。
- `simpleMultiGPU` — 多 GPU 上的工作分配。
- `simpleOccupancy` — 内核占用率计算与调优。
- `simpleP2P` — 多 GPU 点对点访问。
- `simplePitchLinearTexture` — Pitch-linear 纹理访问。
- `simplePrintf` — 设备端 `printf`。
- `simpleStreams` — Stream 并发执行基础。
- `simpleSurfaceWrite` — CUDA Surface 写入。
- `simpleTemplates` — CUDA C++ 模板内核。
- `simpleTexture` — 纹理对象基础用法。
- `simpleTexture3D` — 三维纹理访问。
- `simpleTextureDrv` — Driver API 纹理对象。
- `simpleVoteIntrinsics` — warp vote 投票指令。
- `simpleZeroCopy` — 映射主机内存与零拷贝访问。
- `systemWideAtomics` — 系统范围原子操作。
- `vectorAdd` — CUDA Runtime 向量加法入门。
- `vectorAddDrv` — Driver API 向量加法。
- `vectorAddMMAP` — 映射内存方式的向量加法。
- `vectorAdd_nvrtc` — NVRTC 运行时编译向量加法。

#### 1_Utilities（4 个）

- `bandwidthTest` — 测量主机与设备间及设备内存带宽。
- `deviceQuery` — 查询 CUDA 设备属性与可用能力。
- `deviceQueryDrv` — 使用 Driver API 查询设备属性。
- `topologyQuery` — 查询多 GPU/系统拓扑关系。

#### 2_Concepts_and_Techniques（32 个）

- `EGLStream_CUDA_CrossGPU` — EGLStream 与跨 GPU CUDA 互操作。
- `EGLStream_CUDA_Interop` — EGLStream 与 CUDA 图像互操作。
- `FunctionPointers` — 设备函数指针与间接调用。
- `MC_EstimatePiInlineP` — 内联并行随机数/蒙特卡洛估算圆周率。
- `MC_EstimatePiInlineQ` — 内联准随机数蒙特卡洛估算圆周率。
- `MC_EstimatePiP` — 并行随机数蒙特卡洛估算圆周率。
- `MC_EstimatePiQ` — 准随机数蒙特卡洛估算圆周率。
- `MC_SingleAsianOptionP` — 蒙特卡洛欧式/亚式金融期权计算示例。
- `boxFilter` — 图像盒式滤波。
- `convolutionSeparable` — 可分离卷积实现。
- `convolutionTexture` — 基于纹理读取的卷积。
- `dct8x8` — 8×8 离散余弦变换。
- `eigenvalues` — 矩阵特征值计算。
- `histogram` — 并行直方图计算。
- `imageDenoising` — 图像去噪与并行迭代。
- `inlinePTX` — 在 CUDA C++ 中嵌入 PTX 汇编。
- `inlinePTX_nvrtc` — NVRTC 与内嵌 PTX。
- `interval` — 区间算术计算。
- `particles` — 粒子系统并行模拟。
- `radixSortThrust` — 使用 Thrust 执行基数排序。
- `reduction` — 并行归约求和。
- `reductionMultiBlockCG` — 多线程块协作归约。
- `scalarProd` — 向量点积/标量积。
- `scan` — 并行前缀扫描。
- `segmentationTreeThrust` — 使用 Thrust 构建分割树/并行数据处理。
- `shfl_scan` — 使用 warp shuffle 的前缀扫描。
- `sortingNetworks` — GPU 排序网络。
- `streamOrderedAllocation` — Stream-ordered 内存分配。
- `streamOrderedAllocationIPC` — Stream-ordered 分配与 IPC。
- `streamOrderedAllocationP2P` — Stream-ordered 分配与 GPU P2P。
- `threadFenceReduction` — 使用线程栅栏实现归约。
- `threadMigration` — CUDA 上下文/线程迁移行为示例。

#### 3_CUDA_Features（24 个）

- `StreamPriorities` — Stream 优先级调度。
- `bf16TensorCoreGemm` — Tensor Core BF16 矩阵乘法。
- `binaryPartitionCG` — Cooperative Groups 二分区划分。
- `bindlessTexture` — bindless 纹理资源访问。
- `cdpAdvancedQuicksort` — 动态并行与高级快速排序。
- `cdpBezierTessellation` — 动态并行 Bezier 曲面细分。
- `cdpQuadtree` — 动态并行四叉树构建/遍历。
- `cdpSimplePrint` — 动态并行设备端打印。
- `cdpSimpleQuicksort` — 动态并行快速排序。
- `cudaCompressibleMemory` — CUDA 可压缩内存分配与使用。
- `cudaTensorCoreGemm` — Tensor Core 矩阵乘法。
- `dmmaTensorCoreGemm` — DMMA 指令与 Tensor Core 矩阵乘法。
- `globalToShmemAsyncCopy` — 全局内存到共享内存异步拷贝。
- `graphConditionalNodes` — CUDA Graph 条件节点。
- `graphMemoryFootprint` — CUDA Graph 内存占用分析。
- `graphMemoryNodes` — CUDA Graph 内存节点。
- `immaTensorCoreGemm` — IMMA 整数矩阵乘法指令。
- `jacobiCudaGraphs` — 使用 CUDA Graph 执行 Jacobi 迭代。
- `memMapIPCDrv` — Driver API 映射内存与 IPC。
- `newdelete` — 设备端动态对象分配/释放。
- `ptxjit` — PTX 即时编译与执行。
- `simpleCudaGraphs` — CUDA Graph 捕获与重放。
- `tf32TensorCoreGemm` — Tensor Core TF32 矩阵乘法。
- `warpAggregatedAtomicsCG` — Cooperative Groups 聚合 warp 原子操作。

#### 4_CUDA_Libraries（33 个）

- `FilterBorderControlNPP` — 使用 NPP 控制图像滤波边界。
- `MersenneTwisterGP11213` — GPU 上的 Mersenne Twister 随机数生成。
- `batchCUBLAS` — 批量 cuBLAS 矩阵运算。
- `boxFilterNPP` — 使用 NPP 执行盒式图像滤波。
- `cannyEdgeDetectorNPP` — NPP Canny 边缘检测。
- `conjugateGradient` — 共轭梯度线性求解。
- `conjugateGradientCudaGraphs` — CUDA Graph 加速共轭梯度迭代。
- `conjugateGradientMultiBlockCG` — Cooperative Groups 多块共轭梯度求解。
- `conjugateGradientMultiDeviceCG` — 多 GPU 共轭梯度求解。
- `conjugateGradientPrecond` — 预条件共轭梯度求解。
- `conjugateGradientUM` — 统一内存上的共轭梯度求解。
- `cuSolverDn_LinearSolver` — cuSOLVER 稠密线性求解。
- `cuSolverRf` — cuSolverRF 稀疏矩阵重用分解/求解。
- `cuSolverSp_LinearSolver` — cuSOLVER 稀疏线性求解。
- `cuSolverSp_LowlevelCholesky` — cuSOLVER 稀疏 Cholesky 分解。
- `cuSolverSp_LowlevelQR` — cuSOLVER 稀疏 QR 分解。
- `freeImageInteropNPP` — FreeImage 图像读写与 NPP/CUDA 互操作。
- `histEqualizationNPP` — NPP 图像直方图均衡化。
- `jitLto` — CUDA 链接时优化（LTO）示例。
- `lineOfSight` — 视线/遮挡计算。
- `matrixMulCUBLAS` — 使用 cuBLAS 的矩阵乘法。
- `nvJPEG` — 使用 nvJPEG 解码 JPEG 图像。
- `nvJPEG_encoder` — 使用 nvJPEG 编码 JPEG 图像。
- `oceanFFT` — 基于 FFT 的海面波动模拟。
- `randomFog` — GPU 随机雾效生成。
- `simpleCUBLAS` — cuBLAS 基本向量/矩阵运算。
- `simpleCUBLASXT` — cuBLAS-XT 多 GPU/多处理器矩阵运算。
- `simpleCUBLAS_LU` — 使用 cuBLAS/cuSOLVER 相关流程进行 LU 线性代数示例。
- `simpleCUFFT` — cuFFT 一维傅里叶变换。
- `simpleCUFFT_2d_MGPU` — 多 GPU 二维 cuFFT。
- `simpleCUFFT_MGPU` — 多 GPU cuFFT。
- `simpleCUFFT_callback` — cuFFT 回调机制。
- `watershedSegmentationNPP` — NPP 分水岭图像分割。

#### 5_Domain_Specific（33 个）

- `BlackScholes` — Black-Scholes 期权定价。
- `BlackScholes_nvrtc` — NVRTC 版本 Black-Scholes 期权定价。
- `FDTD3d` — 三维有限差分时域仿真。
- `HSOpticalFlow` — Horn–Schunck 光流计算。
- `Mandelbrot` — Mandelbrot 分形并行计算。
- `MonteCarloMultiGPU` — 多 GPU 蒙特卡洛模拟。
- `NV12toBGRandResize` — NV12 转 BGR 并缩放图像。
- `SobelFilter` — Sobel 图像边缘滤波。
- `SobolQRNG` — Sobol 准随机数生成。
- `bicubicTexture` — 双三次纹理插值。
- `bilateralFilter` — 双边图像滤波。
- `binomialOptions` — 二项树期权定价。
- `binomialOptions_nvrtc` — NVRTC 编译的二项树期权定价。
- `convolutionFFT2D` — FFT 实现二维卷积。
- `dwtHaar1D` — Haar 一维离散小波变换。
- `dxtc` — GPU 纹理压缩（DXTC）处理。
- `fastWalshTransform` — 快速 Walsh–Hadamard 变换。
- `fluidsGL` — 基于 CUDA 与 OpenGL 的流体模拟。
- `marchingCubes` — Marching Cubes 等值面提取。
- `nbody` — N-body 引力/粒子模拟。
- `p2pBandwidthLatencyTest` — 测量 GPU P2P 带宽与延迟。
- `postProcessGL` — CUDA/OpenGL 图像后处理互操作。
- `quasirandomGenerator` — GPU 准随机数生成。
- `quasirandomGenerator_nvrtc` — NVRTC 版本准随机数生成。
- `recursiveGaussian` — 递归高斯图像滤波。
- `simpleGL` — CUDA 与 OpenGL 互操作基础。
- `simpleVulkan` — CUDA 与 Vulkan 外部内存/同步互操作。
- `simpleVulkanMMAP` — CUDA/Vulkan 映射内存互操作。
- `smokeParticles` — 烟雾粒子模拟。
- `stereoDisparity` — 双目视差估计。
- `volumeFiltering` — 三维体数据滤波。
- `volumeRender` — 体渲染。
- `vulkanImageCUDA` — Vulkan 图像与 CUDA 互操作。

#### 6_Performance（5 个）

- `LargeKernelParameter` — 大型内核参数传递与性能行为。
- `UnifiedMemoryPerf` — 统一内存性能测量。
- `alignedTypes` — 数据对齐对访存/性能的影响。
- `cudaGraphsPerfScaling` — CUDA Graph 开销与扩展性能。
- `transpose` — 矩阵转置及合并访存优化。

#### 7_libNVVM（4 个）

- `device-side-launch/dsl` — libNVVM/设备端启动相关示例。
- `ptxgen/ptxgen` — 通过 libNVVM 生成 PTX。
- `simple/simple` — libNVVM 最小编译/代码生成示例。
- `uvmlite/uvmlite` — libNVVM 与轻量统一内存相关示例。

以上清单按实际可执行文件统计；`build/CMakeFiles` 下的编译器探测程序（如 `a.out`、MPI/OpenMP 探测程序）不属于 CUDA Samples，未列入。此次只验证编译与链接，不代表所有程序均已在 GPU 上运行验证。

## 6. 官方资料

- [CUDA Samples 官方仓库](https://github.com/NVIDIA/cuda-samples)
- [CUDA Samples v12.8 标签](https://github.com/NVIDIA/cuda-samples/tree/v12.8)
- [CUDA Toolkit 12.8 Linux 安装指南](https://docs.nvidia.com/cuda/archive/12.8.0/cuda-installation-guide-linux/)
