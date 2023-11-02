# pes_sync_fifo
# Synchronous FIFO for Memory Storage 




## Table of Contents
* [Introduction](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#introduction)
* [RTL Design and Synthesis](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#rtl-design-and-synthesis)
    * [iverilog & Yosys Installation on Ubuntu](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#icarus-verilog-iverilog--yosys-installation-on-ubuntu)
    * [RTL Pre-Simulation](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#rtl-pre-simulation)
    * [Synthesis](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#synthesis)
    * [GLS Post-simulation](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#gls-post-simulation)
* [OpenLane flow](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#openlane-flow)
    * [Installation of ngspice, magic and OpenLane](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#installation-of-ngspice-magic-and-openlane)
    * [Synthesis](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#synthesis-1)
    * [Floorplan](https://github.com/aishwarya-2511/pes_sync_fifo/edit/main/README.md#floorplan)
    * Placement
    * Clock tree synthesis
    * Routing
## Introduction

FIFO is an approach for handling program work requests from queues so that the oldest request is processed first. In the hardware domain it stores data in an array of flops in one clock cycle and can give the same data in another cycle following FIFO logic.

![Alt text](https://github.com/aishwarya-2511/pes_sync_fifo/blob/main/screenshots/Screenshot%202023-10-18%20182330.png)

The FIFO module is a variable-length buffer with a varying data-width. It also has flags to specify the buffer state. Input port is controlled by write-clock and the data is written into FIFO on every rising edge when it is asserted. Output port is controlled by read-clock and data is read from FIFO on every rising edge when read-enable is asserted.

Applications:
The main uses of FIFO are as follows

* Used to buffer data transfers.
* Used in circular buffers and FIR filters.
* FIFO memory chips.
* Increase bandwidth and prevent data loss in high-speed communication.
* FIFOs find use in set-top box for HDTV/IPTV.

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

## OpenLane flow

## Installation of ngspice, magic and OpenLane

* Download the tarball from https://sourceforge.net/projects/ngspice/files/ 
### ngspice
* open terminal and type the following commands
```bash
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```
### magic
```bash
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```
### OpenLane
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
```

### PDKs and Tools
```bash 
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

### To invoke OpenLane
* Go to "designs" folder under OpenLane
* create a folder - 'pes_sync_fifo'
* create config.json file by running ``` ./flow.tcl -design pes_cache_compression -init_design_config -add_to_designs ```
* create a folder src and add your design file & these files: sky130_fd_sc_hd_fast.lib, sky130_fd_sc_hd_slow.lib, sky130_fd_sc_hd_typical.lib 

detailed view of directory structure:
![detailed_dir](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/ade3bcdf-d417-44b8-93a9-c185bb7a970f)


* in the main openlane folder create folder 'pdks' and paste the file 'sky130_fd_sc_hd.v'
![pdks](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/3e5525c8-8a89-42fc-84d4-bd692f2a653a)

* Type ```make mount```
* Type ./flow.tcl -interactive in openlane directory

  <img width="584" alt="Screenshot 2023-11-02 230154" src="https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/a6659a18-62b4-4bc9-a1d7-f93dfd5b92b0">

* Type ```prep -design pes-sync_fifo```
  
![prepdesign](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/1fe4bd2b-f6df-47f7-a0dd-2585cb848277)


## Synthesis
Type ```run_synthesis```
![synthesis1a](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/c48ccd40-6453-4e07-b051-36ed10d90483)

![synthesis1](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/149e3b8c-bcd0-4878-bc7f-638f32f34030)


![pre stat](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/0a8ac58c-2a28-402c-b530-fd0f5b779a15)
![dff stat](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/d5f8b6f4-d8b9-471d-a56f-0449bf74de4a)
![4stat](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/b473ed6c-71e1-4cb6-8888-27a25da04812)
![minmax](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/43033f09-18b6-4b92-81a0-df030b92bd50)

## Floorplan
Type ```run_floorplan```
![floorplann](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/d30fa406-8572-46ae-92d0-2439447b3763)

Die and core area:
![diearea](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/3bbeacf3-4996-43f3-9962-c32972ab1a25)
![corearea](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/2f18cfec-1b99-4b28-89f7-63f41fe5b6f0)

To open in magic tool type:
``` magic -T /home/Desktop/work/tools/openlane-working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech read lef ../../tmp/merged_unpadded.lef def spm.floorplan.def &```

![magicfloorplan](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/411f11ad-0e5a-4f52-af19-dfc8d2df4ad7)

![zoom1](https://github.com/aishwarya-2511/pes_sync_fifo/assets/97291384/df768ea9-4e10-4fbf-b761-945536eba19a)


