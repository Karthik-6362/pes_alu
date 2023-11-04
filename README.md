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


## Start Interactive Mode or using Automated mode :
![Screenshot from 2023-11-02 19-34-09](https://github.com/Karthik-6362/pes_alu/assets/137412032/820bae18-ab58-4db6-8433-1613b6956ea1)
![Screenshot from 2023-11-02 19-34-30](https://github.com/Karthik-6362/pes_alu/assets/137412032/8191b9e8-fce3-413c-89e7-9316caa2a8be)



- `./flow.tcl -interactive`
![openlane starting](https://github.com/Karthik-6362/pes_alu/assets/137412032/ee593336-ffbf-4347-83f8-eb472e35a62a)


## To prep the design type:
- `prep -design pes_alu `
![prep design](https://github.com/Karthik-6362/pes_alu/assets/137412032/5054b9a2-ca80-4721-8efd-0666730b2d34)



## Synthesis 
- Synthesis is the process of translating the RTL design description into a gate-level representation using logic gates from a standard cell library.
- `run_synthesis`
![run synthesis](https://github.com/Karthik-6362/pes_alu/assets/137412032/ad41880c-857b-4d6d-863b-51c862d41401)
ABC results :-

![ABC results](https://github.com/Karthik-6362/pes_alu/assets/137412032/05b35a42-0ad6-43b3-9450-0b3dff56f846)

Statistics :- 

![run synthesis op](https://github.com/Karthik-6362/pes_alu/assets/137412032/0f19e91c-28a3-4993-a0d3-4df255504f40)


## Floorplan
- The floorplanning stage involves defining the physical boundaries and locations of different functional blocks within the chip's die area.
- Invoke floorplan using command `run_floorplan`]
![run_floorplan](https://github.com/Karthik-6362/pes_alu/assets/137412032/f2531c31-6cce-477c-90fd-83cf3889686d)
- view floorplan in Magic
- `magic -T /home/desktop/work/tools/openLane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_alu.def &`
![floorplan cmds](https://github.com/Karthik-6362/pes_alu/assets/137412032/5495bb0a-e99e-4fb7-809e-5b8d45a4632e)

tkcon:- 

![floorplan specifications](https://github.com/Karthik-6362/pes_alu/assets/137412032/0be18f2d-1f69-4feb-aacf-1e76a26b8348)

Output :- 

![floorplan op](https://github.com/Karthik-6362/pes_alu/assets/137412032/58d90acf-9ac7-4270-a03e-9a44554d930c)



## Placement
- Placement is the process of determining the precise locations of individual standard cells within the defined floorplan.
- Invoke placement using command `run_placement`

  To view in magic :-
```
cd to resluts/placement 
magic -T /home/Desktop/OpenLane_working_die/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_alu.def &
```
![run_placement](https://github.com/Karthik-6362/pes_alu/assets/137412032/8d39ef7f-4f6d-4f21-9782-0414501cf064)

Design Stats:-

![run_placement op1](https://github.com/Karthik-6362/pes_alu/assets/137412032/90e53445-b45a-4938-8dc0-d4c48c698a33)

tkcon:- 

![run_placement op2](https://github.com/Karthik-6362/pes_alu/assets/137412032/fac99071-fecf-4385-bdbc-1d637e83a80c)


## CTS 
- To run CTS we use the command ``` run_cts ```
![cts cmds](https://github.com/Karthik-6362/pes_alu/assets/137412032/4674ee9e-b29f-4e88-8487-e22d6dfa6833)

To view in magic :-
```
cd to resluts/cts
magic -T /home/Desktop/OpenLane_working_die/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_alu.def &
```

![ctsop](https://github.com/Karthik-6362/pes_alu/assets/137412032/964c4749-70d9-488e-833b-606bd39faebf)
![ctsop1](https://github.com/Karthik-6362/pes_alu/assets/137412032/8292123d-3b26-48a8-acb7-42ae6ae2fe1b)
![ctsop2](https://github.com/Karthik-6362/pes_alu/assets/137412032/4d3528f0-3eeb-4417-bd72-e86e573f4f2d)

tkcon :- 

![cts tkcon](https://github.com/Karthik-6362/pes_alu/assets/137412032/2f600553-5099-47eb-b1a5-11bc02711996)

## Routing 

- Command ```run_routing```

To view in magic :-
```
cd to resluts/routing 
magic -T /home/Desktop/OpenLane_working_die/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_alu.def &
```

![routing cmds](https://github.com/Karthik-6362/pes_alu/assets/137412032/a58d91ce-a067-4494-910f-5f5190dfd253)
![routing op](https://github.com/Karthik-6362/pes_alu/assets/137412032/7c9c373b-c932-42e0-9165-e8b35573c49c)
![routing op1](https://github.com/Karthik-6362/pes_alu/assets/137412032/6b50c515-cd41-4ace-a1ed-d5de5f38986b)
![routing op2](https://github.com/Karthik-6362/pes_alu/assets/137412032/b59436af-5fce-4b3a-9e33-1f1dc33e1d94)

# Report 

## CTS
```

===========================================================================
report_checks -path_delay max (Setup)
============================================================================
======================= Typical Corner ===================================

Startpoint: op[0] (input port clocked by clk)
Endpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.09    0.33    3.84 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.09    0.00    3.84 v _228_/A2 (sky130_fd_sc_hd__a31o_1)
     2    0.01    0.05    0.25    4.09 v _228_/X (sky130_fd_sc_hd__a31o_1)
                                         _026_ (net)
                  0.05    0.00    4.09 v _232_/B (sky130_fd_sc_hd__nand3_1)
     3    0.01    0.11    0.12    4.21 ^ _232_/Y (sky130_fd_sc_hd__nand3_1)
                                         _030_ (net)
                  0.11    0.00    4.21 ^ _248_/A2 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.05    0.08    4.29 v _248_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _045_ (net)
                  0.05    0.00    4.29 v _255_/A1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.21    4.50 v _255_/X (sky130_fd_sc_hd__o22a_1)
                                         _127_ (net)
                  0.04    0.00    4.50 v _263_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.50   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.50   data arrival time
-----------------------------------------------------------------------------
                                  5.17   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.09    0.33    3.84 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.09    0.00    3.84 v _228_/A2 (sky130_fd_sc_hd__a31o_1)
     2    0.01    0.05    0.25    4.09 v _228_/X (sky130_fd_sc_hd__a31o_1)
                                         _026_ (net)
                  0.05    0.00    4.09 v _233_/A2 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.03    0.18    4.27 v _233_/X (sky130_fd_sc_hd__a21o_1)
                                         _031_ (net)
                  0.03    0.00    4.27 v _241_/A3 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.03    0.21    4.48 v _241_/X (sky130_fd_sc_hd__a31o_1)
                                         _126_ (net)
                  0.03    0.00    4.48 v _262_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.48   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.48   data arrival time
-----------------------------------------------------------------------------
                                  5.19   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.09    0.33    3.84 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.09    0.00    3.84 v _217_/A2 (sky130_fd_sc_hd__a32o_1)
     1    0.01    0.05    0.28    4.12 v _217_/X (sky130_fd_sc_hd__a32o_1)
                                         _016_ (net)
                  0.05    0.00    4.12 v _218_/B (sky130_fd_sc_hd__xnor2_1)
     1    0.00    0.04    0.12    4.24 v _218_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _017_ (net)
                  0.04    0.00    4.24 v _226_/A2 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.17    4.41 v _226_/X (sky130_fd_sc_hd__o21a_1)
                                         _125_ (net)
                  0.03    0.00    4.41 v _261_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.41   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.41   data arrival time
-----------------------------------------------------------------------------
                                  5.26   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _154_/B_N (sky130_fd_sc_hd__or2b_1)
     1    0.00    0.05    0.18    3.48 ^ _154_/X (sky130_fd_sc_hd__or2b_1)
                                         _077_ (net)
                  0.05    0.00    3.48 ^ _156_/A (sky130_fd_sc_hd__xor2_1)
     1    0.00    0.12    0.14    3.62 ^ _156_/X (sky130_fd_sc_hd__xor2_1)
                                         _079_ (net)
                  0.12    0.00    3.62 ^ _157_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.04    0.05    3.67 v _157_/Y (sky130_fd_sc_hd__nor2_1)
                                         _080_ (net)
                  0.04    0.00    3.67 v _164_/A (sky130_fd_sc_hd__or4_1)
     1    0.00    0.08    0.53    4.19 v _164_/X (sky130_fd_sc_hd__or4_1)
                                         _087_ (net)
                  0.08    0.00    4.19 v _165_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.02    0.10    4.30 v _165_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _121_ (net)
                  0.02    0.00    4.30 v _257_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.30   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.30   data arrival time
-----------------------------------------------------------------------------
                                  5.38   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.09    0.33    3.84 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.09    0.00    3.84 v _203_/B (sky130_fd_sc_hd__nand2_1)
     1    0.01    0.07    0.10    3.94 ^ _203_/Y (sky130_fd_sc_hd__nand2_1)
                                         _003_ (net)
                  0.07    0.00    3.94 ^ _204_/B (sky130_fd_sc_hd__xnor2_1)
     1    0.00    0.06    0.06    4.00 v _204_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _004_ (net)
                  0.06    0.00    4.00 v _211_/A2 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.20    4.20 v _211_/X (sky130_fd_sc_hd__o22a_1)
                                         _124_ (net)
                  0.04    0.00    4.20 v _260_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.20   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.20   data arrival time
-----------------------------------------------------------------------------
                                  5.47   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.09    0.33    3.84 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.09    0.00    3.84 v _188_/A2 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.11    0.14    3.97 ^ _188_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _109_ (net)
                  0.11    0.00    3.97 ^ _198_/A2 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.04    0.12    4.09 ^ _198_/X (sky130_fd_sc_hd__o21a_1)
                                         _123_ (net)
                  0.04    0.00    4.09 ^ _259_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.09   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.04    9.71   library setup time
                                  9.71   data required time
-----------------------------------------------------------------------------
                                  9.71   data required time
                                 -4.09   data arrival time
-----------------------------------------------------------------------------
                                  5.62   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.16 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.16 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.07    0.37    2.53 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.07    0.00    2.53 v _149_/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.09    0.18    2.71 v _149_/X (sky130_fd_sc_hd__clkbuf_2)
                                         _072_ (net)
                  0.09    0.00    2.71 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.05    0.21    2.93 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.05    0.00    2.93 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.37    3.30 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.30 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.21    3.51 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.07    0.00    3.51 v _177_/A (sky130_fd_sc_hd__xor2_1)
     1    0.00    0.08    0.17    3.68 v _177_/X (sky130_fd_sc_hd__xor2_1)
                                         _099_ (net)
                  0.08    0.00    3.68 v _178_/B1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.19    3.87 v _178_/X (sky130_fd_sc_hd__o22a_1)
                                         _122_ (net)
                  0.04    0.00    3.87 v _258_/D (sky130_fd_sc_hd__dfxtp_1)
                                  3.87   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -3.87   data arrival time
-----------------------------------------------------------------------------
                                  5.80   slack (MET)


Startpoint: op[2] (input port clocked by clk)
Endpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[2] (in)
                                         op[2] (net)
                  0.02    0.00    2.01 ^ input19/A (sky130_fd_sc_hd__buf_1)
     4    0.01    0.11    0.13    2.14 ^ input19/X (sky130_fd_sc_hd__buf_1)
                                         net19 (net)
                  0.11    0.00    2.14 ^ _130_/A (sky130_fd_sc_hd__buf_2)
    10    0.03    0.15    0.22    2.35 ^ _130_/X (sky130_fd_sc_hd__buf_2)
                                         _054_ (net)
                  0.15    0.00    2.35 ^ _141_/A_N (sky130_fd_sc_hd__and3b_2)
     8    0.02    0.08    0.29    2.64 v _141_/X (sky130_fd_sc_hd__and3b_2)
                                         _065_ (net)
                  0.08    0.00    2.64 v _142_/B1 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.03    0.17    2.81 v _142_/X (sky130_fd_sc_hd__a21o_1)
                                         _066_ (net)
                  0.03    0.00    2.81 v _143_/A1 (sky130_fd_sc_hd__mux2_1)
     1    0.00    0.05    0.28    3.09 v _143_/X (sky130_fd_sc_hd__mux2_1)
                                         _067_ (net)
                  0.05    0.00    3.09 v _144_/C (sky130_fd_sc_hd__or3_1)
     1    0.00    0.06    0.31    3.40 v _144_/X (sky130_fd_sc_hd__or3_1)
                                         _068_ (net)
                  0.06    0.00    3.40 v _145_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.02    0.10    3.50 v _145_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _120_ (net)
                  0.02    0.00    3.50 v _256_/D (sky130_fd_sc_hd__dfxtp_1)
                                  3.50   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -3.50   data arrival time
-----------------------------------------------------------------------------
                                  6.18   slack (MET)


Startpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[0] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _256_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net20 (net)
                  0.04    0.00    0.35 ^ output20/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output20/X (sky130_fd_sc_hd__buf_2)
                                         R[0] (net)
                  0.17    0.00    0.55 ^ R[0] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[1] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _257_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net21 (net)
                  0.04    0.00    0.35 ^ output21/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output21/X (sky130_fd_sc_hd__buf_2)
                                         R[1] (net)
                  0.17    0.00    0.55 ^ R[1] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[7] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _263_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net27 (net)
                  0.04    0.00    0.35 ^ output27/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output27/X (sky130_fd_sc_hd__buf_2)
                                         R[7] (net)
                  0.17    0.00    0.55 ^ R[7] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[4] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _260_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net24 (net)
                  0.04    0.00    0.35 ^ output24/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output24/X (sky130_fd_sc_hd__buf_2)
                                         R[4] (net)
                  0.17    0.00    0.55 ^ R[4] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[3] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _259_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net23 (net)
                  0.04    0.00    0.35 ^ output23/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output23/X (sky130_fd_sc_hd__buf_2)
                                         R[3] (net)
                  0.17    0.00    0.55 ^ R[3] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[2] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.35    0.35 ^ _258_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net22 (net)
                  0.03    0.00    0.35 ^ output22/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output22/X (sky130_fd_sc_hd__buf_2)
                                         R[2] (net)
                  0.17    0.00    0.55 ^ R[2] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[6] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _262_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net26 (net)
                  0.04    0.00    0.35 ^ output26/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.55 ^ output26/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[6] (net)
                  0.11    0.00    0.55 ^ R[6] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[5] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _261_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net25 (net)
                  0.04    0.00    0.35 ^ output25/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output25/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[5] (net)
                  0.11    0.00    0.54 ^ R[5] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)



worst slack corner Typical: 5.1668



```

===========================================================================
report_checks -path_delay min (Hold)
============================================================================
======================= Typical Corner ===================================

Startpoint: A[0] (input port clocked by clk)
Endpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[0] (in)
                                         A[0] (net)
                  0.01    0.00    2.00 v input1/A (sky130_fd_sc_hd__buf_1)
     4    0.01    0.06    0.10    2.10 v input1/X (sky130_fd_sc_hd__buf_1)
                                         net1 (net)
                  0.06    0.00    2.10 v _138_/A (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.07    0.09    2.19 ^ _138_/Y (sky130_fd_sc_hd__nor2_1)
                                         _062_ (net)
                  0.07    0.00    2.19 ^ _144_/B (sky130_fd_sc_hd__or3_1)
     1    0.00    0.04    0.10    2.29 ^ _144_/X (sky130_fd_sc_hd__or3_1)
                                         _068_ (net)
                  0.04    0.00    2.29 ^ _145_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.04    0.07    2.35 ^ _145_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _120_ (net)
                  0.04    0.00    2.35 ^ _256_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.35   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.35   data arrival time
-----------------------------------------------------------------------------
                                  2.12   slack (MET)


Startpoint: B[7] (input port clocked by clk)
Endpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v B[7] (in)
                                         B[7] (net)
                  0.01    0.00    2.00 v input16/A (sky130_fd_sc_hd__clkbuf_1)
     2    0.00    0.03    0.07    2.08 v input16/X (sky130_fd_sc_hd__clkbuf_1)
                                         net16 (net)
                  0.03    0.00    2.08 v _243_/B (sky130_fd_sc_hd__nand2_1)
     3    0.01    0.07    0.07    2.15 ^ _243_/Y (sky130_fd_sc_hd__nand2_1)
                                         _040_ (net)
                  0.07    0.00    2.15 ^ _254_/A2 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.04    0.11    2.26 ^ _254_/X (sky130_fd_sc_hd__a21o_1)
                                         _051_ (net)
                  0.04    0.00    2.26 ^ _255_/B2 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.10    2.35 ^ _255_/X (sky130_fd_sc_hd__o22a_1)
                                         _127_ (net)
                  0.04    0.00    2.35 ^ _263_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.35   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.35   data arrival time
-----------------------------------------------------------------------------
                                  2.12   slack (MET)


Startpoint: A[1] (input port clocked by clk)
Endpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[1] (in)
                                         A[1] (net)
                  0.01    0.00    2.00 v input2/A (sky130_fd_sc_hd__buf_1)
     5    0.01    0.07    0.11    2.11 v input2/X (sky130_fd_sc_hd__buf_1)
                                         net2 (net)
                  0.07    0.00    2.11 v _162_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.08    0.11    2.22 ^ _162_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _085_ (net)
                  0.08    0.00    2.22 ^ _164_/C (sky130_fd_sc_hd__or4_1)
     1    0.00    0.04    0.10    2.32 ^ _164_/X (sky130_fd_sc_hd__or4_1)
                                         _087_ (net)
                  0.04    0.00    2.32 ^ _165_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.04    0.07    2.38 ^ _165_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _121_ (net)
                  0.04    0.00    2.38 ^ _257_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.38   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.38   data arrival time
-----------------------------------------------------------------------------
                                  2.15   slack (MET)


Startpoint: A[4] (input port clocked by clk)
Endpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[4] (in)
                                         A[4] (net)
                  0.01    0.00    2.00 v input5/A (sky130_fd_sc_hd__buf_2)
     9    0.03    0.07    0.14    2.14 v input5/X (sky130_fd_sc_hd__buf_2)
                                         net5 (net)
                  0.07    0.00    2.14 v _206_/A1 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.08    0.13    2.27 ^ _206_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _006_ (net)
                  0.08    0.00    2.27 ^ _211_/B1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.12    2.39 ^ _211_/X (sky130_fd_sc_hd__o22a_1)
                                         _124_ (net)
                  0.04    0.00    2.39 ^ _260_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.39   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.39   data arrival time
-----------------------------------------------------------------------------
                                  2.16   slack (MET)


Startpoint: A[2] (input port clocked by clk)
Endpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[2] (in)
                                         A[2] (net)
                  0.01    0.00    2.00 v input3/A (sky130_fd_sc_hd__dlymetal6s2s_1)
     6    0.02    0.09    0.16    2.17 v input3/X (sky130_fd_sc_hd__dlymetal6s2s_1)
                                         net3 (net)
                  0.09    0.00    2.17 v _167_/A (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.08    0.11    2.27 ^ _167_/Y (sky130_fd_sc_hd__nor2_1)
                                         _089_ (net)
                  0.08    0.00    2.27 ^ _178_/A1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.13    2.40 ^ _178_/X (sky130_fd_sc_hd__o22a_1)
                                         _122_ (net)
                  0.04    0.00    2.40 ^ _258_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.40   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.40   data arrival time
-----------------------------------------------------------------------------
                                  2.17   slack (MET)


Startpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[2] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _258_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net22 (net)
                  0.02    0.00    0.31 v output22/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output22/X (sky130_fd_sc_hd__buf_2)
                                         R[2] (net)
                  0.09    0.00    0.47 v R[2] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[3] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _259_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net23 (net)
                  0.02    0.00    0.31 v output23/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output23/X (sky130_fd_sc_hd__buf_2)
                                         R[3] (net)
                  0.09    0.00    0.47 v R[3] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[4] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _260_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net24 (net)
                  0.02    0.00    0.31 v output24/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output24/X (sky130_fd_sc_hd__buf_2)
                                         R[4] (net)
                  0.09    0.00    0.47 v R[4] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[7] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _263_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net27 (net)
                  0.02    0.00    0.31 v output27/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output27/X (sky130_fd_sc_hd__buf_2)
                                         R[7] (net)
                  0.09    0.00    0.47 v R[7] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[1] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _257_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net21 (net)
                  0.02    0.00    0.31 v output21/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output21/X (sky130_fd_sc_hd__buf_2)
                                         R[1] (net)
                  0.09    0.00    0.47 v R[1] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[0] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _256_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net20 (net)
                  0.02    0.00    0.31 v output20/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output20/X (sky130_fd_sc_hd__buf_2)
                                         R[0] (net)
                  0.09    0.00    0.47 v R[0] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[5] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.31    0.31 v _261_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net25 (net)
                  0.03    0.00    0.31 v output25/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.17    0.47 v output25/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[5] (net)
                  0.08    0.00    0.47 v R[5] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[6] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.31    0.31 v _262_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net26 (net)
                  0.03    0.00    0.31 v output26/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.17    0.48 v output26/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[6] (net)
                  0.08    0.00    0.48 v R[6] (out)
                                  0.48   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.48   data arrival time
-----------------------------------------------------------------------------
                                  2.23   slack (MET)


Startpoint: A[3] (input port clocked by clk)
Endpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ A[3] (in)
                                         A[3] (net)
                  0.02    0.00    2.01 ^ input4/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.12    0.14    2.15 ^ input4/X (sky130_fd_sc_hd__clkbuf_2)
                                         net4 (net)
                  0.12    0.00    2.15 ^ _195_/A1 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.04    0.13    2.28 ^ _195_/X (sky130_fd_sc_hd__a31o_1)
                                         _116_ (net)
                  0.04    0.00    2.28 ^ _197_/C (sky130_fd_sc_hd__or4b_1)
     1    0.00    0.04    0.09    2.37 ^ _197_/X (sky130_fd_sc_hd__or4b_1)
                                         _118_ (net)
                  0.04    0.00    2.37 ^ _198_/B1 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.04    0.10    2.47 ^ _198_/X (sky130_fd_sc_hd__o21a_1)
                                         _123_ (net)
                  0.04    0.00    2.47 ^ _259_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.47   data arrival time
-----------------------------------------------------------------------------
                                  2.23   slack (MET)


Startpoint: A[6] (input port clocked by clk)
Endpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[6] (in)
                                         A[6] (net)
                  0.01    0.00    2.00 v input7/A (sky130_fd_sc_hd__dlymetal6s2s_1)
     6    0.02    0.10    0.16    2.17 v input7/X (sky130_fd_sc_hd__dlymetal6s2s_1)
                                         net7 (net)
                  0.10    0.00    2.17 v _238_/A1 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.06    0.12    2.29 ^ _238_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _036_ (net)
                  0.06    0.00    2.29 ^ _240_/B (sky130_fd_sc_hd__or3_1)
     1    0.00    0.05    0.10    2.39 ^ _240_/X (sky130_fd_sc_hd__or3_1)
                                         _038_ (net)
                  0.05    0.00    2.39 ^ _241_/B1 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.04    0.08    2.47 ^ _241_/X (sky130_fd_sc_hd__a31o_1)
                                         _126_ (net)
                  0.04    0.00    2.47 ^ _262_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.47   data arrival time
-----------------------------------------------------------------------------
                                  2.24   slack (MET)


Startpoint: A[5] (input port clocked by clk)
Endpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ A[5] (in)
                                         A[5] (net)
                  0.02    0.00    2.01 ^ input6/A (sky130_fd_sc_hd__clkbuf_2)
     9    0.02    0.13    0.15    2.16 ^ input6/X (sky130_fd_sc_hd__clkbuf_2)
                                         net6 (net)
                  0.13    0.00    2.16 ^ _215_/A (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.04    0.06    2.22 v _215_/Y (sky130_fd_sc_hd__nand2_1)
                                         _014_ (net)
                  0.04    0.00    2.22 v _216_/B (sky130_fd_sc_hd__nand2_1)
     1    0.01    0.06    0.07    2.29 ^ _216_/Y (sky130_fd_sc_hd__nand2_1)
                                         _015_ (net)
                  0.06    0.00    2.29 ^ _218_/A (sky130_fd_sc_hd__xnor2_1)
     1    0.00    0.05    0.10    2.39 ^ _218_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _017_ (net)
                  0.05    0.00    2.39 ^ _226_/A2 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.09    2.48 ^ _226_/X (sky130_fd_sc_hd__o21a_1)
                                         _125_ (net)
                  0.03    0.00    2.48 ^ _261_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.48   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02    0.23   library hold time
                                  0.23   data required time
-----------------------------------------------------------------------------
                                  0.23   data required time
                                 -2.48   data arrival time
-----------------------------------------------------------------------------
                                  2.25   slack (MET)



worst slack corner Typical: 2.1175


```





```

### Power Statistics
```

===========================================================================
 report_power
============================================================================
======================= Typical Corner ===================================

Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             3.77e-05   9.64e-07   6.75e-11   3.86e-05  23.3%
Combinational          8.01e-05   4.71e-05   6.19e-10   1.27e-04  76.7%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.18e-04   4.81e-05   6.86e-10   1.66e-04 100.0%
                          71.0%      29.0%       0.0%
	

```

## Routing
```

===========================================================================
report_checks -path_delay max (Setup)
============================================================================
======================= Typical Corner ===================================

Startpoint: op[0] (input port clocked by clk)
Endpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.08    0.31    3.73 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.08    0.00    3.73 v _228_/A2 (sky130_fd_sc_hd__a31o_1)
     2    0.00    0.05    0.23    3.97 v _228_/X (sky130_fd_sc_hd__a31o_1)
                                         _026_ (net)
                  0.05    0.00    3.97 v _232_/B (sky130_fd_sc_hd__nand3_1)
     3    0.01    0.10    0.11    4.08 ^ _232_/Y (sky130_fd_sc_hd__nand3_1)
                                         _030_ (net)
                  0.10    0.00    4.08 ^ _248_/A2 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.05    0.08    4.16 v _248_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _045_ (net)
                  0.05    0.00    4.16 v _255_/A1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.21    4.36 v _255_/X (sky130_fd_sc_hd__o22a_1)
                                         _127_ (net)
                  0.04    0.00    4.36 v _263_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.36   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.36   data arrival time
-----------------------------------------------------------------------------
                                  5.31   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.08    0.31    3.73 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.08    0.00    3.73 v _228_/A2 (sky130_fd_sc_hd__a31o_1)
     2    0.00    0.05    0.23    3.97 v _228_/X (sky130_fd_sc_hd__a31o_1)
                                         _026_ (net)
                  0.05    0.00    3.97 v _233_/A2 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.03    0.18    4.14 v _233_/X (sky130_fd_sc_hd__a21o_1)
                                         _031_ (net)
                  0.03    0.00    4.14 v _241_/A3 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.03    0.21    4.35 v _241_/X (sky130_fd_sc_hd__a31o_1)
                                         _126_ (net)
                  0.03    0.00    4.35 v _262_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.35   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.35   data arrival time
-----------------------------------------------------------------------------
                                  5.32   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.08    0.31    3.73 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.08    0.00    3.73 v _217_/A2 (sky130_fd_sc_hd__a32o_1)
     1    0.00    0.05    0.28    4.01 v _217_/X (sky130_fd_sc_hd__a32o_1)
                                         _016_ (net)
                  0.05    0.00    4.01 v _218_/B (sky130_fd_sc_hd__xnor2_1)
     1    0.00    0.04    0.12    4.13 v _218_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _017_ (net)
                  0.04    0.00    4.13 v _226_/A2 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.16    4.29 v _226_/X (sky130_fd_sc_hd__o21a_1)
                                         _125_ (net)
                  0.03    0.00    4.29 v _261_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.29   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.29   data arrival time
-----------------------------------------------------------------------------
                                  5.38   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _154_/B_N (sky130_fd_sc_hd__or2b_1)
     1    0.00    0.05    0.18    3.40 ^ _154_/X (sky130_fd_sc_hd__or2b_1)
                                         _077_ (net)
                  0.05    0.00    3.40 ^ _156_/A (sky130_fd_sc_hd__xor2_1)
     1    0.00    0.11    0.13    3.53 ^ _156_/X (sky130_fd_sc_hd__xor2_1)
                                         _079_ (net)
                  0.11    0.00    3.53 ^ _157_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.03    0.04    3.58 v _157_/Y (sky130_fd_sc_hd__nor2_1)
                                         _080_ (net)
                  0.03    0.00    3.58 v _164_/A (sky130_fd_sc_hd__or4_1)
     1    0.00    0.08    0.52    4.10 v _164_/X (sky130_fd_sc_hd__or4_1)
                                         _087_ (net)
                  0.08    0.00    4.10 v _165_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.02    0.10    4.20 v _165_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _121_ (net)
                  0.02    0.00    4.20 v _257_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.20   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.20   data arrival time
-----------------------------------------------------------------------------
                                  5.47   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.08    0.31    3.73 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.08    0.00    3.73 v _203_/B (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.06    0.09    3.83 ^ _203_/Y (sky130_fd_sc_hd__nand2_1)
                                         _003_ (net)
                  0.06    0.00    3.83 ^ _204_/B (sky130_fd_sc_hd__xnor2_1)
     1    0.00    0.05    0.06    3.89 v _204_/Y (sky130_fd_sc_hd__xnor2_1)
                                         _004_ (net)
                  0.05    0.00    3.89 v _211_/A2 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.19    4.08 v _211_/X (sky130_fd_sc_hd__o22a_1)
                                         _124_ (net)
                  0.04    0.00    4.08 v _260_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.08   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -4.08   data arrival time
-----------------------------------------------------------------------------
                                  5.59   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _185_/A1 (sky130_fd_sc_hd__a211o_1)
     4    0.01    0.08    0.31    3.73 v _185_/X (sky130_fd_sc_hd__a211o_1)
                                         _106_ (net)
                  0.08    0.00    3.73 v _188_/A2 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.10    0.13    3.86 ^ _188_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _109_ (net)
                  0.10    0.00    3.86 ^ _198_/A2 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.11    3.97 ^ _198_/X (sky130_fd_sc_hd__o21a_1)
                                         _123_ (net)
                  0.03    0.00    3.97 ^ _259_/D (sky130_fd_sc_hd__dfxtp_1)
                                  3.97   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.04    9.71   library setup time
                                  9.71   data required time
-----------------------------------------------------------------------------
                                  9.71   data required time
                                 -3.97   data arrival time
-----------------------------------------------------------------------------
                                  5.74   slack (MET)


Startpoint: op[0] (input port clocked by clk)
Endpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[0] (in)
                                         op[0] (net)
                  0.02    0.00    2.01 ^ input17/A (sky130_fd_sc_hd__clkbuf_2)
     7    0.02    0.11    0.15    2.15 ^ input17/X (sky130_fd_sc_hd__clkbuf_2)
                                         net17 (net)
                  0.11    0.00    2.15 ^ _148_/C_N (sky130_fd_sc_hd__or3b_1)
     1    0.00    0.06    0.35    2.51 v _148_/X (sky130_fd_sc_hd__or3b_1)
                                         _071_ (net)
                  0.06    0.00    2.51 v _149_/A (sky130_fd_sc_hd__buf_2)
     8    0.02    0.06    0.16    2.67 v _149_/X (sky130_fd_sc_hd__buf_2)
                                         _072_ (net)
                  0.06    0.00    2.67 v _150_/C (sky130_fd_sc_hd__and3_1)
     2    0.00    0.04    0.19    2.86 v _150_/X (sky130_fd_sc_hd__and3_1)
                                         _073_ (net)
                  0.04    0.00    2.86 v _153_/B (sky130_fd_sc_hd__or3_1)
     2    0.00    0.07    0.36    3.23 v _153_/X (sky130_fd_sc_hd__or3_1)
                                         _076_ (net)
                  0.07    0.00    3.23 v _173_/A1 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.06    0.20    3.43 v _173_/X (sky130_fd_sc_hd__a21o_1)
                                         _095_ (net)
                  0.06    0.00    3.43 v _177_/A (sky130_fd_sc_hd__xor2_1)
     1    0.00    0.06    0.16    3.59 v _177_/X (sky130_fd_sc_hd__xor2_1)
                                         _099_ (net)
                  0.06    0.00    3.59 v _178_/B1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.04    0.17    3.77 v _178_/X (sky130_fd_sc_hd__o22a_1)
                                         _122_ (net)
                  0.04    0.00    3.77 v _258_/D (sky130_fd_sc_hd__dfxtp_1)
                                  3.77   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -3.77   data arrival time
-----------------------------------------------------------------------------
                                  5.90   slack (MET)


Startpoint: op[2] (input port clocked by clk)
Endpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ op[2] (in)
                                         op[2] (net)
                  0.02    0.00    2.01 ^ input19/A (sky130_fd_sc_hd__buf_1)
     4    0.01    0.09    0.11    2.12 ^ input19/X (sky130_fd_sc_hd__buf_1)
                                         net19 (net)
                  0.09    0.00    2.12 ^ _130_/A (sky130_fd_sc_hd__buf_2)
    10    0.03    0.13    0.20    2.31 ^ _130_/X (sky130_fd_sc_hd__buf_2)
                                         _054_ (net)
                  0.13    0.00    2.31 ^ _141_/A_N (sky130_fd_sc_hd__and3b_2)
     8    0.01    0.07    0.27    2.58 v _141_/X (sky130_fd_sc_hd__and3b_2)
                                         _065_ (net)
                  0.07    0.00    2.58 v _142_/B1 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.03    0.16    2.74 v _142_/X (sky130_fd_sc_hd__a21o_1)
                                         _066_ (net)
                  0.03    0.00    2.74 v _143_/A1 (sky130_fd_sc_hd__mux2_1)
     1    0.00    0.05    0.28    3.02 v _143_/X (sky130_fd_sc_hd__mux2_1)
                                         _067_ (net)
                  0.05    0.00    3.02 v _144_/C (sky130_fd_sc_hd__or3_1)
     1    0.00    0.06    0.31    3.33 v _144_/X (sky130_fd_sc_hd__or3_1)
                                         _068_ (net)
                  0.06    0.00    3.33 v _145_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.02    0.09    3.42 v _145_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _120_ (net)
                  0.02    0.00    3.42 v _256_/D (sky130_fd_sc_hd__dfxtp_1)
                                  3.42   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                                  9.75 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.08    9.67   library setup time
                                  9.67   data required time
-----------------------------------------------------------------------------
                                  9.67   data required time
                                 -3.42   data arrival time
-----------------------------------------------------------------------------
                                  6.25   slack (MET)


Startpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[3] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.35    0.35 ^ _259_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net23 (net)
                  0.03    0.00    0.35 ^ output23/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.17    0.20    0.55 ^ output23/X (sky130_fd_sc_hd__buf_2)
                                         R[3] (net)
                  0.17    0.00    0.55 ^ R[3] (out)
                                  0.55   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.55   data arrival time
-----------------------------------------------------------------------------
                                  7.20   slack (MET)


Startpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[0] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _256_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net20 (net)
                  0.04    0.00    0.35 ^ output20/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output20/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[0] (net)
                  0.11    0.00    0.54 ^ R[0] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[1] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _257_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net21 (net)
                  0.04    0.00    0.35 ^ output21/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output21/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[1] (net)
                  0.11    0.00    0.54 ^ R[1] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[2] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _258_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net22 (net)
                  0.04    0.00    0.35 ^ output22/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output22/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[2] (net)
                  0.11    0.00    0.54 ^ R[2] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[4] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _260_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net24 (net)
                  0.04    0.00    0.35 ^ output24/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output24/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[4] (net)
                  0.11    0.00    0.54 ^ R[4] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[5] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _261_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net25 (net)
                  0.04    0.00    0.35 ^ output25/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output25/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[5] (net)
                  0.11    0.00    0.54 ^ R[5] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[6] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _262_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net26 (net)
                  0.04    0.00    0.35 ^ output26/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output26/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[6] (net)
                  0.11    0.00    0.54 ^ R[6] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)


Startpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[7] (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.35    0.35 ^ _263_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net27 (net)
                  0.04    0.00    0.35 ^ output27/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.11    0.19    0.54 ^ output27/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[7] (net)
                  0.11    0.00    0.54 ^ R[7] (out)
                                  0.54   data arrival time

                  0.15   10.00   10.00   clock clk (rise edge)
                          0.00   10.00   clock network delay (ideal)
                         -0.25    9.75   clock uncertainty
                          0.00    9.75   clock reconvergence pessimism
                         -2.00    7.75   output external delay
                                  7.75   data required time
-----------------------------------------------------------------------------
                                  7.75   data required time
                                 -0.54   data arrival time
-----------------------------------------------------------------------------
                                  7.21   slack (MET)



worst slack corner Typical: 5.3051

```


```

===========================================================================
report_checks -path_delay min (Hold)
============================================================================
======================= Typical Corner ===================================

Startpoint: A[0] (input port clocked by clk)
Endpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[0] (in)
                                         A[0] (net)
                  0.01    0.00    2.00 v input1/A (sky130_fd_sc_hd__buf_1)
     4    0.01    0.05    0.09    2.09 v input1/X (sky130_fd_sc_hd__buf_1)
                                         net1 (net)
                  0.05    0.00    2.09 v _138_/A (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.06    0.08    2.17 ^ _138_/Y (sky130_fd_sc_hd__nor2_1)
                                         _062_ (net)
                  0.06    0.00    2.17 ^ _144_/B (sky130_fd_sc_hd__or3_1)
     1    0.00    0.03    0.09    2.26 ^ _144_/X (sky130_fd_sc_hd__or3_1)
                                         _068_ (net)
                  0.03    0.00    2.26 ^ _145_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.03    0.06    2.33 ^ _145_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _120_ (net)
                  0.03    0.00    2.33 ^ _256_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.33   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.33   data arrival time
-----------------------------------------------------------------------------
                                  2.09   slack (MET)


Startpoint: B[7] (input port clocked by clk)
Endpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v B[7] (in)
                                         B[7] (net)
                  0.01    0.00    2.00 v input16/A (sky130_fd_sc_hd__clkbuf_1)
     2    0.00    0.03    0.07    2.07 v input16/X (sky130_fd_sc_hd__clkbuf_1)
                                         net16 (net)
                  0.03    0.00    2.07 v _243_/B (sky130_fd_sc_hd__nand2_1)
     3    0.01    0.06    0.07    2.14 ^ _243_/Y (sky130_fd_sc_hd__nand2_1)
                                         _040_ (net)
                  0.06    0.00    2.14 ^ _254_/A2 (sky130_fd_sc_hd__a21o_1)
     1    0.00    0.03    0.10    2.24 ^ _254_/X (sky130_fd_sc_hd__a21o_1)
                                         _051_ (net)
                  0.03    0.00    2.24 ^ _255_/B2 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.03    0.09    2.34 ^ _255_/X (sky130_fd_sc_hd__o22a_1)
                                         _127_ (net)
                  0.03    0.00    2.34 ^ _263_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.34   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.34   data arrival time
-----------------------------------------------------------------------------
                                  2.10   slack (MET)


Startpoint: A[2] (input port clocked by clk)
Endpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[2] (in)
                                         A[2] (net)
                  0.01    0.00    2.00 v input3/A (sky130_fd_sc_hd__clkbuf_2)
     6    0.02    0.08    0.12    2.13 v input3/X (sky130_fd_sc_hd__clkbuf_2)
                                         net3 (net)
                  0.08    0.00    2.13 v _167_/A (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.07    0.10    2.22 ^ _167_/Y (sky130_fd_sc_hd__nor2_1)
                                         _089_ (net)
                  0.07    0.00    2.22 ^ _178_/A1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.03    0.12    2.35 ^ _178_/X (sky130_fd_sc_hd__o22a_1)
                                         _122_ (net)
                  0.03    0.00    2.35 ^ _258_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.35   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.35   data arrival time
-----------------------------------------------------------------------------
                                  2.11   slack (MET)


Startpoint: A[1] (input port clocked by clk)
Endpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[1] (in)
                                         A[1] (net)
                  0.01    0.00    2.00 v input2/A (sky130_fd_sc_hd__buf_1)
     5    0.01    0.06    0.10    2.10 v input2/X (sky130_fd_sc_hd__buf_1)
                                         net2 (net)
                  0.06    0.00    2.10 v _162_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.07    0.09    2.19 ^ _162_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _085_ (net)
                  0.07    0.00    2.19 ^ _164_/C (sky130_fd_sc_hd__or4_1)
     1    0.00    0.03    0.09    2.29 ^ _164_/X (sky130_fd_sc_hd__or4_1)
                                         _087_ (net)
                  0.03    0.00    2.29 ^ _165_/A (sky130_fd_sc_hd__clkbuf_1)
     1    0.00    0.03    0.06    2.35 ^ _165_/X (sky130_fd_sc_hd__clkbuf_1)
                                         _121_ (net)
                  0.03    0.00    2.35 ^ _257_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.35   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.35   data arrival time
-----------------------------------------------------------------------------
                                  2.12   slack (MET)


Startpoint: A[6] (input port clocked by clk)
Endpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[6] (in)
                                         A[6] (net)
                  0.01    0.00    2.00 v input7/A (sky130_fd_sc_hd__clkbuf_2)
     6    0.01    0.06    0.11    2.11 v input7/X (sky130_fd_sc_hd__clkbuf_2)
                                         net7 (net)
                  0.06    0.00    2.11 v _238_/A1 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.06    0.11    2.22 ^ _238_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _036_ (net)
                  0.06    0.00    2.22 ^ _240_/B (sky130_fd_sc_hd__or3_1)
     1    0.00    0.04    0.09    2.31 ^ _240_/X (sky130_fd_sc_hd__or3_1)
                                         _038_ (net)
                  0.04    0.00    2.31 ^ _241_/B1 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.03    0.07    2.38 ^ _241_/X (sky130_fd_sc_hd__a31o_1)
                                         _126_ (net)
                  0.03    0.00    2.38 ^ _262_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.38   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.38   data arrival time
-----------------------------------------------------------------------------
                                  2.15   slack (MET)


Startpoint: A[4] (input port clocked by clk)
Endpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 v input external delay
     1    0.00    0.01    0.00    2.00 v A[4] (in)
                                         A[4] (net)
                  0.01    0.00    2.00 v input5/A (sky130_fd_sc_hd__clkbuf_4)
     9    0.03    0.07    0.15    2.15 v input5/X (sky130_fd_sc_hd__clkbuf_4)
                                         net5 (net)
                  0.07    0.00    2.15 v _206_/A1 (sky130_fd_sc_hd__o21ai_1)
     1    0.00    0.07    0.12    2.27 ^ _206_/Y (sky130_fd_sc_hd__o21ai_1)
                                         _006_ (net)
                  0.07    0.00    2.27 ^ _211_/B1 (sky130_fd_sc_hd__o22a_1)
     1    0.00    0.03    0.11    2.39 ^ _211_/X (sky130_fd_sc_hd__o22a_1)
                                         _124_ (net)
                  0.03    0.00    2.39 ^ _260_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.39   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.39   data arrival time
-----------------------------------------------------------------------------
                                  2.15   slack (MET)


Startpoint: A[3] (input port clocked by clk)
Endpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ A[3] (in)
                                         A[3] (net)
                  0.02    0.00    2.01 ^ input4/A (sky130_fd_sc_hd__clkbuf_2)
     8    0.02    0.11    0.13    2.14 ^ input4/X (sky130_fd_sc_hd__clkbuf_2)
                                         net4 (net)
                  0.11    0.00    2.14 ^ _195_/A1 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.03    0.13    2.26 ^ _195_/X (sky130_fd_sc_hd__a31o_1)
                                         _116_ (net)
                  0.03    0.00    2.26 ^ _197_/C (sky130_fd_sc_hd__or4b_1)
     1    0.00    0.04    0.08    2.35 ^ _197_/X (sky130_fd_sc_hd__or4b_1)
                                         _118_ (net)
                  0.04    0.00    2.35 ^ _198_/B1 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.09    2.44 ^ _198_/X (sky130_fd_sc_hd__o21a_1)
                                         _123_ (net)
                  0.03    0.00    2.44 ^ _259_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.44   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.44   data arrival time
-----------------------------------------------------------------------------
                                  2.20   slack (MET)


Startpoint: A[5] (input port clocked by clk)
Endpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          2.00    2.00 ^ input external delay
     1    0.00    0.02    0.01    2.01 ^ A[5] (in)
                                         A[5] (net)
                  0.02    0.00    2.01 ^ input6/A (sky130_fd_sc_hd__clkbuf_2)
     9    0.02    0.11    0.14    2.14 ^ input6/X (sky130_fd_sc_hd__clkbuf_2)
                                         net6 (net)
                  0.11    0.00    2.14 ^ _223_/A1 (sky130_fd_sc_hd__a31o_1)
     1    0.00    0.03    0.13    2.27 ^ _223_/X (sky130_fd_sc_hd__a31o_1)
                                         _022_ (net)
                  0.03    0.00    2.27 ^ _225_/C (sky130_fd_sc_hd__or4b_1)
     1    0.00    0.04    0.08    2.36 ^ _225_/X (sky130_fd_sc_hd__or4b_1)
                                         _024_ (net)
                  0.04    0.00    2.36 ^ _226_/B1 (sky130_fd_sc_hd__o21a_1)
     1    0.00    0.03    0.09    2.45 ^ _226_/X (sky130_fd_sc_hd__o21a_1)
                                         _125_ (net)
                  0.03    0.00    2.45 ^ _261_/D (sky130_fd_sc_hd__dfxtp_1)
                                  2.45   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                                  0.25 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.01    0.24   library hold time
                                  0.24   data required time
-----------------------------------------------------------------------------
                                  0.24   data required time
                                 -2.45   data arrival time
-----------------------------------------------------------------------------
                                  2.21   slack (MET)


Startpoint: _259_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[3] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _259_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _259_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net23 (net)
                  0.02    0.00    0.31 v output23/A (sky130_fd_sc_hd__buf_2)
     1    0.03    0.09    0.16    0.47 v output23/X (sky130_fd_sc_hd__buf_2)
                                         R[3] (net)
                  0.09    0.00    0.47 v R[3] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _256_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[0] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _256_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _256_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net20 (net)
                  0.02    0.00    0.31 v output20/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output20/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[0] (net)
                  0.08    0.00    0.47 v R[0] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _257_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[1] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _257_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _257_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net21 (net)
                  0.02    0.00    0.31 v output21/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output21/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[1] (net)
                  0.08    0.00    0.47 v R[1] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _258_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[2] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _258_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _258_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net22 (net)
                  0.02    0.00    0.31 v output22/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output22/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[2] (net)
                  0.08    0.00    0.47 v R[2] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _260_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[4] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _260_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _260_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net24 (net)
                  0.02    0.00    0.31 v output24/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output24/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[4] (net)
                  0.08    0.00    0.47 v R[4] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _261_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[5] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _261_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _261_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net25 (net)
                  0.02    0.00    0.31 v output25/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output25/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[5] (net)
                  0.08    0.00    0.47 v R[5] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _262_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[6] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _262_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _262_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net26 (net)
                  0.02    0.00    0.31 v output26/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output26/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[6] (net)
                  0.08    0.00    0.47 v R[6] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)


Startpoint: _263_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: R[7] (output port clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.15    0.00    0.00 ^ _263_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.31    0.31 v _263_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         net27 (net)
                  0.02    0.00    0.31 v output27/A (sky130_fd_sc_hd__clkbuf_4)
     1    0.03    0.08    0.16    0.47 v output27/X (sky130_fd_sc_hd__clkbuf_4)
                                         R[7] (net)
                  0.08    0.00    0.47 v R[7] (out)
                                  0.47   data arrival time

                  0.15    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.25    0.25   clock uncertainty
                          0.00    0.25   clock reconvergence pessimism
                         -2.00   -1.75   output external delay
                                 -1.75   data required time
-----------------------------------------------------------------------------
                                 -1.75   data required time
                                 -0.47   data arrival time
-----------------------------------------------------------------------------
                                  2.22   slack (MET)



worst slack corner Typical: 2.0919

```

### Power Statistics
![Screenshot from 2023-11-04 13-04-27](https://github.com/Karthik-6362/pes_alu/assets/137412032/2898d39c-5bc3-4956-8b14-876ca35fa119)












