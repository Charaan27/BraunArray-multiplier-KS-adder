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

# Introduction
The need for efficient binary multipliers that could be incorporated in Digital ICs has seen an unprecendented rise over the last few years. The conventional architecture of a binary multiplier involves a series of AND gates that will generate what is called as a _partial product_, and later these are added up with each other to produce the product. This corresponds to a variety of binary multipliers known as **parallel array multipliers**. However, this is an extensively computational process, when compared with other binary operations. But, binary multiplication is a very important arithmetic operation, which is primarily being used in Digital Signal Processing applications. For example, the _Multiply and Accumulate (MAC)_ operation is used in implementing Finite Impulse Response (FIR) Filters, and other DSP algorithms. Hence it becomes important to find out ways to optimize the architecure of binary multipliers thus increasing its efficiency, while decreasing its area occupancy, computation time and power dissipation. One of the efficient parallel array multipliers that is widely used for this purpose is the **Braun Array Multiplier**. It is a type of a high-speed parallel array multiplier that comes with a relatively lesser area and delay, thus making it one of the ideal array multipliers to use. Braun Array Multipliers require a **carry look-ahead adder** in the design. In order to improve the speed and efficiency of the multiplier, a **Kogge-Stone Adder** has been implemented in the place of the carry look-ahead adder. It is a form of a parallel-prefix adder that comes with a lower fan-out at each of its stages, thus increasing the performance for typical CMOS processes.

# Reference Circuit
An n-bit Braun Array Multiplier is realised using (n−1)<sup>2</sup> full adders, n<sup>2</sup> AND gates and a (n−1) bit carry look-ahead adder. Hence for a 4-bit Braun Array Multiplier, we require **9 Full Adders, 16 Logic AND gates and a 3-bit Kogge-Stone Adder**. The reference circuit of the same is shown below.
<p align="center">
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/references/reference_circuit.png">
</p>

# Reference Waveform
For a n\*m multiplication, the number of bits in the product cannot be less than max(m,n) and cannot be more than (m+n), unless one of the two numbers is zero. If the two operands are of similar size (i.e.) n\*n, then the product is of size 2n. Hence for a 4-bit multiplication, the operands are of size 4-bits, while the product is of size 8-bits. In total we will have 8 waveforms for the input (4 + 4 inputs) and 8 waveforms for the output, leading to a total of 16 waveforms. The reference waveform for 4-bit multiplication could be found below.  

<p align="center">
  <br>
  <img src="https://github.com/Charaan27/BraunArray-multiplier-KS-adder/blob/main/project/references/reference_waveform.png">
</p>

# Tools used

- OS: [CentOS Linux 7.9.2009](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7.2009#Packages_released_as_7.8.2003_updates_with_older_packages_on_the_7.9.2009_install_media)
- Design Platform: [Synopsys Custom Design](https://www.synopsys.com/implementation-and-signoff/custom-design-platform.html) - comes with [Synopsys Custom Compiler](https://www.synopsys.com/implementation-and-signoff/custom-design-platform/custom-compiler.html)
- Simulation Software: [PrimeSim HSPICE](https://www.synopsys.com/implementation-and-signoff/ams-simulation/primesim-hspice.html)
- CMOS Process used for device modeling: SAED 32/28nm PDK

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
The illustration of a 3-bit Kogge-Stone Adder is given below:  

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

