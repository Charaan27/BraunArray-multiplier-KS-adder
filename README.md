# BraunArray-multiplier-KS-adder

This repository contains a detailed report on the design of a 4x4 Braun Array Multiplier, with a 3-bit Kogge-Stone Adder. The design and simulation was done using **Synopsys Custom Design Compiler** and the modeling was done using **Synopsys SAED 32nm CMOS technology process**, for the event [_Cloud Based Analog IC Design Hackathon_](https://hackathoniith.in) conducted by the [Department of Electrical Engineering at IIT Hyderabad](https://ee.iith.ac.in), and sponsored by [Synopsys India](https://www.synopsys.com/company/contact-synopsys/office-locations/india/about-synopsys-india.html) and [VLSI System Design (VSD) Corp.](https://www.vlsisystemdesign.com)

# Contents
- [Introduction](#Introduction)  
- [Reference Circuit](#Reference-Circuit)  
- [Reference Waveform](#Reference-Waveform)  
- [Tools used](#Tools-used)  
- [Subcircuits](#Subcircuits)  
  - [2-input AND Gate](#2-input-AND-Gate)  
  - [2-input OR Gate](#2-input-OR-Gate)  
  - [2-input XOR Gate](#2-input-XOR-Gate)  
  - [Half Adder](#Half-Adder)  
  - [Full Adder](#Full-Adder)  
  - [3-bit Kogge-Stone Adder](#3-bit-Kogge-Stone-Adder)
- [Braun Array Multiplier](#Braun-Array-Multiplier)
  - [Circuit Schematic](#Circuit-Schematic)
  - [Netlist](#Netlist)
  - [Simulation and waveform results](#Simulation-and-waveform-results)
 - [Acknowlegdements](#Acknowlegdements)
 - [Author](#Author)
 - [References](#References)

# Introduction
The need for efficient binary multipliers that could be incorporated in Digital ICs has seen an unprecendented rise over the last few years. The conventional architecture of a binary multiplier involves a series of AND gates that will generate what is called as a _partial product_, and later these are added up with each other to produce the product. This corresponds to a variety of binary multipliers known as **parallel array multipliers**. However, this is an extensively computational process, when compared with other binary operations. But, binary multiplication is a very important arithmetic operation, which is primarily being used in Digital Signal Processing applications. For example, the _Multiply and Accumulate (MAC)_ operation is used in implementing Finite Impulse Response (FIR) Filters, and other DSP algorithms. Hence it becomes important to find out ways to optimize the architecure of binary multipliers thus increasing its efficiency, while decreasing its area occupancy, computation time and power dissipation. One of the efficient parallel array multipliers that is widely used for this purpose is the **Braun Array Multiplier**. It is a type of a high-speed parallel array multiplier that comes with a relatively lesser area and delay, thus making it one of the ideal array multipliers to use. Braun Array Multipliers require a **carry look-ahead adder** in the design. In order to improve the speed and efficiency of the multiplier, a **Kogge-Stone Adder** has been implemented in the place of the carry look-ahead adder. It is a form of a parallel-prefix adder that comes with a lower fan-out at each of its stages, thus increasing the performance for typical CMOS processes.

# Reference Circuit
An n-bit Braun Array Multiplier is realised using (n−1)<sup>2</sup> full adders, n<sup>2</sup> AND gates and a (n−1) bit carry look-ahead adder. Hence for a 4-bit Braun Array Multiplier, we require **9 Full Adders, 16 Logic AND gates and a 3-bit Kogge-Stone Adder**. The reference circuit of the same is shown below (image taken from [here](https://ieeexplore.ieee.org/document/8389113) - cited as reference 2 in [references](#References) section).
<p align="center">
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/references/reference_circuit.png">
</p>

# Reference Waveform
For a n\*m multiplication, the number of bits in the product cannot be less than max(m,n) and cannot be more than (m+n), unless one of the two numbers is zero. If the two operands are of similar size (i.e.) n\*n, then the product is of size 2n. Hence for a 4-bit multiplication, the operands are of size 4-bits, while the product is of size 8-bits. In total we will have 8 waveforms for the input (4 + 4 inputs) and 8 waveforms for the output, leading to a total of 16 waveforms. The reference waveform for 4-bit multiplication could be found below (image taken from [here](https://ieeexplore.ieee.org/document/8869307) - cited as reference 3 in [references](#References) section).  

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/references/reference_waveform.png">
</p>

# Tools used

- OS: [CentOS Linux 7.9.2009](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7.2009#Packages_released_as_7.8.2003_updates_with_older_packages_on_the_7.9.2009_install_media) - An open-sourced Linux Distribution, initially developed by 'The CentOS Project' and currently affliated with Red Hat. 
- Design Platform: [Synopsys Custom Design Platform](https://www.synopsys.com/implementation-and-signoff/custom-design-platform.html) - The Synopsys Custom Design Platform is a unified suite of design and verification tools that accelerates the development of robust analog and mixed-signal designs. The platform features [Custom Compiler™](https://www.synopsys.com/implementation-and-signoff/custom-design-platform/custom-compiler.html), a fast, easy-to-use design, and layout solution, [PrimeSim™ Continuum](https://www.synopsys.com/implementation-and-signoff/ams-simulation.html), which delivers industry-leading circuit simulation performance, and best-in-class technologies for parasitic extraction, reliability analysis, and physical verification. 
- Simulation Software: [PrimeSim HSPICE](https://www.synopsys.com/implementation-and-signoff/ams-simulation/primesim-hspice.html) PrimeSim HSPICE provides accurate circuit simulation and offers foundries-certified MOS device models with state-of-the-art simulation and analysis algorithms. With extensive usage in chip/package/board/backplane signal integrity simulation, cell and memory characterization, and analog mixed signal IC design, PrimeSim HSPICE is a powerful, trusted and comprehensive circuit simulator.
- CMOS Process used for device modeling: [SAED 32/28nm PDK](https://www.synopsys.com/community/university-program/teaching-resources.html).

# Subcircuits
This section follows the list of subcircuits designed for the multiplier, along with their respective symbols

## 2-input AND gate
It is one of the digital logic gates that implements logical conjunction. That is to say, the output of an AND gate is HIGH only if all of it's inputs ar HIGH, else it is LOW. A CMOS AND gate consists of three sections - PMOS, NMOS and Inverter. Each of the two inputs is given to the PMOS and NMOS section respectively, and the output is taken from the inverter section. The schematic of the implemented 2-input CMOS Logic AND gate is as follows. along with the symbol constructed and the output. It is observed that the output of the implemented AND gate is HIGH only when all the inputs are HIGH. The [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/cmosAND_netlist.spi) of the design is given here. 
<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/cmosAND/cmosAND_final.png">
  <br>
</p>


## 2-input OR gate
It is one of the digital logic gates that implements logical disjunction. That is to say, the output of an OR gate is LOW only if all of it's inputs ar LOW, else it is HIGH. A CMOS OR gate consists of three sections - PMOS, NMOS and Inverter. Each of the two inputs is given to the PMOS and NMOS section respectively, and the output is taken from the inverter section. The schematic of the implemented 2-input CMOS Logic OR gate is as follows. along with the symbol constructed and the output. It is observed that the output of the implemented OR gate is LOW only when all the inputs are LOW. The [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/cmosOR_netlist.spi) of the design is given here.

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/cmosOR/cmosOR_final.png">
  <br>
</p>

## 2-input XOR gate
It is one of the digital logic gates that implements the inequality function. That is to say, the output of an XOR gate is HIGH only if the inputs are not alike, else it is LOW. The schematic of the implemented 2-input CMOS Logic XOR gate is as follows. along with the symbol constructed and the output. It is observed that the output of the implemented XOR gate is HIGH only when all the inputs are not alike. The [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/cmosXOR_netlist.spi) of the given design is implemented here.

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/cmosXOR/cmosXOR_final.png">
  <br>
</p>

## Half Adder
The half adder is a type of binary adder that adds two binary digits, and produces two outputs which are the SUM and CARRY. The SUM is the output of the XOR gates when the two operands ae passed as inputs, and the CARRY is the output of the AND operation of the two operands. Hence the circuit contains an XOR and an AND gate. The circuit, along with the symbol and the waveform are given below. It is observed that the SUM is zero only if either both the operands are 0 or 1 (indicating an overflow). The carry is zero for all cases except for inputs 1 and 1. The [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/halfadder_netlist.spi) is attached herewith.


<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/half_adder/halfadder_final.png">
  <br>
</p>

## Full Adder
A full adder is yet another type of binary multiplier, which also accounts for the values carried in as well as carried out, along with the binary operands. A one-bit full adder, has three inputs - A, B and C<sub>in</sub>, and gives out two outputs SUM and C<sub>out</sub>. To add two operands of size n-bits, we will require n full adders, where each full adder corresponds to the addition of each bit of the two operands. A full adder circuit can be realized using two half adders and one XOR gate. The circuit, along with the symbol, waveform and [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/fulladder_netlist.spi) are given.

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/full_adder/fulladder_final.png">
  <br>
</p>

## 3-bit Kogge-Stone Adder
Adders form a crucial part in every multiplier circuit. In this multiplier, a Kogge-Stone Adder has been implemented. The concept of this multiplier was developed by two scientists [Peter M. Kogge](https://en.wikipedia.org/wiki/Peter_Kogge) and [Harold S. Stone](https://en.wikipedia.org/wiki/Harold_S._Stone). KS-Adder is a **parallel-prefix** form **carry look-ahead adder**. It generates carry in O(log n) time and is widely considered as one of the fastest adders and is widely used as high performance arithmetic circuits. Parallel prefix adders are indicative of the fact that the carries generated at each stage are computed in parallel. 

### Theory
The theory of KS-Adder can be categorized under three stages:

1. **Pre- Processing**:  
This step involves computation of generate and propagate signals corresponding to each pair of bits in A and B. These signals are given by the logic equations below:  

    ```
    pi  = Ai  xor  Bi
    gi  = Ai  and  Bi
    ```

2. **Carry Look Ahead Adder network**:  
This block differentiates KSA from other adders and is the  main force behind its high performance. This step  involves computation of  carries corresponding  to each  bit.  It uses group propagate and generate as intermediate signals which are given by the logic equations below: 

    ```
    Pi:j   = Pi:k+1 and Pk:j 
    Gi:j   = Gi:k+1 or (Pi:k+1 and Gk:j)
    ```
    
3. **Post-Processing**:  
This is the final step and is common to all adders of this family (carry look ahead). It involves computation of sum  bits. Sum  bits are computed by the logic given below:  

    ```
    Si  = pi  xor Ci-1 
    ```
    
### Illustration
The illustration of a 3-bit Kogge-Stone Adder is given below: (image taken from [here](https://www.researchgate.net/figure/3-bit-Kogge-stone-adder_fig1_269310519) - cited as reference 4 in [references](#References) section)  

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/ks_adder/3-bit-Kogge-stone-adder.png">
  <br>
</p>  

The circuit, along with the symbol, waveform and [netlist](https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/netlists/ks_adder_netlist.spi) are given here.

<p align="center">
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/ks_adder/ks_adder_final.png">
  <br>
</p>  

# Braun Array Multiplier  
Having done the required subcircuits, we now proceed with the design of the multiplier itself. The design comprises an array of AND Gates arranged in an iterative manner, and the output of the AND gates are connected with Full Adders. For the design of a 4x4 Braun Array Multiplier we need a total of **9 Full Adders, 16 Logic AND gates and a 3-bit Kogge-Stone Adder**. The partial products at each stage are generated by the AND Gates. Each of these partial products are added with the sum of the previously produced partial products by the row of full adders. The carry out is shifted one bit either to the left or to the right and then added to the sum which was generated by the first adder and the earlier generated partial product. The last stage of shifting needs a paralle-prefix adder hence we use the 3-bit Kogge Stone Adder. 

## Circuit Schematic
The circuit design and the corresponding symbol can be found below. The Schematic and Symbol were constructed with the help of **Synopsys Custom Compiler**.

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/braun_multiplier/braun_multiplier_final.PNG">
  <br>
</p>  

## Netlist:
The Netlist for the designed circuit is generated after simulating the circuit testbench. The Netlist is obtained using **PrimeSim HSPICE**.

```
*  Generated for: PrimeSim
*  Design library name: braun_mult
*  Design cell name: braun_multiplier_tb
*  Design view name: schematic
.lib 'saed32nm.lib' TT

*Custom Compiler Version S-2021.09
*Fri Feb 25 18:43:12 2022

.global gnd!
********************************************************************************
* Library          : braun_mult
* Cell             : cmosAND
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt cmosand a_in b_in y_out
xm2 y_out net22 vdd vdd p105 w=0.1u l=0.03u nf=1 m=1
xm1 net22 b_in vdd vdd p105 w=0.1u l=0.03u nf=1 m=1
xm0 net22 a_in vdd vdd p105 w=0.1u l=0.03u nf=1 m=1
xm5 net23 b_in gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm4 y_out net22 gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm3 net22 a_in net23 net23 n105 w=0.1u l=0.03u nf=1 m=1
v9 vdd gnd! dc=1.8
.ends cmosand

********************************************************************************
* Library          : braun_mult
* Cell             : cmosXOR
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt cmosxor a_in b_in y_out
xm15 y_out net69 net63 net63 p105 w=0.1u l=0.03u nf=1 m=1
xm7 net29 a_in net69 net29 p105 w=0.1u l=0.03u nf=1 m=1
xm6 net69 net13 net29 net29 p105 w=0.1u l=0.03u nf=1 m=1
xm5 net63 net50 net29 net63 p105 w=0.1u l=0.03u nf=1 m=1
xm4 net29 b_in net63 net63 p105 w=0.1u l=0.03u nf=1 m=1
xm2 net13 b_in net63 net63 p105 w=0.1u l=0.03u nf=1 m=1
xm0 net50 a_in net63 net63 p105 w=0.1u l=0.03u nf=1 m=1
xm16 y_out net69 gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm11 gnd! a_in net44 gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm10 net47 net50 gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm9 net44 net13 net69 net44 n105 w=0.1u l=0.03u nf=1 m=1
xm8 net69 b_in net47 net47 n105 w=0.1u l=0.03u nf=1 m=1
xm3 net13 b_in gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm1 net50 a_in gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
v17 net63 gnd! dc=1.8
.ends cmosxor

********************************************************************************
* Library          : braun_mult
* Cell             : half_adder
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt half_adder carry sum x y
xi0 x y sum cmosxor
xi1 x y carry cmosand
.ends half_adder

********************************************************************************
* Library          : braun_mult
* Cell             : full_adder
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt full_adder c_in carry sum x_in y_in
xi1 net20 sum net22 c_in half_adder
xi0 net21 net22 x_in y_in half_adder
xi2 net21 net20 carry cmosxor
.ends full_adder

********************************************************************************
* Library          : braun_mult
* Cell             : cmosOR
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt cmosor a_in b_in out
xm4 out net25 net17 net17 p105 w=0.1u l=0.03u nf=1 m=1
xm2 net10 b_in net25 net10 p105 w=0.1u l=0.03u nf=1 m=1
xm1 net10 a_in net17 net17 p105 w=0.1u l=0.03u nf=1 m=1
xm6 net25 a_in gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm5 out net25 gnd! gnd! n105 w=0.1u l=0.03u nf=1 m=1
xm7 gnd! b_in net25 gnd! n105 w=0.1u l=0.03u nf=1 m=1
v8 net17 gnd! dc=1.8
.ends cmosor

********************************************************************************
* Library          : braun_mult
* Cell             : ks_adder
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt ks_adder a0 a1 a2 b0 b1 b2 cin cout s0 s1 s2
xi21 net129 net116 s2 cmosxor
xi20 net130 net115 s1 cmosxor
xi19 cin net128 s0 cmosxor
xi12 a2 b2 net116 cmosxor
xi5 a1 b1 net115 cmosxor
xi0 a0 b0 net128 cmosxor
xi17 net121 net130 net76 cmosand
xi15 net115 net116 net121 cmosand
xi14 net116 net123 net125 cmosand
xi13 a2 b2 net127 cmosand
xi10 net120 cin net117 cmosand
xi8 net128 net115 net120 cmosand
xi7 net115 net122 net126 cmosand
xi6 a1 b1 net123 cmosand
xi2 cin net128 net124 cmosand
xi1 a0 b0 net122 cmosand
xi18 net76 net119 cout cmosor
xi16 net125 net127 net119 cmosor
xi11 net117 net118 net129 cmosor
xi9 net126 net123 net118 cmosor
xi3 net124 net122 net130 cmosor
.ends ks_adder

********************************************************************************
* Library          : braun_mult
* Cell             : braun_multiplier
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt braun_multiplier p0 p1 p2 p3 p4 p5 p6 p7 x0 x1 x2 x3 y0 y1 y2 y3
xi32 x0 y3 net174 cmosand
xi31 x1 y3 net170 cmosand
xi30 x2 y3 net81 cmosand
xi5 x2 y1 net156 cmosand
xi18 x3 y3 net105 cmosand
xi14 x0 y2 net166 cmosand
xi13 x1 y2 net164 cmosand
xi12 x2 y2 net162 cmosand
xi11 x3 y2 net83 cmosand
xi7 x0 y1 net160 cmosand
xi6 x1 y1 net158 cmosand
xi4 x3 y1 net56 cmosand
xi3 x0 y0 p0 cmosand
xi2 x1 y0 net39 cmosand
xi1 x2 y0 net34 cmosand
xi0 x3 y0 net29 cmosand
xi24 net89 net96 p3 net174 net93 full_adder
xi23 net84 net97 net101 net170 net88 full_adder
xi22 net79 net99 net103 net81 net83 full_adder
xi17 net62 net89 p2 net166 net94 full_adder
xi16 net57 net84 net93 net164 net61 full_adder
xi15 net52 net79 net88 net162 net56 full_adder
xi10 gnd! net62 p1 net160 net39 full_adder
xi9 gnd! net57 net94 net158 net34 full_adder
xi8 gnd! net52 net61 net156 net29 full_adder
xi25 net96 net97 net99 net101 net103 net105 gnd! p7 p4 p5 p6 ks_adder
.ends braun_multiplier

********************************************************************************
* Library          : braun_mult
* Cell             : braun_multiplier_tb
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
xi0 p0 p1 p2 p3 p4 p5 p6 p7 x0 x1 x2 x3 y0 y1 y2 y3 braun_multiplier
v8 y3 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 5u 10u )
v7 y2 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 40u 80u )
v6 y1 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 20u 40u )
v5 y0 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 10u 20u )
v4 x3 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 5u 10u )
v3 x2 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 10u 20u )
v2 x1 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 20u 40u )
v1 x0 gnd! dc=0 pulse ( 0 1.8 0 0.1n 0.1n 40u 80u )








.tran '0.1u' '100u' name=tran

.option primesim_remove_probe_prefix = 0
.probe v(*) i(*) level=1
.probe tran v(p0) v(p1) v(p2) v(p3) v(p4) v(p5) v(p6) v(p7) v(x0) v(x1) v(x2)
+ v(x3) v(y0) v(y1) v(y2) v(y3)

.temp 25



.option primesim_output=wdf


.option parhier = LOCAL






.end
```

## Simulation and waveform results:
The simulation of the circuit testbench is done using **PrimeSim HSPICE**. Two binary numbers of size 4-bits are given to the multiplier as inputs (through pulse generators). The obtained output is an 8-bit product, and is compared with the reference waveform for the correct working and functionality. The obtained input and output waveforms are given below.

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/braun_multiplier/braun_multiplier_waveform_final.png">
  <br>
</p>

By comparing the obtained waveforms with the [reference waveform](#Reference-Waveform) added earlier, we can conclude that the designed multiplier circuit is working as expected, as is giving out the correct waveform result. 

```
NOTE: The input and output waveforms must be interpreted backwards (i.e.) from X3-X1, Y3-Y1 for inputs and P7-P0 for output.
```

# Acknowlegdements:
- [Mr. Kunal Ghosh](https://www.vlsisystemdesign.com/about-us/) - Co-Founder at VLSI System Design (VSD) Corp.
- [Mr. Chinmaya Panda](https://ee.iith.ac.in/staff.html) - Technical Officer, Department of Electrical Engineering, IIT Hyderabad
- [Mr. Sameer Durgoji](https://www.udemy.com/user/sameer-s-durgoji/) - Final year student - BTech ECE at NIT Karnataka
- [Synopsys India](https://www.synopsys.com/company/contact-synopsys/office-locations/india/about-synopsys-india.html)
- [Mr. Sumanto Kar](https://www.linkedin.com/in/sumanto-kar-0424391a9/?originalSubdomain=in) - Sr. Project Technical Assistant, IIT Bombay
- [Mr. Mukesh Nadar](https://www.linkedin.com/in/mukesh-nadar-b310761b2/?originalSubdomain=in) - Senior Web developer at ISF Analytica and Informatica

# Author:
Charaan S, Pre-Final year student, B.E. ECE, Madras Institute of Technology, Anna University, Chennai, India

# References:
1. Shinde, Kunjan & Kumar, K. & Rashmi, D. & Rukhsar, R. & Shilpa, H. & Vidyashree, C.. (2018). A Novel Approach to Design Braun Array Multiplier Using Parallel Prefix Adders for Parallel Processing Architectures: Second International Conference, ICSCS 2018, Kollam, India, April 19–20, 2018, Revised Selected Papers. [10.1007/978-981-13-1936-5_62.](https://www.researchgate.net/publication/327856495_A_Novel_Approach_to_Design_Braun_Array_Multiplier_Using_Parallel_Prefix_Adders_for_Parallel_Processing_Architectures_Second_International_Conference_ICSCS_2018_Kollam_India_April_19-20_2018_Revised_Se)
2. A. Raju and S. K. Sa, "Design and performance analysis of multipliers using Kogge Stone Adder," 2017 3rd International Conference on Applied and Theoretical Computing and Communication Technology (iCATccT), 2017, pp. 94-99, [doi: 10.1109/ICATCCT.2017.8389113.](https://ieeexplore.ieee.org/document/8389113)
3. B. Neeraja and R. S. P. Goud, "Design of an Area Efficient Braun Multiplier using High Speed Parallel Prefix Adder in Cadence," 2019 IEEE International Conference on Electrical, Computer and Communication Technologies (ICECCT), 2019, pp. 1-5, [doi: 10.1109/ICECCT.2019.8869307.](https://ieeexplore.ieee.org/document/8869307)
4. A. Sindhu, A. Bhatia, "8 Bit Kogge Stone Adder," [Project Report, Indian Institute of Technology, Kanpur](https://www.cl.cam.ac.uk/research/srg/han/ACS-P35/8-bit-KoggeStone-Adder.pdf)
5. Rengasamy, Dr. Dhanabal & V, Barathi & Sahoo, Sarat Kumar & Samhitha, Naamatheertham & Cherian, Neethu & Jacob, Pretty. (2014). Implementation of floating point MAC using Residue Number System. Journal of Theoretical and Applied Information Technology. 62. 461-465. [10.1109/ICROIT.2014.6798385](https://www.researchgate.net/publication/269310519_Implementation_of_floating_point_MAC_using_Residue_Number_System). 
6. Kogge-Stone Adder - [Wikipedia Article](https://en.wikipedia.org/wiki/Kogge%E2%80%93Stone_adder)
