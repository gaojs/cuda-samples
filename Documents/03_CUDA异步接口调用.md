# CUDA 异步接口调用：asyncAPI

本文根据 CUDA Samples 12.8 的 `Samples/0_Introduction/asyncAPI` 说明 CUDA Stream（调用流）、Event（事件）、GPU 计时，以及 CPU 与 GPU 工作重叠的基本方式。该 Sample 的重点是展示异步提交、事件查询和重叠机制；不要将其输出的时间直接视为独立、严谨的性能基准。

## 1. asyncAPI 做了什么

示例在 GPU 上对数组每个整数加上常数 `26`。主要步骤如下：

1. 用 `cudaMallocHost` 分配页锁定（pinned）Host 内存，并用 `cudaMalloc` 分配 Device 内存。
2. 创建 CUDA Events `start` 和 `stop`。
3. 在 Stream 0 中依次排入 `start` 事件、Host-to-Device 异步拷贝、Kernel、Device-to-Host 异步拷贝、`stop` 事件。
4. CPU 在 GPU 处理队列执行期间继续工作，并通过 `cudaEventQuery(stop)` 轮询是否完成。
5. 等待 GPU 操作完成后读取 Event 时间，并检查结果。

核心代码（省略错误检查和上下文）如下：

```cpp
cudaEventRecord(start, 0);
cudaMemcpyAsync(d_a, a, nbytes, cudaMemcpyHostToDevice, 0);
increment_kernel<<<blocks, threads, 0, 0>>>(d_a, value);
cudaMemcpyAsync(a, d_a, nbytes, cudaMemcpyDeviceToHost, 0);
cudaEventRecord(stop, 0);

while (cudaEventQuery(stop) == cudaErrorNotReady) {
  // CPU 可在这里执行与 GPU 工作无依赖的任务
}

cudaEventElapsedTime(&gpu_time, start, stop);
```

本例中输入输出 Host 缓冲区由 `cudaMallocHost` 分配，因此可用于异步传输。`cudaMemcpyAsync` 只表示接口允许异步提交，不表示传输一定与 Kernel 重叠；是否真正并发还取决于依赖关系、硬件复制引擎、Stream 使用方式和资源情况。

## 2. 什么是 CUDA Stream（调用流）

CUDA Stream 是一条 GPU 操作队列。Host 代码可以把拷贝、Kernel、Event 等操作提交到 Stream；同一 Stream 中的工作按提交顺序执行。不同 Stream 之间的工作在满足依赖且硬件允许时可能并发，但不能仅凭创建了多个 Stream 就断定发生重叠。

本示例的异步拷贝和 Kernel 都显式传入 `0`，Events 也记录到 `0`，即使用默认 Stream（Stream 0）：

- `cudaMemcpyAsync(..., 0)`：将拷贝排入 Stream 0。
- `increment_kernel<<<blocks, threads, 0, 0>>>`：Kernel launch 的第四个配置参数是 Stream，值 `0` 表示 Stream 0。
- `cudaEventRecord(event, 0)`：将 Event 排入 Stream 0。

因此队列顺序是 `start → H2D → Kernel → D2H → stop`。由于 `stop` 在同一 Stream 中排在所有工作之后，stop 完成意味着它之前排入该 Stream 的操作也已完成。

Stream 是设备侧执行顺序/依赖的组织方式，不是 CPU 线程，也不意味着 GPU 上每个操作必然同时执行。默认 Stream 与其他 Stream 的交互同步语义还会受 legacy/per-thread 默认 Stream 模式影响；需要并发控制时应明确创建和使用 Stream，并查清项目采用的默认 Stream 模式。

## 3. 用 CUDA Event 计量 GPU 时间

CUDA Event 可插入某个 Stream 的时间线上。把开始 Event 排在待测 GPU 工作之前，把结束 Event 排在之后，再等待结束 Event 完成并调用 `cudaEventElapsedTime`，得到两事件之间的 GPU 时间（毫秒）。本例的 `start` 和 `stop` 都记录在 Stream 0，因此时间区间包含排在两者之间的 H2D、Kernel、D2H 操作，而不仅是 Kernel 执行时间。

基本模式如下：

```cpp
cudaStream_t stream;
cudaStreamCreate(&stream);

cudaEvent_t start, stop;
cudaEventCreate(&start);
cudaEventCreate(&stop);

cudaEventRecord(start, stream);
// 在同一个 stream 中提交待测 GPU 工作
launchKernel<<<grid, block, 0, stream>>>(...);
cudaEventRecord(stop, stream);

cudaEventSynchronize(stop);
float milliseconds = 0.0f;
cudaEventElapsedTime(&milliseconds, start, stop);

cudaEventDestroy(start);
cudaEventDestroy(stop);
cudaStreamDestroy(stream);
```

`cudaEventSynchronize(stop)` 让 CPU 阻塞等待，直到 GPU 执行到 `stop`。此后 Event 时间戳可用于计算 elapsed time。通常无需单独同步 `start`：它在同一 Stream 中排于工作之前；等待 `stop` 完成也意味着前面的 `start` 已完成。若开始和结束 Event 不在同一条实际工作的 Stream 上，或测量区间内工作存在跨 Stream 依赖，则必须另外安排依赖，不能假定 Event 自动覆盖其他 Stream 的工作。

如果只想计量 Kernel，应将 Events 紧贴 Kernel 前后记录，并保证它们和 Kernel 在同一 Stream；如果要计量一串拷贝与 Kernel，则把 Events 放在整段工作两端。Event 时间是 GPU 时间线的区间，不包含完整的 CPU 初始化、Host 分配或整个程序启动耗时。要测端到端延迟，应另用 CPU 单调时钟包住明确定义的完整流程，并在停止计时前同步相关 GPU 工作。

## 4. CPU 如何判断 GPU 工作是否完成

### 4.1 轮询：`cudaEventQuery`

```cpp
cudaError_t status = cudaEventQuery(stop);
if (status == cudaSuccess) {
  // stop 已完成；同一 Stream 中排在它前面的工作也已完成
} else if (status == cudaErrorNotReady) {
  // 尚未完成；CPU 可以做其他独立工作，稍后再查询
}
```

本例用 `while (cudaEventQuery(stop) == cudaErrorNotReady)` 轮询，并增加 `counter` 展示等待期间 CPU 仍执行了指令。轮询是非阻塞查询，但忙等会占用 CPU 核心；实际程序通常应在每轮查询之间处理有用任务，或考虑用同步等待而不是持续空转。示例中查询以外的 CUDA 错误仍应检查，不能把所有非 `cudaSuccess` 状态都当成“尚未完成”。

### 4.2 阻塞等待：`cudaEventSynchronize`

`cudaEventSynchronize(stop)` 让调用它的 CPU 线程等待指定 Event 完成。适用于后续 Host 代码必须使用结果、且没有其他 CPU 工作可做的场景。它只等待到指定 Event，不是为了检查完成状态而反复轮询。

### 4.3 设备级等待：`cudaDeviceSynchronize`

`cudaDeviceSynchronize()` 会等待当前设备此前提交的工作完成，等待范围比同步一个 Event 更广。适合需要设备级同步的场景；若只关心某一 Stream 上某个阶段，Event/Stream 级同步通常更精确。

**重要：**异步 D2H 拷贝未完成前，CPU 不应读取或复用目标 Host 缓冲区；异步 H2D 未完成前，也不应修改或释放源 Host 缓冲区。用 Event 查询或同步确认相关操作完成后，才可安全访问这些缓冲区。本示例在 `stop` 完成后才校验输出。

## 5. CPU 与 GPU 如何重叠执行

重叠的关键是：CPU 将 GPU 工作异步排入 Stream 后，不立即等待，而是执行与这些 GPU 操作无数据依赖的 CPU 工作。GPU 在其队列中执行拷贝和 Kernel 时，CPU 可同时进行其他任务；当 CPU 需要 GPU 结果时，再查询或等待相应 Event。

```text
CPU：提交 H2D / Kernel / D2H ── 执行独立工作 ── 查询/等待 stop ── 使用结果
GPU：                    H2D ── Kernel ── D2H ── stop 完成
```

“异步提交”与“实际重叠”是两回事：

- CPU 可在异步 API 返回后继续执行，不等于拷贝和 Kernel 一定在 GPU 上彼此重叠。
- 同一 Stream 中的 H2D、Kernel、D2H 按序执行；此例主要展示 CPU 工作与 GPU 队列执行在时间上的重叠。
- 异步 Host↔Device 传输通常需要 pinned Host memory；本例使用 `cudaMallocHost`。
- 重叠还取决于硬件能力、数据依赖、传输方向、资源占用和同步方式。若 CPU 工作太少，或立刻同步等待，就看不到有意义的重叠。
- 输入缓冲区、输出缓冲区和 Device 缓冲区的生命周期必须覆盖相应异步操作。

## 6. 本示例的计时输出该如何理解

示例中 `gpu_time` 来自 `cudaEventElapsedTime(start, stop)`，计时区间覆盖 Stream 0 中从 H2D 到 D2H 的已排队 GPU 工作。`sdkGetTimerValue(&timer)` 来自 Samples 的 Host 侧 Stopwatch，计时包住 CPU 发起这一组 CUDA 调用的代码段；由于异步调用通常很快返回，它不是 GPU 完成全部工作的时间。`counter` 仅显示 CPU 在轮询等待时执行了多少次循环，不是时间单位，也不是性能指标。

因此不要把 CPU API 调用耗时、Event GPU 区间和端到端时间混为一谈。该示例的主要教学目标是理解事件、异步调用和重叠，不是对特定设备做稳定的带宽或 Kernel 性能基准。

## 7. 构建和运行

先按[编译指导](00_CUDA示例编译指导.md)完成仓库构建；从仓库根目录运行：

```bash
(cd build/Samples/0_Introduction/asyncAPI && ./asyncAPI)
```

括号 `( ... )` 使命令在子 Shell 中运行，退出后当前终端仍留在仓库根目录；`&&` 表示只有切换目录成功后才运行程序。程序应报告输出校验成功。源码与说明见 [asyncAPI.cu](../Samples/0_Introduction/asyncAPI/asyncAPI.cu) 和 [README.md](../Samples/0_Introduction/asyncAPI/README.md)。
