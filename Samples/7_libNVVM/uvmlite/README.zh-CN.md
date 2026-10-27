来自 NVVM IR 的统一虚拟内存轻量版（UVM-lite）
===================================================

本文面向目标为 NVVM IR、并计划在其语言中支持统一虚拟内存轻量版（UVM-lite）的编程语言和编译器实现者。本文提供了在 NVVM IR 层级支持 kernel 启动的底层细节。

本文假设使用 CUDA runtime。有关限制和约束，请参阅官方 CUDA 文档。

在统一虚拟内存环境中分配变量。
----------------------------------------------------------------

在支持统一虚拟内存环境的系统中，变量可以分配在一个位置，使主机和系统中的其他设备都能直接引用该变量。我们称此类变量为 managed variable，并称该变量具有 managed 属性，或称其为 managed。该属性可以通过 metadata 指定。

    @xxx = internal addrspace(1) global i32 10, align 4

    ...

    !1 = !{i32 addrspace(1)* @xxx, !"managed", i32 1}

全局变量（例如 @xxx）可以像往常一样定义和使用，但这里有一项 metadata 用于指定 managed 属性。（请注意，该属性只能用于全局地址空间中的变量。）

在主机端访问 managed 变量
---------------------------------------- 

要访问 NVVM IR 代码中定义的 managed 变量，应先获取 device 指针，这可以通过 cuModuleGetGlobal() 完成。

    CUdeviceptr devp_xxx; // 指向 xxx 的 device 指针
    size_t      size_xxx; // xxx 的大小
    result = cuModuleGetGlobal(&devp_xxx, &size_xxx, hModule, "xxx");

可以调用 cuPointerGetAttribute() 并传入指针属性 CU_POINTER_ATTRIBUTE_IS_MANAGED，查询该指针是否指向 managed memory。

    unsigned int attrVal;
    result = cuPointerGetAttribute(&attrVal, CU_POINTER_ATTRIBUTE_IS_MANAGED, devp_xxx);
    // 如果该指针是 managed，则 result 为 1，否则为 0。

由于 device 指针是不透明的，因此可以安全地使用 cuPointerGetAttribute 获取主机指针。

    void *host_ptr_xxx;
    int *p_xxx;

    result = cuPointerGetAttribute(&host_ptr_xxx, CU_POINTER_ATTRIBUTE_HOST_POINTER, devp_xxx);
    p_xxx = (int *)devp_xxx;
    *p_xxx += 1;   // 无需显式复制即可读写
