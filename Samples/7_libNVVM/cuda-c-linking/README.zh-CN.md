简介
============

此示例演示将 libnvvm 生成的模块与现有 CUDA C 库链接。使用 LLVM C++ API 生成一个符合 NVVM IR 规范且包含对外部定义函数调用的 LLVM IR 模块，并使用 libnvvm 将此模块编译为 PTX。随后使用 JIT linker（CUDA Driver API 的一部分）汇编 PTX 并将其与数学库链接，从而创建链接后的 CUBIN 映像。然后在系统中的第一个 CUDA 设备上执行此映像。

文件
-----

- cuda-c-linking.cpp    - 主源文件，演示使用 libnvvm 生成 PTX 文件并将其与 CUDA C device 库链接

- math-funcs            - CUDA C device 库源文件

- CMakeLists.txt        - CMake 构建脚本

构建
--------

此示例作为 CUDA samples 树中 libnvvm 示例的一部分进行可选构建。构建说明请参阅 libnvvm samples 根目录中的 README 文件。

用法
-----

构建完成后，运行 "cuda-c-linking" 二进制文件即可执行此示例。

Linux:

    $ cd $SAMPLES_INSTALL_DIR
    $ ./cuda-c-linking

Windows:

    $ cd %SAMPLES_INSTALL_DIR%
    $ cuda-c-linking.exe

以下命令行选项可用于检查：

- -save-ptx     - 将生成的 PTX kernel 写入 cuda-c-linking.kernel.ptx
- -save-ir      - 将生成的 LLVM IR 写入 cuda-c-linking.kernel.ll
- -save-cubin   - 将链接后的 CUBIN 映像写入 cuda-c-linking.linked.cubin
