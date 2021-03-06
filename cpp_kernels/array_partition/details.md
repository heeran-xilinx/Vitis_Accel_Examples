Array Partition
================
This example demonstrates how `array partition` in HLS kernel can help to improve the performance. In this example matrix multiple is functionality is used to showcase the benefit of array partition. Design contains two kernels "matmul" a simple matrix multiplication and "matmul_partition" a matrix multiplication implementation using array partition.

`#pragma HLS array partition` is used to partition an array into multiple smaller arrays or memories. Arrays can be partitioned in three ways, `cyclic` where elements are put into smaller arrays one by one in the interleaved manner until the whole array is partitioned, `block` where elements are put into smaller arrays from continuous blocks of original array and `complete` where array is decomposed into individual elements each having its own read/write ports. In this example, `complete` partition is used to partition one of the dimension of local Matrix array as below

```c++
int B[MAX_SIZE][MAX_SIZE];
int C[MAX_SIZE][MAX_SIZE];
#pragma HLS ARRAY_PARTITION variable = B dim = 2 complete
#pragma HLS ARRAY_PARTITION variable = C dim = 2 complete
```    

This array partition helps design to access 2nd dimension of both Matrix B and C concurrently to reduce the overall latency.

Example generates the following information as output when ran on Alevo U200 Card:
```
Found Platform
Platform Name: Xilinx
INFO: Reading ./build_dir.hw.xilinx_u200_qdma_201910_1/matmul.xclbin
Loading: './build_dir.hw.xilinx_u200_qdma_201910_1/matmul.xclbin'
|-------------------------+-------------------------|
| Kernel                  |    Wall-Clock Time (ns) |
|-------------------------+-------------------------|
| matmul:                 |                  396685 |
| matmul: partition       |                  256367 |
|-------------------------+-------------------------|
Note: Wall Clock Time is meaningful for real hardware execution only, not for emulation.
Please refer to profile summary for kernel execution time for hardware emulation.
TEST PASSED
```
