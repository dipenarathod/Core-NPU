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
