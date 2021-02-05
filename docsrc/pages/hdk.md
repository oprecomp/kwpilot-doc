## kwHDK

The repository with the approrpiate files for kwHDK is [PowerPULP](https://iis-git.ee.ethz.ch/oprecomp/powerpulp).

`git clone git@iis-git.ee.ethz.ch:oprecomp/powerpulp.git`

This repository contains PowerPULP, a PULP system attached to a IBM POWER8/9 machine via the CAPI interface. We currently support the following combinations:

- board `ad8k5`, design `xcku115`, part `xcku115-flva1517-2-e`
- board `adku3`, design `xcku060`, part `xcku060-ffva1156-2-e`

### Getting Started

This project uses bender [1] for dependency management (kind of), so make sure that is installed if you want to change IPs. To get started, do the following:

Set the following environment variables to something reasonable for your setup:

- `export VSIM="vsim-10.7b -64"`
- `export VLOG="vlog-10.7b -64"`
- `export VCOM="vcom-10.7b -64"`
- `export VOPT="vopt-10.7b -64"`
- `export VIVADO="vivado-2019.1.1 vivado"`

### Flow

To synthesize and implement the design for an FPGA, proceed as follows:

1. generate the IPs needed for `capi_adapter`
2. ~~synthesize `capi_adapter`~~
3. ~~synthesize `pulp_cluster`~~
4. ~~synthesize `pulp_soc`~~
5. ~~synthesize `pulp_top`~~
6. synthesize and implement the entire board

Refer to `scripts/end_to_end.sh` for a full end-to-end example. The outbut `*.bit` and `*.bin` file can then be used to flash the corresponding FPGA card.

### Simulation

The module hierarchy is structured as follows:

- card top-level (see `capi` repository)
- `pulp_top`
- `hero_powerpulp`
- `pulp_cluster` (multiple)
- `riscv_core` (multiple)

The following testbenches are available:

- `tb_pslse` to simulate `pulp_top` together with the CAPI interface
- `tb_soc` to simulate `hero_powerpulp`, where the host memory is emulated as a regular AXI memory

To compile Xilinx sim libraries (needed only onece), run the script `compile_simlibs.sh`.
To perform a simulation of the PULP system with emulated host, in VSIM source the script `scripts/vsim/compile.tcl` first, and then `scripts/vsim/run_soc.tcl` after changing the binary to load in that script as desired.

In order to run a full simulation with a running pslse on your machine, make sure to have run the `scripts/vsim/compile.tcl` script in your working directory first.
Then, you'll need three terminals to do the following:

1. First, start the RTL simulation with `scripts/vsim/run_pslse.sh` (or `scripts/vsim/run_pslse.tcl` if you're already in vsim), and run it (`run -all`). This will open a server on a local port that a pslse instance can connect to. Simulation time will not advance here unless started through this port.
2. In a second terminal, launch `pslse` which will connect to the simulator and in turn offer the pslse server on another port. See instructions on building pslse in the `capi` repository.
3. Lastly, run your host application in a third

### FPGA Bitstreams

This is a summary table of FPGA implmentations for the kw-Pilot, during the course of the OPRECOMP H2020 project.

| Host    | Transprecision Device | Comm/tion  | FPGA  | Description     | Bitstream | Publication |
| ------- |:---------------------:| ----------:| ----- | --------------- | --------- | ----------- |
| [S822LC-P8](https://www.redbooks.ibm.com/redpapers/pdfs/redp5283.pdf)  | PULP Overlay | [CAPI 1.0](https://www-304.ibm.com/webapp/set2/sas/f/capi/CAPI_POWER8.pdf)   | [AD8K5](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-8k5) | [1cluster 8cores](https://iis-git.ee.ethz.ch/oprecomp/powerpulp/tree/master/mappings/1clu_8core) | [bit](https://github.com/oprecomp/kwpilot-doc/blob/master/bitstreams/power8-pulp-capi1/powerpulp_ad8k5.bit), [bin](https://github.com/oprecomp/kwpilot-doc/blob/master/bitstreams/power8-pulp-capi1/powerpulp_ad8k5.bin) | [COOL CHIPS 2020](https://ieeexplore.ieee.org/document/9097644) |
| [AC922-P9](https://www.ibm.com/products/power-systems-ac922) | Accelerator Overlay | [CAPI 2.0](https://www-355.ibm.com/systems/power/openpower/tgcmDocumentRepository.xhtml?aliasId=CAPI)   | [AD9V3](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-9v3) | [BLSTM](https://github.com/oprecomp/HLS_BLSTM) | bit, bin | [FPT 2018](https://ieeexplore.ieee.org/document/8742271), [COOL CHIPS 2018](https://ieeexplore.ieee.org/document/8373077) | 
| [IC922-P9](https://www.ibm.com/products/power-system-ic922) | Accelerator Overlay | [CAPI 2.0](https://www-355.ibm.com/systems/power/openpower/tgcmDocumentRepository.xhtml?aliasId=CAPI)   | [AD9V3](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-9v3) | [Hello word](https://github.com/oprecomp/oc-accel-tp/tree/master/actions/hls_helloworld_1024) | bit, bin | N/A | 
| [IC922-P9](https://www.ibm.com/products/power-system-ic922) | Accelerator Overlay | [CAPI 2.0](https://www-355.ibm.com/systems/power/openpower/tgcmDocumentRepository.xhtml?aliasId=CAPI)   | [AD9V3](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-9v3) | [Scalar Mult/tion](https://github.com/oprecomp/oc-accel-tp/tree/master/actions/hls_tp_mult) | bit, bin | [ASAP 2019](https://ieeexplore.ieee.org/document/8825124) |
| [IC922-P9](https://www.ibm.com/products/power-system-ic922) | Accelerator Overlay | [CAPI 2.0](https://www-355.ibm.com/systems/power/openpower/tgcmDocumentRepository.xhtml?aliasId=CAPI)   | [AD9V3](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-9v3) | High Order Stencils | bit, bin | [SAMOS 2019](https://link.springer.com/chapter/10.1007%2F978-3-030-27562-4_29) |
| [IC922-P9](https://www.ibm.com/products/power-system-ic922) | Accelerator Overlay | [OpenCAPI 3.0](https://opencapi.org/) | [AD9V3](https://www.alpha-data.com/dcp/products.php?product=adm-pcie-9v3) | PHRYCTORIA | bit, bin | [RAW-IPDPS 2020](https://ieeexplore.ieee.org/document/9150140) | 
| [IC922-P9](https://www.ibm.com/products/power-system-ic922) | DDR4 Controller | [AXI 4](https://developer.arm.com/documentation/ihi0022/h/) | [HTG-830](http://www.hitechglobal.com/boards/Virtex-UltraScale-FPGA.htm) | [DDR4-Controller](https://github.com/oprecomp/DDR4_controller) | [bit, bin](https://github.com/oprecomp/DDR4_controller/tree/main/bitstreams) | N/A | 
