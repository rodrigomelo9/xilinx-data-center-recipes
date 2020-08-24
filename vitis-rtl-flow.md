# Vitis RTL Flow

## Create a new Vitis project and launch the RTL Wizard

* Launch Vitis
* Select a workspace and *Launch*
* Select *Create Application Project* (or *File* -> *New* -> *Application Project...*)
* Select a *Project Name* and *Next*
* Select `xilinx_aws-vu9p-f1_shell-*` as platform and *Next*. If it is not available:
  * Click on `+`
  * Browse and select `<AWS_FPGA_REPO>/Vitis/aws-platform`, *OK*
* Select *Empty Application* and *Finish*
* Go to *Xilinx*, *RTL Kernel Wizard...*

**Notes:**
* Three main directories are created: `Emulation-SW`, `Emulation-HW` and `Hardware`
* A dummy Vivado project is created (`vivado_rtl_kernel`) when the RTL Kernel Wizard is launched

## RTL kernel wizard

* *Next* in the *Welcome to the RTL Kernel Wizard* screen
* Specify *General Settings*, *Next*
* Specify *Scalars*, *Next*
* Specify *Global Memory*, *Next*
* Specify *Streaming Interfaces*, *Next*
* *OK* in the *Summary* screen

**Notes:**
* A Vivado project for the new kernel is created under `vivado_rtl_kernel` (`<KernelName>_ex`)
* The RTL files are under `vivado_rtl_kernel/<KernelName>_ex/imports`
* A demo host app is under `vivado_rtl_kernel/<KernelName>_ex/exports/src/host_example.cpp`

## RTL Vivado Project

* Modify the design
* Click on *Generate RTL Kernel*
* *Sources-only Kernel*, *OK*
* *Yes* to exit Vivado
* *OK* on *RTL Kernel has been imported*

**Notes:**
* A kernel package (`.xo`) is created under `vivado_rtl_kernel<KernelName>_ex/exports`
* A Vivado module is created under `vivado_rtl_kernel<KernelName>_ex/<KernelName>`
* The generated kernel package and the example host file are automatically imported into the Vitis project under `src/vitis_rtl_kernel/<KernelName>`

## Build the Hardware configuration

Under the *Assistant* panel:
* Right click over *<ProjectName>_system* -> *<ProjectName>* -> *Hardware* and select *Add Binary Container...*
* Right click over the created Binary Container and select *Add Hardware Function...*
* Select your kernel and *OK*
* Right click over *<ProjectName>_system* -> *<ProjectName>* -> *Hardware* and select *Build*

**Notes:**
* You can directly select *Add Hardware Function...* and a default *binary_container_1* will be employed
* When *build* is executed, a `makefile` is generated under the corresponding directory
* This `makefile` can be used (`make`) to generate the host and fpga (`*.xclbin`) binaries
* The `Hardware` build takes several hours
