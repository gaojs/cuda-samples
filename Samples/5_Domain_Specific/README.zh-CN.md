# 5. 特定领域


### [bicubicTexture](./bicubicTexture)
此示例演示如何使用 CUDA texture 高效实现双三次 B 样条插值滤波器。

### [bilateralFilter](./bilateralFilter)
双边滤波器是一种边缘保留型非线性平滑滤波器，使用 CUDA 实现并通过 OpenGL 渲染。它可用于图像恢复和去噪。每个像素的权重同时考虑其邻域像素的空间距离和颜色距离。参考文献："C. Tomasi, R. Manduchi, Bilateral Filtering for Gray and Color Images, proceeding of the ICCV, 1998, http://users.soe.ucsc.edu/~manduchi/Papers/ICCV98.pdf"

### [binomialOptions](./binomialOptions)
此示例使用二项式模型评估一组欧式期权的公平看涨期权价格。

### [binomialOptions_nvrtc](./binomialOptions_nvrtc)
此示例使用二项式模型评估一组欧式期权的公平看涨期权价格。此示例使用 NVRTC 进行运行时编译。

### [BlackScholes](./BlackScholes)
此示例使用 Black-Scholes 公式评估一组欧式期权的公平看涨和看跌期权价格。

### [BlackScholes_nvrtc](./BlackScholes_nvrtc)
此示例使用 Black-Scholes 公式评估一组欧式期权的公平看涨和看跌期权价格，并使用 NVRTC 在运行时编译相关 CUDA 内核。
    

### [convolutionFFT2D](./convolutionFFT2D)
此示例演示如何使用 FFT 变换高效实现具有超大卷积核的二维卷积。

### [dwtHaar1D](./dwtHaar1D)
对长度为 2 的幂的一维信号进行离散 Haar 小波分解。

### [dxtc](./dxtc)
使用 CUDA 进行高质量 DXT 压缩。此示例展示如何将已有的计算密集型 CPU 压缩算法并行实现在 GPU 上，并获得数量级的性能提升。

### [fastWalshTransform](./fastWalshTransform)
自然（Hadamard）顺序的快速 Walsh 变换，用于批处理长度为 2 的幂且符合要求的任意长度向量。

### [FDTD3d](./FDTD3d)
此示例在三维表面上应用有限差分时域推进模板。

### [fluidsD3D9](./fluidsD3D9)
使用 CUDA 和 CUFFT 进行流体模拟并通过 Direct3D 9 渲染的示例。需要支持 Direct3D 的设备。

### [fluidsGL](./fluidsGL)
使用 CUDA 和 CUFFT 进行流体模拟并通过 OpenGL 渲染的示例。

### [fluidsGLES](./fluidsGLES)
使用 CUDA 和 CUFFT 进行流体模拟并通过 OpenGLES 渲染的示例。

### [HSOpticalFlow](./HSOpticalFlow)
变分光流估计示例。使用 texture 进行图像操作，展示如何使用 CUDA 加速简单的 PDE 求解器。

### [Mandelbrot](./Mandelbrot)
此示例使用 CUDA 交互式计算并显示 Mandelbrot 集或 Julia 集。它还展示如何使用“双单精度”算术，在深入图案进行高倍缩放时提高精度。此示例使用双精度。感谢 NewTek 的 Mark Granger 提交此代码示例！

### [marchingCubes](./marchingCubes)
此示例使用 marching cubes 算法从体数据集中提取几何等值面。它使用 Thrust 库的 scan（前缀和）函数执行流压缩。

### [MonteCarloMultiGPU](./MonteCarloMultiGPU)
此示例使用 Monte Carlo 方法评估一组欧式期权的公平看涨期权价格，并利用系统中安装的所有支持 CUDA 的 GPU。如果存在 GTX 200 系列 GPU，此示例会使用双精度硬件。此示例还利用 CUDA 4.0 的能力，通过单个 CPU 线程控制多个 GPU。

### [nbody](./nbody)
此示例演示如何在 CUDA 中高效地进行引力 n 体全对全模拟。本示例对应 GPU Gems 3 章节“Fast N-Body Simulation with CUDA”。在 CUDA 5.5 中，Tesla K20c 的单精度性能提升至超过 1.8TFLOP/s。所有 Kepler 和 Fermi GPU 架构上的双精度性能也有所提升。从 CUDA 4.0 开始，nBody 示例已更新，可利用新功能轻松将 n 体模拟扩展到单台 PC 中的多个 GPU。在命令行中添加 `-numbodies=<bodies>` 可设置模拟的物体数量。添加命令行选项 `-numdevices=<N>` 将使示例使用 N 个设备（如果可用）进行模拟。在此模式下，所有物体的位置和速度数据都通过“zero copy”从系统内存读取，而非从设备内存读取。对于数量较少（4 个或更少）且物体数量足够大的设备，带宽不会成为瓶颈，因此可在这些设备上实现良好的扩展性。

### [nbody_opengles](./nbody_opengles)
此示例演示如何高效地进行引力 n 体全对全模拟。与 OpenGL nbody 示例不同，此示例不提供用户交互。

### [nbody_screen](./nbody_screen)
此示例演示如何高效地进行引力 n 体全对全模拟。与 OpenGL nbody 示例不同，此示例不提供用户交互。

### [NV12toBGRandResize](./NV12toBGRandResize)
此代码展示使用 CUDA 批量将 NV12 帧转换并缩放为 BGR 三平面帧的两种方法。方法一：将 NV12 输入转换为 BGR（分辨率为输入分辨率 #1），然后缩放至分辨率 #2。方法二：先将 NV12 输入缩放至分辨率 #2，再转换为 BGR 输出。NVIDIA 硬件解码器（包括 dGPU 和 Tegra）通常输出 NV12 pitch 格式帧。使用 TensorRT 进行推理时，输入帧需要为 BGR 平面格式，且尺寸可能不同。因此，解码后通常需要将 NV12 转换并缩放为 BGR 平面格式，以供后续推理使用。此 CUDA 代码提供转换和缩放的参考实现。

### [p2pBandwidthLatencyTest](./p2pBandwidthLatencyTest)
此应用演示成对 GPU 之间的 CUDA Peer-To-Peer (P2P) 数据传输，并计算延迟和带宽。测试将分别针对启用和未启用 P2P 的 GPU 对进行。

### [postProcessGL](./postProcessGL)
此示例展示如何使用 CUDA 对 OpenGL 渲染的图像进行后处理。

### [quasirandomGenerator](./quasirandomGenerator)
此示例实现 Niederreiter 准随机序列生成器和逆累积正态分布函数，用于生成标准正态分布。

### [quasirandomGenerator_nvrtc](./quasirandomGenerator_nvrtc)
此示例实现 Niederreiter 准随机序列生成器和逆累积正态分布函数，用于生成标准正态分布；相关 CUDA 内核使用 NVRTC 在运行时编译。

### [recursiveGaussian](./recursiveGaussian)
此示例使用 Deriche 递归方法实现高斯模糊。该方法的优点是执行时间与滤波器宽度无关。

### [simpleD3D10](./simpleD3D10)
演示 CUDA 与 Direct3D10 互操作的简单程序。程序使用 CUDA 生成顶点数组，并使用 Direct3D10 渲染几何图形。需要支持 Direct3D 的设备。

### [simpleD3D10RenderTarget](./simpleD3D10RenderTarget)
演示 Direct3D10 渲染目标与 CUDA 互操作的简单程序。程序使用 CUDA 处理渲染目标位置并生成带可视化的直方图。需要支持 Direct3D10 的设备。

### [simpleD3D10Texture](./simpleD3D10Texture)
演示 CUDA 与 Direct3D10 Texture 互操作的简单程序。程序创建多个由 CUDA 内核生成的 D3D10 Texture（2D、3D 和 CubeMap），然后由 Direct3D 在屏幕上渲染结果。需要支持 Direct3D10 的设备。

### [simpleD3D11](./simpleD3D11)
演示如何使用 CUDA D3D11 External Resource Interoperability API 从 CUDA 更新 D3D11 缓冲区，并使用 Keyed Mutexes 在 D3D11 与 CUDA 之间同步的简单程序。


### [simpleD3D11Texture](./simpleD3D11Texture)
演示 Direct3D11 Texture 与 CUDA 互操作的简单程序。程序创建多个由 CUDA 内核写入的 D3D11 Texture（2D、3D 和 CubeMap），然后由 Direct3D 在屏幕上渲染结果。需要支持 Direct3D 的设备。

### [simpleD3D12](./simpleD3D12)
演示 Direct3D12 与 CUDA 互操作的程序。程序在 DX12 顶点缓冲区中生成正弦波，该缓冲区由 CUDA 内核创建。DX12 和 CUDA 使用 DirectX12 Fences 进行同步，然后由 Direct3D 在屏幕上渲染结果。需要运行 Windows10 或更高版本 OS 的、支持 DirectX12 的 NVIDIA GPU。

### [simpleD3D9](./simpleD3D9)
演示 CUDA 与 Direct3D9 互操作的简单程序。程序使用 CUDA 生成顶点数组，并使用 Direct3D9 渲染几何图形。需要支持 Direct3D 的设备。

### [simpleD3D9Texture](./simpleD3D9Texture)
演示 Direct3D9 Texture 与 CUDA 互操作的简单程序。程序创建多个由 CUDA 内核写入的 D3D9 Texture（2D、3D 和 CubeMap），然后由 Direct3D 在屏幕上渲染结果。需要支持 Direct3D 的设备。

### [simpleGL](./simpleGL)
演示 CUDA 与 OpenGL 互操作的简单程序。程序使用 CUDA 修改顶点位置，并使用 OpenGL 渲染几何图形。

### [simpleGLES](./simpleGLES)
演示 CUDA 与 OpenGL ES（也称为 Graphics interop）之间数据交换。程序使用 CUDA 修改顶点位置，并使用 OpenGL ES 渲染几何图形。

### [simpleGLES_EGLOutput](./simpleGLES_EGLOutput)
演示 CUDA 与 OpenGL ES（也称为 Graphics interop）之间数据交换。程序使用 CUDA 修改顶点位置，并使用 OpenGL ES 渲染几何图形，同时展示如何通过 EGLOutput 机制和 DRM 库直接渲染到显示器。

### [simpleGLES_screen](./simpleGLES_screen)
演示 CUDA 与 OpenGL ES（也称为 Graphics interop）之间数据交换。程序使用 CUDA 修改顶点位置，并使用 OpenGL ES 渲染几何图形。

### [simpleVulkan](./simpleVulkan)
此示例演示 Vulkan CUDA 互操作。CUDA 导入 Vulkan 顶点缓冲区并对其进行操作以创建正弦波，通过 CUDA 导入的 Vulkan 信号量与 Vulkan 同步。此示例依赖 Vulkan SDK、GLFW3 库；构建此示例请参阅其目录中提供的 `Build_instructions.txt`。

### [simpleVulkanMMAP](./simpleVulkanMMAP)
此示例演示通过 cuMemMap API 实现 Vulkan CUDA 互操作。CUDA 导出供 Vulkan 作为顶点缓冲区导入的缓冲区。CUDA 调用内核处理顶点，并通过 CUDA 导入的 Vulkan 信号量与 Vulkan 同步。此示例依赖 Vulkan SDK、GLFW3 库；构建此示例请参阅其目录中提供的 `Build_instructions.txt`。

### [SLID3D10Texture](./SLID3D10Texture)
演示 SLI 与 Direct3D10 Texture 和 CUDA 互操作的简单程序。程序创建由 CUDA 内核写入的 D3D10 Texture，然后由 Direct3D 在屏幕上渲染结果。需要支持 Direct3D 的设备。

### [smokeParticles](./smokeParticles)
使用半角切片技术和体积阴影进行烟雾模拟。使用 CUDA 进行过程式模拟，使用 Thrust 库 执行排序算法，并使用 OpenGL 进行图形渲染。

### [SobelFilter](./SobelFilter)
此示例为 8 位单色图像实现 Sobel 边缘检测滤波器。

### [SobolQRNG](./SobolQRNG)
此示例实现 Sobol 准随机序列生成器。

### [stereoDisparity](./stereoDisparity)
演示如何使用 SIMD SAD（绝对差之和）内在函数计算立体视差图的 CUDA 程序。需要 Compute Capability 2.0 或更高版本。

### [VFlockingD3D10](./VFlockingD3D10)
此示例模拟鹅、鹤等大型鸟类组成的 V 字形鸟群。鸟群算法源自论文“V-like formations in flocks of artificial birds”，发表于 Artificial Life，第 14 卷第 2 期，2008 年。示例提供基于 CPU 和 GPU 的实现。按 `g` 可在两者之间切换。基于 GPU 的模拟速度比基于 CPU 的快很多。控制台窗口中的输出会报告每步模拟所用时间。按 `r` 可重置鸟群的初始分布。

### [volumeFiltering](./volumeFiltering)
此示例演示使用 三维纹理 和 3D 表面写入 进行三维体数据滤波。

### [volumeRender](./volumeRender)
此示例演示使用 三维纹理 进行基本体渲染。

### [vulkanImageCUDA](./vulkanImageCUDA)
此示例演示 Vulkan Image 与 CUDA 互操作。CUDA 导入 Vulkan 图像缓冲区，对其执行盒式滤波，并通过 CUDA 导入的 Vulkan semaphores 与 Vulkan 同步。此示例依赖 Vulkan SDK、GLFW3 库；构建此示例请参阅其目录中提供的 `Build_instructions.txt`。
