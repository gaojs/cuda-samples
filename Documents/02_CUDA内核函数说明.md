# CUDA 内核函数与 vectorAdd

本文以 CUDA Samples 12.8 的 `vectorAdd` 为起点，介绍 Kernel（内核函数）的基本执行模型、如何分别测量分配/拷贝/Kernel/端到端耗时，以及几个相关示例的差异。示例是教学实现；小规模向量加法主要用于理解流程，不代表通用高性能实现或可靠的性能基准。

## 1. vectorAdd 做了什么

数学操作是逐元素相加：`C[i] = A[i] + B[i]`，输入和输出均为长度 `N` 的 `float` 数组。CPU 端准备输入并分配内存，将输入拷贝到 GPU，启动 Kernel 计算，再将结果拷回 CPU 校验。

源码中的核心 Kernel 为：

```cpp
__global__ void vectorAdd(const float *A, const float *B, float *C,
                          int numElements) {
  int i = blockDim.x * blockIdx.x + threadIdx.x;

  if (i < numElements) {
    C[i] = A[i] + B[i] + 0.0f;
  }
}
```

其中 `__global__` 表示这是一个由 Host（CPU）发起、在 Device（GPU）执行的 CUDA Kernel。参数中的指针指向设备内存，`numElements` 给出有效元素数。Kernel 自身不替 Host 分配这些数组。

### 线程、Block、Grid

- **Thread**：执行单个元素的加法。`threadIdx.x` 是线程在当前 Block 内的索引。
- **Block**：一组线程。`blockIdx.x` 是当前 Block 在 Grid 中的索引；`blockDim.x` 是每个 Block 的线程数。
- **Grid**：一次 Kernel 启动的所有 Block。
- 一维线程的全局索引为 `i = blockDim.x * blockIdx.x + threadIdx.x`。将多个 Block 的线程映射到一维数组元素，使每个线程负责一个元素。
- 元素数通常不是 Block 大小的整数倍，因此必须检查 `i < numElements`，避免访问数组边界之外。

该示例使用 `threadsPerBlock = 256`，并以向上取整计算 Block 数：

```cpp
int blocksPerGrid = (numElements + threadsPerBlock - 1) / threadsPerBlock;
vectorAdd<<<blocksPerGrid, threadsPerBlock>>>(d_A, d_B, d_C, numElements);
```

当 `N = 50000` 时，Grid 有 196 个 Block，共启动 50176 个线程；多出的线程由边界判断跳过。`<<<grid, block>>>` 是 Runtime API 的 Kernel launch 语法。Kernel launch 通常异步返回 CPU，因此紧接其后的 `cudaGetLastError()` 用于检查启动配置等即时错误；它不代表 GPU 上的执行已经结束。之后的同步操作（例如本例的同步 D2H `cudaMemcpy`）会等待此前相关工作完成，并可能报告执行期间的错误。

### 执行流程与数据位置

1. Host 用 `malloc` 建立 `h_A`、`h_B`、`h_C`，在 CPU 上初始化输入。
2. Device 用 `cudaMalloc` 建立 `d_A`、`d_B`、`d_C`。
3. `cudaMemcpy(..., cudaMemcpyHostToDevice)` 将 A、B 从 Host 复制到 Device。
4. 使用 Grid/Block 配置启动 Kernel，逐元素写入 `d_C`。
5. `cudaMemcpy(..., cudaMemcpyDeviceToHost)` 将结果拷回 `h_C`；这也是等待计算完成的同步点。
6. CPU 对每个元素做结果校验，然后分别释放 Host 和 Device 内存。

要点：普通 `malloc` 得到的是 Host 内存；`cudaMalloc` 得到的是 Device 内存。Host 和 Device 地址不能因为类型同为指针就互换使用。Pinned（页锁定）Host 内存可以改善/支持异步传输，但不会消除 H2D/D2H 数据传输本身。

## 2. 与 vectorAdd 相关的示例

这些示例计算的仍是向量加法，重点是对照执行 API、Kernel 编译时机或设备内存分配机制，而不是提供不同的加法算法。

| 示例 | 与基础 `vectorAdd` 的关系 | 展示重点 |
|---|---|---|
| `vectorAdd` | 基准入门版本 | CUDA Runtime API；直接在 `.cu` 中编写 Kernel，使用 `cudaMalloc`、`cudaMemcpy` 和 `<<<>>>` 启动 |
| `vectorAddDrv` | 同一运算的 Driver API 版本 | 显式初始化 Driver、创建 Context、加载 fatbin/Module、取得 Kernel 函数句柄，并用 `cuLaunchKernel` 启动 |
| `vectorAdd_nvrtc` | Driver API 执行同一运算，Kernel 在运行时编译 | 用 NVRTC 将 `vectorAdd_kernel.cu` 编译成 CUBIN，再通过 Driver API 加载和执行；体现运行时编译流程 |
| `vectorAddMMAP` | 基于 Driver API 的向量加法，替换普通设备分配 | 用 CUDA Virtual Memory Management 的 `cuMemMap` 类接口建立映射分配，演示内存物理属性/映射管理；Kernel 的连续向量访问结构不变 |

### 各示例的适用场景

- **`vectorAdd`：常规 CUDA 入门和大多数普通 GPU 计算。** 如果使用 CUDA C++、在编译时确定 Kernel，并且 Runtime API 提供的分配、拷贝、启动功能够用，基础版通常就是合适的做法。后三个示例并非它的必要替代品。
- **`vectorAddDrv`：需要 Driver API 控制或已有 Driver API 集成环境时。** 例如应用需要显式管理 Context、Module、函数句柄，或要在更底层的 Driver API 工作流中加载设备代码。代价是 Host 端管理步骤更多；单纯做向量加法并不会因此自动更快。
- **`vectorAdd_nvrtc`：Kernel 需要在程序运行期间编译时。** 例如程序按运行时生成的源码、用户选择的参数或设备能力生成 Kernel，再编译加载。该示例用 NVRTC（NVIDIA Runtime Compilation）将源文件编译为 CUBIN；它适用于展示“运行时编译”机制，不是因为 NVRTC 比预编译必然快。需要额外的 NVRTC 运行库与编译时间；若 Kernel 固定且可提前编译，普通构建通常更简单。
- **`vectorAddMMAP`：需要显式控制 GPU 虚拟地址与物理内存映射时。** CUDA VMM 可将虚拟地址范围与一个或多个物理分配分开管理，并指定分配所在设备及访问权限；在多 GPU 内存放置、跨设备映射、稀疏/按需映射等场景有用。本 Sample 将分配按设备切分并映射到连续虚拟地址；当前单 GPU 的向量加法只是演示 API，不会体现多 GPU 条带化收益。VMM 还要求设备支持相关能力，并带来更复杂的粒度、映射、访问权限和释放管理。

**选择建议：**先使用 `vectorAdd` 学 CUDA Kernel、数据传输和结果验证；只有当应用确实需要 Driver API 的显式控制、运行时编译或 VMM 映射时，再选择对应变体。对于固定 Kernel 的普通单 GPU 程序，仅使用基础版完全可以。

### 如何构建和运行

先按[编译指导](00_CUDA示例编译指导.md)配置并构建。以下命令均可从仓库根目录 `/home/hill/cuda-samples` 执行；每条命令用子 Shell 切换到该示例自己的构建目录，彼此独立，不会改变当前终端目录。Shell 中的括号 `( ... )` 会在子 Shell 中执行括号内命令；子 Shell 退出后，当前终端仍保持原目录。命令中的 `&&` 表示只有前一条命令成功，才执行后一条：

```bash
(cd build/Samples/0_Introduction/vectorAdd && ./vectorAdd)
(cd build/Samples/0_Introduction/vectorAddDrv && ./vectorAddDrv)
(cd build/Samples/0_Introduction/vectorAdd_nvrtc && ./vectorAdd_nvrtc)
(cd build/Samples/0_Introduction/vectorAddMMAP && ./vectorAddMMAP)
```

`vectorAdd_nvrtc` 启动时通过 `sdkFindFilePath("vectorAdd_kernel.cu", argv[0])` 查找运行时编译所需的 Kernel 源文件；本仓库的构建规则会把 `vectorAdd_kernel.cu` 复制到可执行文件所在目录。从仓库根目录直接执行 `./build/.../vectorAdd_nvrtc` 时，`argv[0]` 所带路径不保证该辅助函数能定位旁边的源文件；你提供的实测结果表明，从它自己的构建目录运行成功。`vectorAddDrv` 和 `vectorAddMMAP` 则在各自目录查找 `vectorAdd_kernel64.fatbin`。这些示例依赖各自目录中的运行时文件，互相独立，不需要把一个示例的资源文件复制给另一个示例。若只需构建某个目标，可在已有 `build` 配置目录中执行，例如：

```bash
cmake --build build --target vectorAdd
cmake --build build --target vectorAddDrv
cmake --build build --target vectorAdd_nvrtc
cmake --build build --target vectorAddMMAP
```

这些示例通常直接打印 `PASS`/`Result = PASS` 表示计算校验通过。`vectorAdd_nvrtc` 需要 Toolkit 提供 NVRTC；`vectorAddMMAP` 需要设备支持 Virtual Address Management，源码会检查能力，不支持时会跳过（waive）。示例的 README 中仍可能写 Toolkit 12.5 等上游说明；本仓库本次工作基于 CUDA Samples 12.8，实际构建与运行应以当前构建配置和设备支持为准。

## 3. 建议的学习顺序

1. 运行 `vectorAdd`，对照 Host/Device 指针、索引计算、边界检查和 Kernel launch。
2. 将测量范围分别设为分配、H2D、Kernel、D2H 和端到端；预热并多轮测量，记录输入规模与计时方法。
3. 阅读 `vectorAddDrv`，把 Runtime API 的简洁调用与 Driver API 的显式 Context/Module/Function 管理对应起来。
4. 阅读 `vectorAdd_nvrtc`，追踪 Kernel 源文件如何在运行时编译、加载、启动。
5. 最后阅读 `vectorAddMMAP`，重点理解 VMM 映射分配及设备能力要求；它不是提高向量加法算术效率的版本。

## 5. 源码位置

- [vectorAdd Runtime API 源码](../Samples/0_Introduction/vectorAdd/vectorAdd.cu)
- [vectorAddDrv Driver API 源码](../Samples/0_Introduction/vectorAddDrv/vectorAddDrv.cpp)
- [vectorAdd_nvrtc Host 代码](../Samples/0_Introduction/vectorAdd_nvrtc/vectorAdd.cpp)
- [vectorAdd_nvrtc Kernel 源码](../Samples/0_Introduction/vectorAdd_nvrtc/vectorAdd_kernel.cu)
- [vectorAddMMAP 源码](../Samples/0_Introduction/vectorAddMMAP/vectorAddMMAP.cpp)
