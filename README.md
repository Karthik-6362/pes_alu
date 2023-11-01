# top_module :- pes_alu

# Arithmatic Logic Unit
In order to support CPU for its arithmatic and logical processes, ALU has AU (i.e) arithmatic unit and LU (i.e) Logical Unit. 
We deal with its analysis and functioning of this important unit ofCPU using verilog.
ALU is a processor unit which performs the task of addition, subtraction, multiplication, and division. 

# Introduction
An arithmatic logic unit is an important part of central
processing unit and performs arithmatic and logic operations.
It can take inputs as multiple bits through an input register.
It is the fundamental building block of the central processing
unit of a computer.The ALU is the mathematical brain of a
computer. The first ALU was INTEL 74181 implemented as
a 7400 series is a TTL integrated circuit that was released in
1970.[1]


# Description
This is a ALU model which receives two input operands ’A’
and ’B’ which are 8 bits long. The result is denoted by ’R’
which is also 8 bit long. The input signal ’Op’ is a 3 bit value
which tells the ALU what operation has to be performed by
the ALU. Since ’Op’ is 3 bits long we can have a maximum
of 2 3 = 8 operations.

This ALU is capable of doing the following operations: 
- Add Signed : R = A + B ( Treating A, B, and R as signed two’s complement integers.)
- Subtract Signed : R = A - B (Treating A, B, and R as signed two’s complement integers.)
- Bitwise AND : R(i) = A(i) AND B(i).
- Bitwise NOR : R(i) = A(i)
- NOR B(i). Bitwise OR : R(i) = A(i) OR B(i).
- Bitwise NAND: R(i) = A(i) NAND B(i).
- Bitwise XOR : R(i) = A(i) XOR B(i).
- Biwise NOT : R(i) = NOT A(i).


# Applications :- 
* Processor Applications
* Logical calculations
* Arithmatic Calculations

# RTL Schematic using Quartus:- 
![schematic](https://github.com/Karthik-6362/pes_alu/assets/137412032/708f94d4-7c7f-434d-97fe-8cb3b3bdcb16)

# Synthesis and GLS :- 

**Synthesis:** Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates. 
It is done using YOSYS by following commands.

## Verifying the design before GLS

### Changing directory to the location of the design and testbench files.
```
cd ~/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
### Using iverilog to compile and simulate Verilog source code.
```
iverilog pes_alu.v pes_alu_tb.v -o pes_alu.out
./pes_alu.out 
```
### Using gtkwave to get the output waveforms. 
```
gtkwave pes_alu_out.vcd
```
![gtk before gls](https://github.com/Karthik-6362/pes_alu/assets/137412032/0b0146a9-cc86-4c66-898f-1a3d61579fe1)
![gtkwave ](https://github.com/Karthik-6362/pes_alu/assets/137412032/a0d355d8-3ca6-4a70-885c-2ace10524f61)


## Using YOSYS to do synthesis

#### Invoking yosys :- 
```
cd ~/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```


#### Reading liberty files :- 
```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
#### Reading the design.v file :- 
```
read_verilog pes_alu.v
```
![Invoke yosys, read liberty files and verilog files](https://github.com/Karthik-6362/pes_alu/assets/137412032/670991e8-ab41-45bb-9496-b380d693e815)

#### Synthesizing the design for the top module pes_alu
```
synth -top pes_alu
```
![run_synthesis](https://github.com/Karthik-6362/pes_alu/assets/137412032/e9f4fc44-4322-48d2-806a-d51e0e87fc56)

####  Mapping to mycells.lib and d-flops :- 
```
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![diff lib map](https://github.com/Karthik-6362/pes_alu/assets/137412032/a29b2296-9a14-4162-8c60-3e93774afb07)


#### Getting the netlist file
```
write_verilog -noattr pes_alu_netlist.v
show
exit  // To exit yosys
```
![write verilog](https://github.com/Karthik-6362/pes_alu/assets/137412032/43126b79-fc22-4a4e-932c-d0f38f1805d8)

Netlist :- 
![netlist](https://github.com/Karthik-6362/pes_alu/assets/137412032/28255552-9f0f-4078-891d-fa8ccfe7ad2b)

#### GATE LEVEL SIMULATION(GLS) :- 
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v pes_alu_netlist.v pes_alu_tb.v
./a.out
gtkwave pes_alu_out.vcd
```
![gtk after gls](https://github.com/Karthik-6362/pes_alu/assets/137412032/7a57e5ec-13e8-4239-bce4-1c56b0dbdadd)

#### Before GLS :- 
![gtkwave ](https://github.com/Karthik-6362/pes_alu/assets/137412032/6427cf5b-1c27-454e-89cf-41ee4a0baec5)

#### After GLS :- 
![after gls](https://github.com/Karthik-6362/pes_alu/assets/137412032/cd0c5770-41a1-45f7-a567-aa0d914709f5)




# PHYSICAL DESIGN :- 

OpenLANE is an opensource tool or flow used for opensource tape-outs. The OpenLANE flow comprises a variety of tools such as Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen and Magic which are used to harden chips and macros, i.e. generate final GDSII from the design RTL. The primary goal of OpenLANE is to produce clean GDSII with no human intervention. OpenLANE has been tuned to function for the Google-Skywater130 Opensource Process Design Kit.

**SoC Design & OpenLANE**

Components of opensource digital ASIC design
The design of digital Application Specific Integrated Circuit (ASIC) requires three enablers or elements - Resistor Transistor Logic Intellectual Property (RTL IPs), Electronic Design Automation (EDA) Tools and Process Design Kit (PDK) data.

![soc and openlane](https://github.com/Karthik-6362/pes_pd/assets/137412032/fabbcc1d-4321-449b-ace6-90db2ccaaa8a)


 * Opensource RTL Designs: github, librecores, opencores
 * Opensource EDA tools: QFlow, OpenROAD, OpenLANE
 * Opensource PDK data: Google Skywater130 PDK

The ASIC flow objective is to convert RTL design to GDSII format used for final layout. The flow is essentially a software also known as automated PnR (Place & route).

**Opensource EDA tools**
OpenLANE utilises a variety of opensource tools in the execution of the ASIC flow:

![image](https://user-images.githubusercontent.com/110485513/187844211-8c82e3ce-4549-4d72-9bd1-02322351fb51.png)

OpenLANE design stages
1. Synthesis
     * yosys - Performs RTL synthesis
     * abc - Performs technology mapping
     * OpenSTA - Performs static timing analysis on the resulting netlist to generate timing reports
2. Floorplan and PDN
     * init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
     * ioplacer - Places the macro input and output ports
     * pdn - Generates the power distribution network
     * tapcell - Inserts welltap and decap cells in the floorplan
3. Placement
    * RePLace - Performs global placement
    * Resizer - Performs optional optimizations on the design
    * OpenDP - Perfroms detailed placement to legalize the globally placed components
4. CTS
    * TritonCTS - Synthesizes the clock distribution network (the clock tree)
5. Routing
    * FastRoute - Performs global routing to generate a guide file for the detailed router
    * CU-GR - Another option for performing global routing.
    * TritonRoute - Performs detailed routing
    * SPEF-Extractor - Performs SPEF extraction
6. GDSII Generation
    * Magic - Streams out the final GDSII layout file from the routed def
    * Klayout - Streams out the final GDSII layout file from the routed def as a back-up
7. Checks
    * Magic - Performs DRC Checks & Antenna Checks
    * Klayout - Performs DRC Checks
    * Netgen - Performs LVS Checks
    * CVC - Performs Circuit Validity Checks



<details>
<summary> Tools used for physical Design</summary>

## Python Installation
```
$ sudo apt install -y build-essential python3 python3-venv python3-pip
```
## Docker Installation
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc (removes older version of docker if installed)

$ sudo apt-get update

$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
$ sudo mkdir -p /etc/apt/keyrings

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

$ apt-cache madison docker-ce (copy the version string you want to install)

$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin (paste the version string copies in place of <VERSION_STRING>)

$ sudo docker run hello-world (If the docker is successfully installed u will get a success message here)
```
## OpenLane Installation
```
$ git clone https://github.com/The-OpenROAD-Project/OpenLane.git

$ cd OpenLane/

$ make

$ make test
```
## Magic Installation
For Magic to be installed and work properly the following softwares have to be installed first:
## Installing csh
``` $ sudo apt-get install csh ```
## Installing x11/xorg
``` $ sudo apt-get install x11

$ sudo apt-get install xorg

$ sudo apt-get install xorg openbox
``` 
## Installing GCC
``` $ sudo apt-get install gcc ```
## Installing build-essential
``` $ sudo apt-get install build-essential ```
## Installing OpenGL
``` $ sudo apt-get install freeglut3-dev ```
## Installing tcl/tk
``` $ sudo apt-get install tcl-dev tk-dev ```
## Installing magic
After all the softwares are installed, run the following commands for installing magic:
``` $ git clone https://github.com/RTimothyEdwards/magic

$ cd magic

$ ./configure

$ make

$ make install
```
## Klayout Installation
``` $ sudo apt-get install klayout ```
## ngspice Installation
``` $ sudo apt-get install ngspice ```



</details>





















