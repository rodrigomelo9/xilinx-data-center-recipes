# Alveo Workflow

## Host and FPGA binaries building

### Software defined development with Vitis (Makefile based)

Preparation:
```bash
source <VITIS_INSTALL_PATH>/settings64.sh
```

Software Emulation:
```bash
make clean
make check TARGET=sw_emu
```

Hardware Emulation:
```bash
make clean
make check TARGET=hw_emu
```

Build the binaries:
```bash
make clean
make TARGET=hw
```

### Software defined development with Vitis (GUI based)

[TODO](https://github.com/Xilinx/SDAccel-Tutorials/blob/master/docs/aws-getting-started/CPP/STEP2.md)

## Hardware Execution

```bash
source /opt/xilinx/xrt/setup.sh
./<HOSTBIN> <FPGABIN>.xclbin
```
