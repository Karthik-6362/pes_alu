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





























