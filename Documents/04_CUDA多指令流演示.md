# CUDA 多指令流演示：simpleStreams

本文根据 CUDA Samples 12.8 的 `Samples/0_Introduction/simpleStreams`，说明如何用多个 CUDA Stream 分块排入 Kernel 和异步内存拷贝，并通过 CUDA Events 比较同步执行与多 Stream 执行的时间。这里的“多指令流”对应 CUDA Streams（流），每条 Stream 是设备操作的有序队列，不是 CPU 线程。多 Stream 让操作具备并发/重叠执行的可能性，但不保证硬件一定并发执行。

## 1. 示例功能

Kernel `init_array` 对数组每个整数重复加上设备端常量 `*factor`。Host 端把数组切成若干段，每段由一条 Stream 处理。默认 `nstreams = 4`，每次重复执行时，向每条 Stream 依次提交：

1. 对该 Stream 所负责片段执行 Kernel。
2. 将该片段从 Device 异步拷贝到 Host。

同一 Stream 中，操作按提交顺序执行，因此该片段的拷贝会等待本 Stream 的 Kernel 完成；不同 Stream 的操作之间没有相同的队列顺序限制，硬件和驱动可以在资源允许时重叠不同片段的 Kernel 与拷贝。

```text
Stream 0：片段 0 的 Kernel → 片段 0 的 D2H 拷贝
Stream 1：片段 1 的 Kernel → 片段 1 的 D2H 拷贝
Stream 2：片段 2 的 Kernel → 片段 2 的 D2H 拷贝
Stream 3：片段 3 的 Kernel → 片段 3 的 D2H 拷贝
```

实际重叠取决于 GPU 的异步复制能力、工作负载、内存带宽、资源占用及同步关系。创建多条 Stream 本身不等于发生并发。

## 2. 主要 CUDA 接口

| 接口/语法 | 用途 |
|---|---|
| `cudaStreamCreate` / `cudaStreamDestroy` | 创建并释放 Stream。本例创建 4 条。 |
| `cudaMemcpyAsync` | 将每个分块的 Device-to-Host 拷贝异步排入对应 Stream。 |
| `kernel<<<grid, block, sharedMem, stream>>>` | 将 Kernel 排入指定 Stream；第四个启动配置参数为 Stream。 |
| `cudaEventCreateWithFlags` | 创建带同步属性的 CUDA Event。 |
| `cudaEventRecord` | 将 Event 排入指定 Stream，用于时间边界或依赖标记。 |
| `cudaEventSynchronize` | 阻塞 CPU，直到指定 Event 完成。 |
| `cudaEventElapsedTime` | 计算两个 Event 之间的时间，单位为毫秒。 |
| `cudaMalloc` / `cudaMemset` / `cudaMemcpy` | 分配和初始化 Device 缓冲区，拷贝常量到 Device。 |
| `cudaHostRegister` / `cudaHostUnregister` | 将普通页对齐 Host 内存注册为页锁定内存，并在结束时注销。 |
| `cudaMallocHost` / `cudaFreeHost` | 另一种分配、释放页锁定 Host 内存的方式。 |

异步 Host↔Device DMA 传输需要页锁定 Host 内存。非 Apple 平台上，本示例默认通过系统 `mmap` 分配页对齐内存，再用 `cudaHostRegister` 注册；也可加 `--use_cuda_malloc_host` 改用 `cudaMallocHost`。注册内存和 pinned allocation 都会占用系统资源，应按需使用，并在所有相关异步传输完成后再释放/注销。

## 3. 代码如何组织多 Stream

程序先创建 Stream 数组：

```cpp
cudaStream_t *streams = (cudaStream_t *)malloc(nstreams * sizeof(cudaStream_t));
for (int i = 0; i < nstreams; i++) {
  cudaStreamCreate(&streams[i]);
}
```

每轮中，先将各段 Kernel 分别排入对应 Stream，再将每段 D2H 拷贝排到同一 Stream：

```cpp
for (int i = 0; i < nstreams; i++) {
  init_array<<<blocks, threads, 0, streams[i]>>>(
      d_a + i * n / nstreams, d_c, niterations);
}

for (int i = 0; i < nstreams; i++) {
  cudaMemcpyAsync(hAligned_a + i * n / nstreams,
                  d_a + i * n / nstreams,
                  nbytes / nstreams,
                  cudaMemcpyDeviceToHost, streams[i]);
}
```

同一 Stream 的顺序保证每段输出在 Kernel 写完后再传回 Host。不同 Stream 可由 GPU 调度交错或并发执行。输入/输出分块的起始地址和大小必须对应，Host 与 Device 缓冲区在异步拷贝完成前必须保持有效且不能被不当读写。

## 4. Events 与耗时比较

示例通过 `cudaEventRecord` 和 `cudaEventElapsedTime` 测量 GPU 时间，并比较三类执行：

- **单项测量**：分别测一段 D2H 拷贝和一个 Kernel。
- **`non-streamed` 基线**：循环执行 Kernel，再执行同步 D2H 拷贝。
- **`4 streams`**：每轮向四条 Stream 排入分块 Kernel 与异步 D2H 拷贝。

计时范围由 start/stop Event 的位置决定；报告的 `non-streamed` 与 `4 streams` 是每次重复的平均毫秒数。默认重复次数为 10；使用 `--sync_method=0`、`1`、`2` 或 `4` 时，程序设为 100 次。程序结束前会校验 Host 数组内容。

源码把开始和结束 Event 记录在 Stream 0，而被测工作排入其他 Stream。其计时边界依赖所用默认 Stream 模式下 Stream 0 与这些 Stream 的同步语义；如果项目启用了 per-thread default stream 等不同语义，不应想当然地认为这两个 Event 包含了所有工作。新代码可通过在相关 Stream 中记录 Event 并建立显式依赖来清晰定义计时区间。

## 5. CPU/GPU 同步选项

运行时必须指定 `--sync_method`，否则程序只显示帮助并退出。源码仅接受 `0`、`1`、`2`、`4`，它们对应传给 `cudaSetDeviceFlags` 的设备调度策略标志：

- `0` — `cudaDeviceScheduleAuto`（自动）：由 CUDA 运行时选择主机线程等待 GPU 的策略；不保证固定采用某一种具体策略。
- `1` — `cudaDeviceScheduleSpin`（忙等）：主机线程持续检查 GPU 是否完成，通常响应较快，但会持续占用 CPU。
- `2` — `cudaDeviceScheduleYield`（让出）：主机线程等待时让出 CPU 执行机会，之后继续等待/检查；不是只让出一次。
- `4` — `cudaDeviceScheduleBlockingSync`（阻塞同步）：主机线程阻塞等待 GPU 工作完成，通常降低等待期间的 CPU 忙等占用，但唤醒可能有额外延迟。

这四项是互斥的等待调度选择，不是 Stream 数量或 GPU 并发模式；`3` 不受本程序支持。代码将选中的策略传给 `cudaSetDeviceFlags`，并在选择 `4` 时为计时 Event 选择 `cudaEventBlockingSync` 标志。参数影响主机等待 GPU 完成时的行为，不改变 Stream 中 Kernel 与拷贝的先后依赖。实际 CPU 占用和等待延迟受操作系统、Runtime 与工作负载影响，不能仅凭 GPU 计时输出判断哪种策略更优。

## 6. 构建与运行

先按[编译指导](00_CUDA示例编译指导.md)完成构建，从仓库根目录运行：

```bash
(cd build/Samples/0_Introduction/simpleStreams && ./simpleStreams --sync_method=4)
```

括号 `( ... )` 使命令在子 Shell 中运行，完成后当前终端仍位于仓库根目录；`&&` 表示只有切换目录成功后才运行程序。也可通过 `--help` 查看程序支持的选项。源码和上游说明：[simpleStreams.cu](../Samples/0_Introduction/simpleStreams/simpleStreams.cu)、[README.md](../Samples/0_Introduction/simpleStreams/README.md)。
