## Testbench for the NPU peripheral

This testbench tests the worst-case performance for all the NPU operations, except SoftMax.

The clock speed used is 72 MHz, the same speed used for the NEORV32 + NPU system

### Tools Required to run
- GTKWave
- GHDL

### Steps for using the testbench
1. Store the test bench in a folder
2. Put all NPU RTL files inside the same folder
3. Run the following commands:
```bash
ghdl -a --std=08 tensor_operations_base.vhd
ghdl -a --std=08 tensor_operations_pooling.vhd
ghdl -a --std=08 tensor_operations_activation.vhd
ghdl -a --std=08 tensor_operations_conv_dense.vhd
ghdl -a --std=08 wb_npu.vhd
ghdl -a --std=08 tb_wb_npu.vhd

ghdl -e --std=08 tb_wb_npu
ghdl -r --std=08 tb_wb_npu --wave=tb_wb_npu.ghw
gtkwave tb_wb_npu.ghw
```
**You can modify the commands to use relative file paths instead of creating a new folder for just the testbench**

### Worst case tests:
- MaxPool and AvgPool: 100x100 tensor
  - 10,000 elements. Completely utilize the input tensor
- ReLU and Sigmoid: 2500 words
  - 10,000 elements. Completely utilize the input tensor
- Dense: 18 inputs and 2000 outputs.
  - 18*2000 = 36,000. Completely utilize the weights and bias tensors (tensors B and C, respectively)
- Conv2D: 13x13 input tensor, 50 input channels, and 80 output channels.
  - 11x11*80 = 9680, nearly maxing out the result tensor
  - 80 filters with 50 3x3 kernels = 80 * 50 * 3 * 3 = 36,000, maxing out the weights tensor

### Results:
```bash
tb_wb_npu.vhd:188:17:@486336928ps:(report note): MaxPool = 35018
tb_wb_npu.vhd:199:17:@972666912ps:(report note): AvgPool = 35018
tb_wb_npu.vhd:209:17:@1111783008ps:(report note): ReLU = 10017
tb_wb_npu.vhd:219:17:@1250899104ps:(report note): Sigmoid = 10017
tb_wb_npu.vhd:236:17:@2028960416ps:(report note): Dense = 56024
tb_wb_npu.vhd:254:17:@116338116384ps:(report note): Conv2D = 8230786
```
