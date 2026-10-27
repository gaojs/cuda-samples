libNVVM 和 NVVM IR 示例
===========================

简介
------------

以下示例展示了 libNVVM 和 NVVM IR 的用法。

- cuda-shared-memory - 包含 NVVM IR 程序的目录，用于演示 CUDA 'shared' 内存的用法。

- cuda-c-linking - 使用 LLVM IR 构建 API 构建 NVVM IR 程序。它会将生成的 PTX 与 nvcc 生成的 PTX 链接，并使用 CUDA Driver API 在 GPU 上启动链接后的程序。

- device-side-launch - 演示在一个 kernel 中启动另一个 kernel（CUDA 动态并行）。

- ptxgen - 独立的 NVVM IR 程序到 PTX 编译器。它会将 libDevice 库链接到输入 NVVM IR 程序，验证 IR 是否符合 NVVM IR 规范，然后生成 PTX。

- simple - 从文件读取 NVVM IR 程序，将其编译为 PTX，并使用 CUDA Driver API 在 GPU 上启动程序。

- syscalls - 包含 NVVM IR 程序的目录，用于演示 device 端 malloc/free/vprintf 函数的用法。

- uvmlite - 演示统一虚拟内存的用法。

构建示例的步骤
------------------------------

以下环境变量可用于控制示例的构建过程。如果未指定，CUDA_HOME 将通过在 PATH 中查找 nvcc 来确定。CMake 会尝试自动识别所有这些路径。

- CUDA_HOME    : CUDA toolkit 的安装目录，例如 /usr/local/cuda。
- LIBNVVM_HOME : libNVVM 组件所在目录，例如 $CUDA_HOME/nvvm。
- LLVM_HOME    : 如果在本地构建了 llvm，此变量应指向其安装目录。仅构建 cuda-c-linking 示例时需要（见下文的 cuda-c-linking 说明）。

设置环境变量并通过 PATH 环境变量添加 CMake 工具的路径后，可运行示例脚本 utils/build.sh（Linux）或 utils/build.bat（Windows）。此脚本会使用 "build" 目录构建示例，然后将其安装到 "install" 目录。

如果选择使用 Visual Studio 及其集成的 CMake 支持进行构建，只需运行 "Build All" 和 "Install libnvvm-samples"。安装后的示例将复制到 out/install/\<build architecture\>/bin/

此外，我们提供了 Makefile，可在 Linux 上作为顶层 cuda-samples 构建的一部分自动构建这些示例。Windows 用户应通过 utils/built.bat 或 Visual Studio 的 CMake 集成手动构建。

关于 cuda-c-linking 示例的说明
--------------------------------------

此示例需要 7 到 14（含）版本之间的 LLVM 开发包（或本地构建的 LLVM 库）。LLVM 15 默认使用 opaque pointers，目前 libNVVM 尚不支持它们。

希望构建 cuda-c-linking 示例且打算使用本地构建 LLVM 的用户，需要设置 LLVM_HOME 环境变量。此示例需要包含 LLVM 头文件和库的 LLVM 开发包。

满足 LLVM 依赖后，用户可以通过 CMake 命令行调用设置 CMake 变量 "ENABLE_CUDA_C_LINKING_SAMPLE"，或修改此目录中的 CMakeLists.txt，以启用此示例的构建。

Windows 用户应从 llvm.org 下载 LLVM 14 源码并在本地构建和安装 LLVM。使用 llvm.org 提供的 Windows 安装程序会缺少 cuda-c-linking 示例依赖的一些必要组件。

对于 Ubuntu 用户，"llvm-dev" 软件包包含此示例所需的 LLVM 头文件和库，因此通常无需显式定义 LLVM_HOME。

Windows 用户应使用与构建 LLVM 相同的 CMake 构建模式来构建此示例。例如，如果以 Release 模式构建 LLVM，此示例也应以 Release 模式构建。可更新 utils/build.bat 来实现：在 CMake 调用中添加 "-DCMAKE_BUILD_TYPE=Release"。
