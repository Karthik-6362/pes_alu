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



# Tools used for physical Design :-

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



# OpenLANE Flow :-

## Creating a design folder through OpenLane
- To create a design folder with a default config.json file
- ``cd``
- ``cd OpenLane``
- ``make mount``
- ``./flow.tcl -design pes_alu -init_design_config``

- In a new tab,
- ``cd ~/OpenLane/openlane/pes_alu``
- ``mkdir src``
- ``cd src``
- ``gedit pes_alu.v``

![files](https://github.com/Karthik-6362/pes_alu/assets/137412032/0018cbc7-0cc1-431e-81be-d6e7e53d05c6)

```
//Verilog module for an ALU
module pes_alu(
clk,
A,
B,
op,
R   );
    
//inputs,outputs and internal variables declared here
input clk;
input [7:0] A,B;
input [2:0] op;
output [7:0] R;
wire [7:0] Reg1,Reg2;
reg [7:0] Reg3;
    
//Assign A and B to internal variables for doing operations
assign Reg1 = A;
assign Reg2 = B;
//Assign the output 
assign R = Reg3;

//Always block with inputs in the sensitivity list.
always @(posedge clk)
begin
case (op)
0 : Reg3 = Reg1 + Reg2;  //addition
1 : Reg3 = Reg1 - Reg2; //subtraction
2 : Reg3 = ~Reg1;  //NOT gate
3 : Reg3 = ~(Reg1 & Reg2); //NAND gate 
4 : Reg3 = ~(Reg1 | Reg2); //NOR gate               
5 : Reg3 = Reg1 & Reg2;  //AND gate
6 : Reg3 = Reg1 | Reg2;  //OR gate    
7 : Reg3 = Reg1 ^ Reg2; //XOR gate  
endcase 
end
    
endmodule


```


## Start Interactive Mode:
- `./flow.tcl -interactive`
![openlane starting](https://github.com/Karthik-6362/pes_alu/assets/137412032/ee593336-ffbf-4347-83f8-eb472e35a62a)


## To prep the design type:
- `prep -design pes_alu `
![prep design](https://github.com/Karthik-6362/pes_alu/assets/137412032/5054b9a2-ca80-4721-8efd-0666730b2d34)



## Synthesis 
- Synthesis is the process of translating the RTL design description into a gate-level representation using logic gates from a standard cell library.
- `run_synthesis`
![run synthesis](https://github.com/Karthik-6362/pes_alu/assets/137412032/ad41880c-857b-4d6d-863b-51c862d41401)
![ABC results](https://github.com/Karthik-6362/pes_alu/assets/137412032/05b35a42-0ad6-43b3-9450-0b3dff56f846)
![run synthesis op](https://github.com/Karthik-6362/pes_alu/assets/137412032/0f19e91c-28a3-4993-a0d3-4df255504f40)


## Floorplan
- The floorplanning stage involves defining the physical boundaries and locations of different functional blocks within the chip's die area.
- Invoke floorplan using command `run_floorplan`]
![run_floorplan](https://github.com/Karthik-6362/pes_alu/assets/137412032/f2531c31-6cce-477c-90fd-83cf3889686d)
- - view floorplan in Magic
- magic -T /home/desktop/work/tools/openLane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_alu.def &
![floorplan](https://github.com/Karthik-6362/pes_alu/assets/137412032/596b6c50-4696-47b6-a34d-76ba2591c6a7)


## Placement
- Placement is the process of determining the precise locations of individual standard cells within the defined floorplan.
- Invoke placement using command `run_placement`

![run_placement](https://github.com/Karthik-6362/pes_alu/assets/137412032/f02ff75b-7a99-4a0e-b7ee-6c6e74e956a4)
![run_placement op2](https://github.com/Karthik-6362/pes_alu/assets/137412032/00597a70-2623-4750-94c3-4d7e476f8e26)
![run_placement op1](https://github.com/Karthik-6362/pes_alu/assets/137412032/5d3e1e27-94d5-4fda-bc92-d50bbc4fe366)













