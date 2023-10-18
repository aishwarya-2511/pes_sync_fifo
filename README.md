# pes_sync_fifo
# Synchronous FIFO for Memory Storage 




## Table of Contents
* [Introduction](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#introduction)
* [RTL Design and Synthesis](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#rtl-design-and-synthesis)
    * [iverilog & Yosys Installation on Ubuntu](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#icarus-verilog-iverilog--yosys-installation-on-ubuntu)
    * [RTL Pre-Simulation](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#rtl-pre-simulation)
    * [Synthesis](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#synthesis)
    * [GLS Post-simulation](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#gls-post-simulation)
## Introduction

FIFO is an approach for handling program work requests from queues so that the oldest request is processed first. In the hardware domain it stores data in an array of flops in one clock cycle and can give the same data in another cycle following FIFO logic.

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20182330.png)

The FIFO module is a variable-length buffer with a varying data-width. It also has flags to specify the buffer state. Input port is controlled by write-clock and the data is written into FIFO on every rising edge when it is asserted. Output port is controlled by read-clock and data is read from FIFO on every rising edge when read-enable is asserted.

Applications:
The main uses of FIFO are as follows

Used to buffer data transfers.
Used in circular buffers and FIR filters.
FIFO memory chips.
Increase BW and prevent data loss in high-speed communication.
FIFOs find use in set-top box for HDTV/IPTV.

## RTL Design and Synthesis
## Icarus Verilog (iverilog) & Yosys Installation on Ubuntu

To install iverilog and gtkwave we type the following
```bash
sudo apt-get update
sudo apt-get install iverilog gtkwave
```
To install yosys
```bash
git clone https://github.com/YosysHQ/yosys.git
sudo apt install make
sudo apt-get install build-essential clang bison flex \
 libreadline-dev gawk tcl-dev libffi-dev git \
 graphviz xdot pkg-config python3 libboost-system-dev \
 libboost-python-dev libboost-filesystem-dev zlib1g-dev
```
and then type ```cd yosys``` to go into yosys folder and type 
```bash
sudo make install
```


## RTL Pre-Simulation

To clone this repository:
```bash
git clone https://github.com/aishwarya-2511/pes_sync_fifo.git
```
Details of different directories

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20182018.png)


To Run the .v file and obtain simulation wavefile in gtkwave, type the following commands
```bash
iverilog pes_sync_fifo.v pes_sync_fifo_tb.v
./a.out
gtkwave dump.vcd
```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20125425.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20125343.png)




## Synthesis
Invoke the yosys using following commands
```yosys```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20125711.png)

* Reads the library file from sky130
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

* Reads the verilog file
```bash
read_verilog pes_sync_fifo.v
```
* Synthesize the top module of verilog file
```bash
synth -top iiitb_sfifo
```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175147.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175200.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175206.png)

* Map the FF library file
```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175314.png)

* Generates netlist, simplify and show circuit
```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175329.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20175335.png)

Netlist ckt:

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20181002.png)

zoomed in screenshots:

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180106.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180114.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180122.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180131.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180139.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180146.png)

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180207.png)


* To generate the netlist file
```bash
write_verilog -noattr pes_sync_fifo_net.v
```
* to open generated file
``` !gvim pes_sync_fifo_net.v```

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20180526.png)




## GLS Post-Simulation
Invoke GLS
```bash
iverilog ../verilog_model/primitives.v ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v pes_sync_fifo.v pes_sync_fifo_tb.v
```
![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20193847.png)

Now we type ```./a.out``` to generate the .vcd file.

To see waveform:
``` gtkwave dump.vcd```

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20182001.png)

The synthesis and simulation waveform is matching
