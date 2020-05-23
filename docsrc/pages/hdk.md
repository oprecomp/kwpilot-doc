# kwHDK

The repository with the approrpiate files for kwHDK is [PowerPULP](https://iis-git.ee.ethz.ch/oprecomp/powerpulp).

This repository contains PowerPULP, a PULP system attached to a IBM POWER8/9 machine via the CAPI interface. We currently support the following combinations:

- board `ad8k5`, design `xcku115`, part `xcku115-flva1517-2-e`
- board `adku3`, design `xcku060`, part `xcku060-ffva1156-2-e`

## Getting Started

This project uses bender [1] for dependency management (kind of), so make sure that is installed if you want to change IPs. To get started, do the following:

Set the following environment variables to something reasonable for your setup:

- `export VSIM="vsim-10.7b -64"`
- `export VLOG="vlog-10.7b -64"`
- `export VCOM="vcom-10.7b -64"`
- `export VOPT="vopt-10.7b -64"`
- `export VIVADO="vivado-2019.1.1 vivado"`

## Flow

To synthesize and implement the design for an FPGA, proceed as follows:

1. generate the IPs needed for `capi_adapter`
2. ~~synthesize `capi_adapter`~~
3. ~~synthesize `pulp_cluster`~~
4. ~~synthesize `pulp_soc`~~
5. ~~synthesize `pulp_top`~~
6. synthesize and implement the entire board

Refer to `scripts/end_to_end.sh` for a full end-to-end example. The outbut `*.bit` and `*.bin` file can then be used to flash the corresponding FPGA card.

## Simulation

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