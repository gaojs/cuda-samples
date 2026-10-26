# 6. 性能

### [alignedTypes](./alignedTypes)

通过简单测试展示结构体对齐与非对齐访问之间的速度差异。它针对大块数据测量逐元素拷贝吞吐量。

### [transpose](./transpose)

演示矩阵转置，并比较不同实现策略的性能，以逐步提升转置效率。

### [UnifiedMemoryPerf](./UnifiedMemoryPerf)

使用矩阵乘法 Kernel 比较单 GPU 上不同内存类型和传输方式的性能，包括带/不带提示的统一内存、零拷贝缓冲区、可分页内存、页锁定内存，以及同步和异步传输。

### [cudaGraphsPerfScaling](./cudaGraphsPerfScaling)

展示 CUDA Graph 的性能特征，重点考察不同 Graph 规模下相关 API 的扩展表现。

### [LargeKernelParameter](./LargeKernelParameter)

演示 CUDA 12.1 引入的大型 Kernel 参数在性能和易用性方面的改进。
