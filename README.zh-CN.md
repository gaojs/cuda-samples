# CUDA Samples

CUDA Samples 展示 CUDA Toolkit 中面向开发者的功能。本分支基于 CUDA Samples 12.8 发布，构建系统已迁移至 CMake。

## 发布说明

本节介绍 GitHub 上 CUDA Samples 的发布说明。

### 变更记录

完整历史请参阅 [CHANGELOG.md](./CHANGELOG.md)。CUDA Samples 12.8 对仓库进行了 CMake 构建系统迁移，并删除了旧版 Visual Studio 工程文件和 Makefile。部分过时示例也已删除或迁移；详情见变更记录。

## 开始使用

### 前置条件

安装适用于当前平台的 CUDA Toolkit。英文 README 的简介称本版本支持 CUDA Toolkit 12.6，但前置条件要求安装 CUDA Toolkit 12.8；两处版本号不一致。本分支提交及变更记录对应 CUDA Samples 12.8 发布，建议按该发布所对应的 Toolkit 12.8 使用。系统要求和安装步骤请参阅 [Linux 安装指南](https://docs.nvidia.com/cuda/archive/12.8.0/cuda-installation-guide-linux/) 或 [Windows 安装指南](https://docs.nvidia.com/cuda/archive/12.8.0/cuda-installation-guide-microsoft-windows/)。

### 获取 CUDA Samples

使用 Git 克隆仓库：

```bash
git clone https://github.com/NVIDIA/cuda-samples.git
```

也可以在 GitHub 仓库页面下载 ZIP 压缩包并解压。

## 构建 CUDA Samples

Samples 使用 CMake 构建。部分示例有额外的平台、CUDA 功能或第三方依赖；若系统缺少某个可选依赖，CMake 配置时可能跳过相应示例。具体依赖请查看该示例 README 中的 Dependencies 部分。

### Linux

需要 CMake 3.20 或更高版本。Debian/Ubuntu 可通过包管理器安装：

```bash
sudo apt install cmake
```

在仓库根目录配置并构建：

```bash
cmake -S . -B build
cmake --build build -j"$(nproc)"
```

如需只为指定 GPU 架构编译，可在配置时传入架构参数，例如 Compute Capability 8.9：

```bash
cmake -S . -B build -DCMAKE_CUDA_ARCHITECTURES=89
```

构建完成后，从 `build` 目录中对应示例的输出路径运行程序。也可以从仓库子目录或单个示例目录配置构建。

### Windows

Visual Studio 2019 16.5 或更高版本提供 CMake 语言服务，可从仓库根目录、子目录或单个示例导入项目。

若使用命令行，请打开 Visual Studio 提供的 `x64 Native Tools Command Prompt`，在仓库根目录执行：

```bat
mkdir build
cd build
cmake .. -G "Visual Studio 16 2019" -A x64
```

然后在 Visual Studio 中打开生成的 `CUDA_Samples.sln`，选择所需配置（例如 Debug 或 Release），按 F7 构建。运行程序时使用 Visual Studio 指定的输出目录。

### 平台专用示例

Tegra 专用示例可通过 CMake 选项启用：

```bash
cmake -S . -B build -DBUILD_TEGRA=True
```

### Tegra 交叉编译

按 Tegra Development Guide 安装 NVIDIA 工具链和交叉编译环境，并确保 CMake 3.20 或更高版本已安装。在仓库根目录执行：

```bash
cmake -S . -B build \
  -DCMAKE_TOOLCHAIN_FILE=/path/to/tegra/toolchain.cmake
cmake --build build -j"$(nproc)"
```

构建后将二进制文件传输到 Tegra 设备运行。

### Automotive Linux 平台构建

这类平台需向 CMake 指定架构专用的头文件和库路径，而不是默认的 `/usr/local/cuda/include` 或 `/usr/local/cuda/lib64`。例如：

```text
/usr/local/cuda/<ARCH>/targets/aarch64-linux/lib
/usr/local/cuda-12.8/<ARCH>/include
```

示例配置：

```bash
cmake -S . -B build \
  -DCMAKE_CUDA_COMPILER=/usr/local/cuda/bin/nvcc \
  -DCMAKE_LIBRARY_PATH=/usr/local/cuda/orin/lib64/ \
  -DCMAKE_INCLUDE_PATH=/usr/local/cuda/orin/include \
  -DBUILD_TEGRA=True
```

### QNX

当前分支的 QNX 交叉编译尚未完成验证。若需交叉编译 QNX，请使用 12.8 CMake 构建系统迁移之前的版本。

## Samples 分类

- [0. Introduction](./Samples/0_Introduction/README.md)：CUDA 与 CUDA Runtime API 入门示例。
- [1. Utilities](./Samples/1_Utilities/README.md)：查询设备能力、测量 GPU/CPU 带宽等工具。
- [2. Concepts and Techniques](./Samples/2_Concepts_and_Techniques/README.md)：CUDA 概念和常见问题求解技术。
- [3. CUDA Features](./Samples/3_CUDA_Features/README.md)：Cooperative Groups、CUDA Dynamic Parallelism、CUDA Graphs 等功能。
- [4. CUDA Libraries](./Samples/4_CUDA_Libraries/README.md)：NPP、NVJPEG、NVGRAPH、cuBLAS、cuFFT、cuSPARSE、cuSOLVER、cuRAND 等库。
- [5. Domain Specific](./Samples/5_Domain_Specific/README.md)：图形、金融、图像处理等领域示例。
- [6. Performance](./Samples/6_Performance/README.md)：性能优化示例。
- [7. libNVVM](./Samples/7_libNVVM/README.md)：libNVVM 与 NVVM IR 示例。
- `8_Platform_Specific/Tegra`：Tegra 专用示例；通过 `BUILD_TEGRA=True` 启用。

## 依赖

部分示例依赖第三方应用/库，或依赖 CUDA Toolkit 与 Driver 提供的特定功能。各示例的具体依赖列在该示例 README 的 Dependencies 部分。若检测到第三方依赖未安装，CMake 配置可能跳过该示例。

### 第三方依赖

按示例需求，依赖可能由系统包管理器提供，或需从第三方网站安装，包括：

- **FreeImage**：开源图像库。Linux 通常可从发行版包管理器安装。Windows 可将 FreeImage DLL 发行包解压到 `../../../Common/FreeImage/Dist/x64`，目录中需包含头文件和 `.lib` 文件；DLL 应放在运行目录。也可配置 `FREEIMAGE_INCLUDE_DIR` 和 `FREEIMAGE_LIBRARY`。
- **MPI**：分布式进程间通信 API。Linux 可安装 MPI 编译器（例如 Open MPI）；Windows CUDA MPI 应用可安装 MS-MPI SDK。
- **OpenGL / OpenGL ES**：图形 API；可用性依赖平台与驱动。
- **Vulkan**：构建 Vulkan 示例需要 [Vulkan SDK](https://www.lunarg.com/vulkan-sdk/)。
- **GLFW**：用于创建和管理 OpenGL、OpenGL ES、Vulkan 上下文、窗口及输入。
- **OpenMP**：多处理编程 API，通常可通过系统包管理器安装，GCC 通常已提供支持。
- **X11、EGL、EGLOutput、EGLSync**：分别用于窗口系统、图形 API 与平台显示/同步互操作；具体要求取决于示例和平台。
- **Screen**：QNX 窗口系统组件。
- **NvSci**：NVIDIA 平台上的 NvSciBuf、NvSciSync 通信接口。
- **NvMedia**：面向 Tegra 的多媒体硬件加速 API。

部分示例仅支持 64 位操作系统。Windows DirectX、DirectX 12 互操作示例还需相应 Windows SDK 与 Visual Studio；Tegra 示例需匹配的 NVIDIA 平台和工具链。

### CUDA 功能与库依赖

有些示例要求特定 GPU 架构、驱动或 CUDA Toolkit 功能。仓库文档列出的功能/库包括：CUDA Dynamic Parallelism、Multi-block / Multi-device Cooperative Groups、CUDA IPC、cuBLAS、cuFFT、cuRAND、cuSPARSE、cuSOLVER、NPP、NVGRAPH、NVJPEG、NVRTC、CUDA Stream Priorities、Unified Virtual Memory、FP16、C++11 CUDA 等。能否构建或运行需以目标样例的 README 和当前设备/Toolkit 支持情况为准。

## 贡献指南

欢迎提交问题和示例建议。当前暂不接受公众直接贡献，后续政策请关注仓库更新。源代码遵循 [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)。

## 常见问题

CUDA 常见问题请参阅 [CUDA FAQ](https://developer.nvidia.com/cuda-faq) 和 [CUDA Toolkit Release Notes](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)。

## 参考资料

- [CUDA 编程指南](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html)
- [NVIDIA Accelerated Computing Blog](https://developer.nvidia.com/blog/?tags=accelerated-computing)

## 图片署名

Teapot 图片来自 [Wikimedia](https://en.wikipedia.org/wiki/File:Original_Utah_Teapot.jpg)，采用 Creative Commons [署名-相同方式共享 2.0 通用许可](https://creativecommons.org/licenses/by-sa/2.0/deed.en)。图片经修改用于 Samples 示例。
