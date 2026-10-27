来自 NVVM IR 的 device-side launch
===============================

本文面向目标为 NVVM IR、并计划在其语言中支持动态并行的编程语言和编译器实现者。本文提供了在 NVVM IR 层级支持 kernel 启动的底层细节。

本文假设使用 CUDA runtime。使用 OpenCL runtime 进行 device-side launch 的方法类似，但有所不同。

本文依据 CUDA C Programming Guide 中的“Device-Side Launch from PTX”部分编写
(http://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#device-side-launch-from-ptx)。

Kernel 启动 API
------------------

NVVM IR 程序可使用以下两个 API 实现 device-side kernel launch：cudaLaunchDevice() 和 cudaGetParameterBuffer()。cudaLaunchDevice() 使用通过调用 cudaGetParameterBuffer() 获取并填入待启动 kernel 参数的参数缓冲区来启动指定 kernel。如果待启动 kernel 不带参数，则参数缓冲区可以为 NULL，即无需调用 cudaGetParameterBuffer()。

cudaLaunchDevice
----------------

在 NVVM IR 层级，使用 cudaLaunchDeviceV2() 前必须按如下形式声明。

    ; NVVM IR level declaration of cudaLaunchDeviceV2
    declare i32 @cudaLaunchDeviceV2(i8*, %struct.CUstream_st*)

下面的 CUDA 层级声明会映射到上述 NVVM IR 层级声明之一，定义位于系统头文件 cuda_device_runtime_api.h 中。该函数定义在 cudadevrt 系统库中；要使用 device-side kernel launch 功能，必须将该库链接到程序。

    extern __device__ __cudart_builtin__ cudaError_t CUDARTAPI
    cudaLaunchDeviceV2(void *parameterBuffer, cudaStream_t stream);

第一个参数是指向参数缓冲区的指针，第二个参数是与此次启动关联的 stream。参数缓冲区的布局将在下文“参数缓冲区布局”中说明。

cudaGetParameterBuffer
----------------------

在 NVVM IR 层级使用 cudaGetParameterBufferV2() 前必须先声明。NVVM IR 层级声明必须采用以下形式：

    ; NVVM IR level declaration of cudaGetParameterBufferV2
    declare i8* @cudaGetParameterBufferV2(i8*, %struct.dim3, %struct.dim3, i32)

以下 CUDA 层级的 cudaGetParameterBufferV2() 声明会映射到上述 NVVM IR 层级声明：

    extern __device__ __cudart_builtin__ void * CUDARTAPI 
    cudaGetParameterBufferV2(void *func, dim3 gridDimension, 
                             dim3 blockDimension,
                             unsigned int sharedMemSize);

第一个参数是指向待启动 kernel 的指针，其余参数指定启动配置，即 grid 维度、block 维度和 shared memory 大小。

参数缓冲区布局
-----------------------

禁止对参数缓冲区中的参数重新排序，并且要求每个放入参数缓冲区的独立参数都按对齐方式放置。也就是说，每个参数必须放在参数缓冲区中第 n 个字节处，其中 n 是大于前一个参数所占最后一个字节偏移量的、能被参数大小整除的最小值。参数缓冲区的最大大小为 4KB。
