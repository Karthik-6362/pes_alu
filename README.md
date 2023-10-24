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

# Synthesis and GLS :- 


