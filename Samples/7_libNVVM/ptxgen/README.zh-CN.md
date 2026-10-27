简介
============

ptxgen 是一个简单的 IR 编译器，使用输入的 NVVM IR 文件生成 PTX 代码并输出到 stdout。它会在 stderr 中生成警告、错误和其他消息。给定多个输入文件时，会先将它们链接到一个模块中再编译，并生成单个 PTX 模块。

ptxgen 总是将 libDevice 库链接到输入 NVVM IR 程序。

编译输入 IR 之前，ptxgen 会验证 IR 是否符合 NVVM IR 规范。

用法
-----

除程序名称和输入文件名外，命令行选项都会原样传递给 nvvmCompileProgram。每个输入 NVVM IR 文件可以是 bitcode 表示形式，也可以是文本表示形式。输入文件名和命令行选项可以交错排列。

例如，

    $ ptxgen a.ll -arch=compute_50 b.bc

会链接 a.ll 和 b.bc，并为 compute_50 架构生成 PTX 代码。
