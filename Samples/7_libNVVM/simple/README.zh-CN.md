简介
============

simple 是一个展示 libNVVM 用法的 C 程序。
simple 通过 libNVVM API 将 simple-gpu64.ll NVVM-IR 文件转换为 PTX，并将该 PTX 传递给 CUDA driver 以在 GPU 上执行。
