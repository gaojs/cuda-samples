# 1. 实用工具

### [bandwidthTest](./bandwidthTest)
这是一个简单的测试程序，用于测量 GPU 内存拷贝带宽以及 PCIe 上的拷贝带宽。该程序可以测量设备到设备的拷贝带宽，也可以测量使用可分页内存和页锁定内存时，主机到设备及设备到主机的拷贝带宽。

### [deviceQuery](./deviceQuery)
此示例枚举系统中 CUDA 设备的属性。

### [deviceQueryDrv](./deviceQueryDrv)
此示例使用 CUDA Driver API 调用枚举系统中 CUDA 设备的属性。

### [topologyQuery](./topologyQuery)
此示例演示如何查询包含多个 GPU 的系统拓扑结构。
