# CUDA 设备环境和基线

本文记录本机通过 CUDA Samples 12.8 的 `deviceQuery` 和 `bandwidthTest` 实测得到的 GPU 环境与带宽基线。数据用于后续优化前后的参考，不是硬件规格保证值；GPU Boost、系统负载、功耗/温度状态、PCIe 配置和测试实现均会影响结果。

## 1. 测试环境

| 项目 | 实测结果 |
|---|---|
| GPU | NVIDIA GeForce RTX 4090 D |
| CUDA Compute Capability | 8.9 |
| GPU 数量 | 1 |
| 显存 | 24,083 MiB（`deviceQuery`）；`nvidia-smi` 报告 24,564 MiB |
| NVIDIA Driver | 595.45.04；`deviceQuery` 显示 Driver API 版本 13.2 |
| CUDA Toolkit / Runtime | Toolkit 12.8.93；Runtime 12.8 |
| CMake | 3.28.3 |
| PCI Bus ID | `00000000:81:00.0` |
| 测试时间 | 以本文件创建时间为准；如需严格复测比较，请另记日期和运行条件 |

`deviceQuery` 报告 114 个 Multiprocessors、每个 SM 128 个 CUDA cores（合计 14,592）、最大 GPU 时钟 2,520 MHz、384-bit 显存总线、L2 缓存 72 MiB、Warp 大小 32。支持 Unified Addressing、Managed Memory、Cooperative Kernel Launch、主机页锁定内存映射，并可并发执行拷贝与 Kernel（2 个 copy engine）。ECC 当前关闭。

## 2. 运行命令

在仓库根目录运行已构建的 Samples：

```bash
./build/Samples/1_Utilities/deviceQuery/deviceQuery
./build/Samples/1_Utilities/bandwidthTest/bandwidthTest
```

本次两个程序均正常识别 GPU，结果均为 `PASS`。`bandwidthTest` 本次使用 Quick Mode 和 pinned memory，传输大小为 32,000,000 Bytes。

## 3. Pinned（锁页）主机内存

Pinned memory，也称 page-locked host memory（页锁定主机内存），是分配在系统 RAM 中、由操作系统锁定而不能换出或迁移的页面。它仍是主机内存，不是 GPU 显存。

锁页的主要作用是让 CUDA 更直接、高效地发起 Host↔Device 传输，并支持真正的异步拷贝以及与 Kernel 执行重叠。普通 pageable 内存可能需要驱动先复制到内部的页锁定暂存区，再传给 GPU；使用 pinned 内存可以避免这类额外 staging 拷贝，但 H2D/D2H 的数据仍然需要经过 PCIe 等互连传输，因此不能把 pinned 理解为消除了主机与设备间的数据拷贝。

映射的锁页内存可在部分场景让 GPU 直接访问主机内存（zero-copy），但数据访问仍走主机—设备互连，延迟和带宽特性不同于 GPU 本地显存。Pinned 内存属于有限系统资源，不应无节制分配；是否使用、是否能提速应以目标工作负载测量为准。

## 4. 带宽基线

| 传输方向 | 测试内存类型 | 测试大小 | 实测带宽 |
|---|---|---:|---:|
| Host → Device | Pinned | 32,000,000 Bytes | 25.4 GB/s |
| Device → Host | Pinned | 32,000,000 Bytes | 13.6 GB/s |
| Device → Device | GPU 内存拷贝 | 32,000,000 Bytes | 3311.6 GB/s |

这是 CUDA Samples `bandwidthTest` 的快速单次基线，不能直接等同于理论峰值或应用实际吞吐。尤其 Host↔Device 结果受主机内存、PCIe 链路、虚拟化/容器环境及系统负载影响；Device↔Device 结果是该 Sample 的测量值，也不代表任意 Kernel 的有效显存带宽。

### H2D、D2H 与 D2D 的差异

- **H2D（Host to Device）和 D2H（Device to Host）**：数据在主机 RAM 与 GPU 显存之间传输，通常受 PCIe/互连链路、主机内存子系统及驱动/测试方式共同限制。两种方向的理论能力不应简单假定相等，但 CUDA 也没有“D2H 必定只有 H2D 一半”的通用规律。
- **本次 H2D 25.4 GB/s、D2H 13.6 GB/s**：D2H 约为 H2D 的 54%，是当前机器在 `bandwidthTest` Quick Mode、32 MB pinned 测试下的一次实测差异，不应推广成常识或据此直接判断硬件故障。要判断是否稳定，应在相同条件下重复、多次交换测试顺序，并检查 PCIe 链路与系统负载。
- **D2D（Device to Device）**：数据在 GPU 内存间复制，不经主机 RAM 和 PCIe 主机链路，主要利用 GPU 的显存子系统，因此速度显著高于 H2D/D2H 是正常的。本次 3311.6 GB/s 约为 H2D 的 130 倍；该数值是此 Sample 测量到的设备内复制吞吐，不代表任意 Kernel 都能达到的显存带宽，也不能直接视为应用有效吞吐。

## 5. 对后续优化的使用方式

1. 在运行负载尽量一致的条件下重复测试，使用多次结果的中位数或稳定统计量，不依据单次读数下结论。
2. 优化前后保持 GPU、输入规模、编译选项、计时方法一致，并先确认结果正确。
3. 同时区分端到端时间、Host↔Device 拷贝时间和 Kernel 时间；若传输占比高，再检查页锁定内存、异步拷贝及计算/传输重叠。
4. CUDA Samples 自身提示其示例并非专业性能测量工具，结果可能随 GPU Boost 变化。正式分析可结合 CUDA Events、Nsight Systems 和 Nsight Compute。

## 5. 原始结果摘要

### deviceQuery

```text
Device 0: NVIDIA GeForce RTX 4090 D
CUDA Driver Version / Runtime Version: 13.2 / 12.8
CUDA Capability: 8.9
Global memory: 24083 MBytes
Multiprocessors: 114
CUDA Cores: 14592
Concurrent copy and kernel execution: Yes with 2 copy engine(s)
Result = PASS
```

### bandwidthTest

```text
Quick Mode; Pinned Memory; Transfer Size = 32000000 Bytes
Host to Device: 25.4 GB/s
Device to Host: 13.6 GB/s
Device to Device: 3311.6 GB/s
Result = PASS
```
