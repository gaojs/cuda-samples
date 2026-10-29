# CUDA 矩阵乘法优化：matrixMul

本文以 CUDA Samples 的 `matrixMul` 示例为基础，说明如何使用 CUDA Kernel 计算矩阵乘法，并通过二维线程块、共享内存分块（tiling）和数据复用改善全局内存访问效率。

这个示例的首要目标是教学，而不是提供通用场景下性能最高的 GEMM 实现。实际项目通常还应评估 cuBLAS、cuBLASLt 或其他成熟矩阵计算库。

## 1. 示例演示什么

示例计算矩阵乘法：

```text
C = A × B
```

如果：

- `A` 的形状为 `M × K`；
- `B` 的形状为 `K × N`；
- `C` 的形状为 `M × N`；

则：

```text
C[row][col] = Σ A[row][k] × B[k][col]
```

本示例演示的 CUDA 编程功能包括：

- CUDA Runtime API 的设备内存分配和释放；
- 页锁定主机内存（pinned host memory）；
- CUDA Stream 和异步内存拷贝；
- CUDA Event 计时；
- 二维 Grid 和二维 Thread Block；
- 使用 shared memory 缓存矩阵 tile；
- 通过数据复用减少全局内存访问；
- 使用 Kernel 计算浮点矩阵乘法并校验结果。

## 2. 示例的矩阵规模

默认情况下，源码将线程块大小设置为 `32`：

```cpp
int block_size = 32;
```

默认矩阵尺寸为：

```text
A：320 × 320
B：640 × 320
C：640 × 320
```

这里采用“宽 × 高”的表示方式：

- `dimsA.x` 是 A 的宽度，即 K；
- `dimsA.y` 是 A 的高度，即 M；
- `dimsB.x` 是 B 的宽度，即 N；
- `dimsB.y` 是 B 的高度，即 K；
- 输出 C 的宽度为 `dimsB.x`，高度为 `dimsA.y`。

矩阵乘法要求 A 的宽度等于 B 的高度：

```text
dimsA.x == dimsB.y
```

如果不满足，程序会直接报错并退出。

## 3. 朴素矩阵乘法的问题

矩阵乘法中，一个输出元素需要读取 A 的一行和 B 的一列：

```text
C[row][col] = A[row][0] × B[0][col]
             + A[row][1] × B[1][col]
             + ...
```

如果每个线程直接从全局内存读取完成自己的点积，会产生两个问题：

1. 同一行 A 的数据会被多个线程重复读取；
2. 同一列 B 的数据也会被多个线程重复读取。

这会导致大量重复的全局内存访问。全局内存延迟较高，而同一个线程块中的线程又具有明显的数据共享需求，因此直接逐元素从全局内存读取通常不能充分发挥 GPU 的计算能力。

`matrixMul` 通过 shared memory 把线程块共同需要的数据搬到片上缓存中，让多个线程重复使用同一份 A 和 B 的数据。

## 4. 如何使用分块和共享内存

### 4.1 一个线程计算一个 C 元素

Kernel 使用二维线程块：

```cpp
dim3 threads(block_size, block_size);
```

线程坐标为：

```cpp
int tx = threadIdx.x;
int ty = threadIdx.y;
```

每个线程最终负责输出矩阵 C 中的一个元素。线程块负责 C 的一个 `BLOCK_SIZE × BLOCK_SIZE` 子矩阵。

因此，在同一个线程块中：

- `tx` 决定输出列；
- `ty` 决定输出行；
- 每个线程使用一个累加变量 `Csub` 保存自己的点积结果。

### 4.2 将矩阵切成 tile

A 和 B 沿着共同的 K 维度被切成多个 `BLOCK_SIZE × BLOCK_SIZE` 的 tile。

对某一个 C tile，Kernel 依次处理：

```text
A 的一个 tile × B 的一个 tile
```

每一轮中：

1. 每个线程从全局内存加载 A 的一个元素到 `As`；
2. 每个线程从全局内存加载 B 的一个元素到 `Bs`；
3. 线程块同步，确保整个 tile 已加载完成；
4. 每个线程在 shared memory 中循环计算 `BLOCK_SIZE` 次乘加；
5. 再次同步，确保当前 tile 的计算完成；
6. 加载下一组 A、B tile。

源码中的 shared memory 为：

```cpp
__shared__ float As[BLOCK_SIZE][BLOCK_SIZE];
__shared__ float Bs[BLOCK_SIZE][BLOCK_SIZE];
```

计算核心为：

```cpp
for (int k = 0; k < BLOCK_SIZE; ++k) {
  Csub += As[ty][k] * Bs[k][tx];
}
```

同一个 `As[ty][k]` 会被同一线程块中同一行的多个线程复用；同一个 `Bs[k][tx]` 会被同一线程块中同一列的多个线程复用。这就是分块矩阵乘法的数据复用。

### 4.3 为什么需要同步

第一次加载 tile 后，不能立即读取 shared memory，因为不同线程完成全局内存加载的时间可能不同。因此需要：

```cpp
__syncthreads();
```

第一次同步保证所有线程都完成 tile 加载，之后才能开始乘加计算。

当前 tile 计算完成后还需要第二次同步，避免部分线程已经开始覆盖 `As`、`Bs`，而其他线程仍在读取上一轮数据。

这两个同步点保证了 shared memory 的读写阶段不会相互覆盖。

## 5. 全局内存访问和数据复用

在加载 tile 时，线程按二维坐标读取 A 和 B 的连续元素：

```cpp
As[ty][tx] = A[a + wA * ty + tx];
Bs[ty][tx] = B[b + wB * ty + tx];
```

同一个 Warp 中相邻线程通常访问相邻地址，这有利于形成合并的全局内存访问。

加载到 shared memory 后，矩阵乘法的重复读取发生在片上存储中，而不是反复访问全局内存。这样做的主要收益不是减少浮点乘加次数，而是减少全局内存流量，并提高每次全局内存加载所服务的计算量。

从计算量角度看，一个 C tile 在每个 K tile 上进行约 `BLOCK_SIZE³` 次乘加，而只需加载约 `2 × BLOCK_SIZE²` 个浮点元素。随着 tile 被复用，计算与全局内存访问的比例提高。

## 6. Grid、Block 和边界条件

Grid 根据输出矩阵 C 的宽高配置：

```cpp
dim3 grid(dimsB.x / threads.x, dimsA.y / threads.y);
```

默认参数下，`block_size = 32`，线程块为 `dim3 threads(32, 32)`，即每个 Block 有 32 × 32 = 1024 个线程。默认矩阵为 A = 320 × 320、B = 640 × 320，因此输出 C = 640 × 320（宽 × 高）。Grid 的 x 维覆盖 C 的宽度，y 维覆盖 C 的高度：

```text
grid.x = dimsB.x / threads.x = 640 / 32 = 20
grid.y = dimsA.y / threads.y = 320 / 32 = 10
Grid = dim3(20, 10, 1)，共 200 个 Block
```

每个 Block 计算 C 的一个 32 × 32 tile，200 个 Block 合起来覆盖 640 × 320 的输出矩阵。因此，当前示例要求矩阵尺寸能够被线程块大小整除。默认尺寸满足这一要求。

Kernel 中没有针对任意边界尺寸添加条件判断。如果通过命令行指定宽高，应确保：

- `dimsA.x == dimsB.y`；
- `dimsA.x`、`dimsA.y`、`dimsB.x` 是 `block_size` 的整数倍；
- 矩阵尺寸不会导致显存不足。

否则可能出现越界访问、结果错误或程序失败。示例的目标是清晰展示分块算法，因此没有实现通用 GEMM 所需的边界填充和任意尺寸处理。

## 7. Host 端如何准备和执行

### 7.1 使用页锁定主机内存

主机端通过 `cudaMallocHost` 分配 A、B 和 C：

```cpp
cudaMallocHost(&h_A, mem_size_A);
cudaMallocHost(&h_B, mem_size_B);
cudaMallocHost(&h_C, mem_size_C);
```

页锁定内存有利于执行异步 Host↔Device 拷贝。随后通过 `cudaMalloc` 分配设备端的 `d_A`、`d_B` 和 `d_C`。

### 7.2 使用非阻塞 Stream 和异步拷贝

示例创建非阻塞 Stream：

```cpp
cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking);
```

然后在该 Stream 中将 A、B 从主机异步拷贝到设备：

```cpp
cudaMemcpyAsync(d_A, h_A, mem_size_A,
                cudaMemcpyHostToDevice, stream);
cudaMemcpyAsync(d_B, h_B, mem_size_B,
                cudaMemcpyHostToDevice, stream);
```

Kernel 也在同一个 Stream 中执行，因此 Stream 会按照提交顺序保证：先完成输入拷贝，再执行矩阵乘法。结果拷回主机后，程序调用 `cudaStreamSynchronize` 等待传输完成。

本示例的主要性能计时只覆盖 Kernel 的重复执行区间，不应理解为完整端到端时间。

## 8. 如何计时和计算性能

### 8.1 预热

程序先执行一次矩阵乘法 Kernel 作为预热，减少首次 Kernel 启动等因素对正式计时的影响。

### 8.2 重复执行

正式测试执行 `300` 次：

```cpp
int nIter = 300;
```

程序在同一个 Stream 中记录 CUDA Event：

```cpp
cudaEventRecord(start, stream);
// 重复执行 300 次 Kernel
cudaEventRecord(stop, stream);
cudaEventSynchronize(stop);
cudaEventElapsedTime(&msecTotal, start, stop);
```

平均单次矩阵乘法耗时为：

```text
平均耗时 = 300 次总耗时 / 300
```

### 8.3 计算 FLOP/s

矩阵乘法 C = A × B 的浮点运算量近似为：

```text
FLOPs = 2 × M × N × K
```

其中一次乘法和一次加法计为两个浮点运算。程序输出的性能指标为：

```text
GFlop/s = FLOPs / 平均耗时（秒）/ 10^9
```

程序输出格式类似：

```text
Performance= <GFlop/s> GFlop/s, Time= <毫秒数> msec,
Size= <运算数> Ops, WorkgroupSize= <线程数> threads/block
```

这里要除以平均耗时，是因为 FLOPs 表示完成这一次矩阵乘法所执行的运算总数，而 GFlop/s 表示单位时间内完成的运算数。吞吐率的定义就是“工作量 ÷ 时间”：

```text
FLOP/s = 一次矩阵乘法的 FLOPs ÷ 一次矩阵乘法的平均耗时（秒）
GFlop/s = FLOP/s ÷ 10^9
```

源码先用 CUDA Event 测得 300 次 Kernel 的总耗时，再除以 300 得到平均单次耗时，最后用单次 FLOPs 除以这个平均耗时。代码中的 `msecPerMatrixMul / 1000.0f` 是把毫秒换成秒。若不除以时间，得到的只是运算次数，不是每秒运算率。

`Performance` 越高通常表示矩阵乘法 Kernel 完成得越快；`Time` 越低表示单次 Kernel 耗时越短。比较不同版本时，应同时查看二者，并确认矩阵尺寸、数据类型和编译条件一致。

## 9. 正确性如何验证

示例初始化：

```text
A 中每个元素 = 1.0
B 中每个元素 = 0.01
```

因此 C 中每个元素的理论结果为：

```text
C[row][col] = K × 0.01
```

程序将设备端的 C 拷回主机，逐个元素计算相对误差，并与 `1.e-6` 的阈值比较。如果所有元素均通过，输出：

```text
Checking computed result for correctness: Result = PASS
```

最后程序还会返回成功状态。若存在误差超过阈值，则输出 `Result = FAIL` 并以失败状态退出。

## 10. 如何运行

先按照 [CUDA 示例编译指导](00_CUDA示例编译指导.md) 完成构建，然后从仓库根目录运行：

```bash
(cd build/Samples/0_Introduction/matrixMul && ./matrixMul)
(cd build/Samples/0_Introduction/matrixMul && ./matrixMul -help)
(cd build/Samples/0_Introduction/matrixMul && ./matrixMul -device=0)
```

也可以通过参数指定矩阵尺寸：

```bash
(cd build/Samples/0_Introduction/matrixMul && \
  ./matrixMul -wA=320 -hA=320 -wB=640 -hB=320)
```

参数含义：

- `-wA`：矩阵 A 的宽度；
- `-hA`：矩阵 A 的高度；
- `-wB`：矩阵 B 的宽度；
- `-hB`：矩阵 B 的高度；
- `-device`：选择 CUDA 设备编号。

指定尺寸时，必须满足 A 的宽度等于 B 的高度，并且各维度满足线程块整除要求。

## 11. 结果应该如何理解

运行结果通常包含设备信息、矩阵尺寸、性能、正确性和提示信息。例如：

```text
Performance= <数值> GFlop/s, Time= <数值> msec,
...
Checking computed result for correctness: Result = PASS
```

结果说明：

1. `Result = PASS` 只说明计算结果在误差阈值内正确；
2. `Time` 表示单次 Kernel 的平均设备执行时间；
3. `Performance` 是根据理论 FLOP 数和测量耗时计算出的有效计算吞吐量；
4. `WorkgroupSize` 是每个线程块中的线程数，例如 `32 × 32 = 1024`；
5. 不同 GPU、驱动、时钟状态、矩阵尺寸和编译选项会影响数值。

源码明确提醒：CUDA Samples 不是严格的性能测量工具，GPU Boost 开启时结果可能变化。因此，不应把一次运行得到的 GFlop/s 当作 GPU 的固定矩阵乘法性能。

## 12. 这个示例的局限

`matrixMul` 是教学实现，主要用于展示：

- 二维线程块；
- shared memory 分块；
- 数据复用；
- 全局内存访问；
- 线程同步；
- CUDA Event 计时。

它不是通用的最高性能矩阵乘法 Kernel，主要局限包括：

- 只支持满足线程块整除要求的尺寸；
- 使用单精度 `float`；
- 没有使用 Tensor Core；
- 没有针对不同 GPU 自动选择 tile 大小和线程组织；
- 没有实现完整的边界处理、复杂矩阵布局和通用批量场景；
- 性能计时不是完整端到端应用计时。

实际项目中，通常应该先比较 cuBLAS 或 cuBLASLt 等成熟库。如果确实需要自己编写 GEMM，再根据具体矩阵尺寸、数据类型、精度要求和硬件架构进行专门优化。

## 13. 建议的动手实验

1. 先运行默认尺寸，记录 `Time`、`Performance` 和 `Result`。
2. 修改 `-wA`、`-hA`、`-wB`、`-hB`，确保维度关系和线程块整除条件正确。
3. 对比不同矩阵规模下的 GFlop/s，观察小矩阵和大矩阵的差异。
4. 在源码中比较 `BLOCK_SIZE=16` 和 `BLOCK_SIZE=32` 的实现路径。
5. 修改 shared memory 加载方式，观察数据复用和全局内存访问变化。
6. 对比直接从全局内存计算的版本，观察 shared memory 分块带来的收益。

对比时不要只看最高 GFlop/s，还要确认：

- 结果是否正确；
- 测试是否包含相同的数据传输范围；
- 是否使用相同的矩阵尺寸和数据类型；
- 是否进行了预热和多次重复；
- 测量结果是否稳定。

## 14. 与学习路线的关系

`matrixMul` 建议放在 `transpose` 和 `alignedTypes` 之后学习：

- `transpose` 先建立合并访问、shared memory 和存储体冲突的概念；
- `alignedTypes` 补充数据对齐、元素大小和结构体布局；
- `matrixMul` 将这些访存基础扩展到二维分块、数据复用、同步和计算吞吐；
- `matrixMulCUBLAS` 再用于比较教学实现与成熟库实现。

## 15. 与 matrixMulCUBLAS 实测结果对比

### 15.1 测试方法

为了进行有意义的性能对比，两个程序必须使用相同的矩阵规格和数据类型。本次测试统一使用单精度 `float`，并计算：

```text
A(640 × 480) × B(480 × 320) = C(640 × 320)
M = 640，K = 480，N = 320
FLOPs = 2 × M × N × K = 196,608,000
```

运行手写 CUDA Kernel：

```bash
build/Samples/0_Introduction/matrixMul/matrixMul \
  -wA=480 -hA=640 -wB=320 -hB=480
```

参数对应关系为：

```text
-wA = K = 480
-hA = M = 640
-wB = N = 320
-hB = K = 480
```

运行 cuBLAS 示例：

```bash
build/Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS
```

该 cuBLAS 示例的默认尺寸正好是 `M=640、K=480、N=320`，两个程序因此都计算：

```text
A = 640 × 480，B = 480 × 320，C = 640 × 320（高 × 宽）
```

两个程序都使用单精度浮点数；手写版本调用 `MatrixMulCUDA`，cuBLAS 版本调用单精度 `cublasSgemm`。运行前应确认使用相同的 GPU、编译架构和运行环境。需要注意，两个示例的重复次数、输入初始化方式和计时代码略有不同，因此以下结果属于教学性的工程对比，而不是完全统一的基准测试。

### 15.2 测试结果

测试环境为 RTX 4090 D，Compute Capability 8.9。两个程序的运算量相同，且都通过了正确性校验：

| 示例 | 平均 Kernel 时间 | 输出性能 | 正确性 |
|---|---:|---:|---|
| `matrixMul` | 0.059 ms | 3,357.83 GFlop/s | `PASS` |
| `matrixMulCUBLAS` | 0.012 ms | 15,911.60 GFlop/s | `PASS` |

按有效吞吐计算：

```text
15,911.60 / 3,357.83 ≈ 4.74
```

因此，在这组矩阵尺寸和本次运行环境下，cuBLAS 的有效吞吐约为手写 `matrixMul` 的 **4.74 倍**，可以概括为约 5 倍。从平均耗时看，`0.059 / 0.012 ≈ 4.92`；由于输出时间只保留三位小数，耗时倍率仅作近似参考。

### 15.3 结果解释：为什么 cuBLAS 快很多

这次结果说明，当前教学版手写 Kernel 与 cuBLAS 的性能存在明显差距。手写 [matrixMul 源码](../Samples/0_Introduction/matrixMul/matrixMul.cu) 主要展示了 `32 × 32` 线程块、shared memory 分块、全局内存加载和基本数据复用，重点是代码清晰和便于学习，并不是针对 RTX 4090 D 的最高性能实现。

cuBLAS 的 `cublasSgemm` 是经过长期架构级优化的通用 GEMM 实现，会根据 GPU 架构、矩阵尺寸、数据类型、布局和资源情况选择合适的内部 Kernel。高性能 GEMM 通常会综合使用：

- 更细致的线程块分块和数据复用；
- 寄存器分块，使一个线程计算多个输出元素；
- shared memory 与寄存器之间的流水化数据搬运；
- 合并访存、向量化加载和更好的数据布局；
- 双缓冲或异步加载，重叠数据搬运与计算；
- 针对 GPU 架构的线程组织、寄存器使用和指令调度；
- 在满足数据类型和数学模式条件时使用 Tensor Core。

仅凭这次程序输出，无法确认 cuBLAS 内部实际选择了哪一个 Kernel，也不能断定本次 `cublasSgemm` 一定使用了 Tensor Core；这些信息需要通过对应版本文档、Nsight Compute 或 cuBLASLt 的算法信息进一步核实。

此外，CUDA Samples 源码本身也明确说明，`matrixMul` 是为了清晰展示 CUDA 编程原理，而不是提供通用场景下性能最高的矩阵乘法实现。因此，约 4.7～5 倍的差距是当前两个具体实现之间的差距，不应推广为所有矩阵尺寸下都固定为 5 倍。

### 15.4 手写 Kernel 的优化方向

可以以当前 `matrixMul` 的 3,357.83 GFlop/s 作为基线，使用 Nsight Compute 逐步优化：

1. **先分析瓶颈**：查看 Kernel Duration、Occupancy、SM Throughput、Memory Throughput、Global Memory Load/Store Efficiency、shared memory 和 Warp Stall，确认主要限制来自访存、同步、计算吞吐还是资源占用。
2. **改进线程到输出元素的映射**：让一个线程计算多个相邻或分块的 C 元素，增加寄存器级数据复用，减少重复的 shared memory 读取。
3. **实现寄存器分块**：为每个线程维护多个累加器，让 A 和 B 的数据在寄存器中得到更多复用。
4. **调优 tile 和线程块尺寸**：比较不同的 tile 大小、线程组织和 `BLOCK_SIZE`，在并行度、shared memory 使用量和寄存器压力之间取得平衡。
5. **改进加载与计算流水**：研究双缓冲、异步拷贝和 `cp.async`，尝试将下一组 tile 的数据搬运与当前 tile 的计算重叠。
6. **优化数据布局与访存**：确保全局内存合并访问，检查 shared memory 是否存在 bank conflict，并根据数据布局考虑向量化加载。
7. **面向 Ada 架构优化**：针对 RTX 4090 D 的计算能力、寄存器资源、shared memory 和指令特性调整 Kernel，而不是只追求某一个固定 tile 大小。
8. **研究 Tensor Core 路径**：在允许使用相应数据类型和精度模式时，学习 WMMA 或 CUTLASS 等技术，与 cuBLAS 的专用矩阵计算路径比较。

优化时每次只修改一个主要因素，并在相同的 `M、N、K`、数据类型和计时范围下重新测试。手写 Kernel 可以在固定尺寸和固定硬件上逐步逼近 cuBLAS，但不保证在所有问题规模上都达到库实现的性能；实际项目通常仍应优先使用 cuBLAS/cuBLASLt。

相关文件：[matrixMul 源码](../Samples/0_Introduction/matrixMul/matrixMul.cu)、[matrixMul 中文 README](../Samples/0_Introduction/matrixMul/README.zh-CN.md)、[matrixMulCUBLAS 源码](../Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp)。
