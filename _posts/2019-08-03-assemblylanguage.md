---
layout: post
title: Introduction to assembly language
subtitle: Speaking to the microcontroller!
gh-repo: daninedo/stm8
gh-badge: [star, fork, follow]
tags: [STM, SMT8, Assembly, C]
comments: true
---

Last time our compiler generated a few files but we only used the main.ihx one
to flash the microcontroller. In this brief entry we are going to take a look at
some other files to understand what is happening under the hood.

## Running the code
To begin with let's quickly explain how the code is ran in the microcontroller.

### Arquitecture
The very basic building blocks of a microcontroller or even a computer are the
Central Processing Unit (CPU) and the Memory. The memory stores the data and the
CPU processes it.

The CPU itself is composed of the Arithmetic Logic Unit (ALU), that performs arithmetic
and logic operations, processor registers that supply operands to the ALU and store
the results of its operations and the Control Unit (CU) that manages all the processes.

Finally, the different components are linked together by one or more Data Buses.

### Computer Instructions
In order to make something to happen, hopefully what is writen in our code, we
need to tell the CPU where to get the data and how to process it. This is done using
Instructions that are a representation of a real physical process, you can think of
them as the last frontier between firmware and hardware. They are an array of bits
composed of an identifier and some arguments that can be data or memory locations.

Instructions can be strored in the same memory as the data and
the program (Von-Neumann Arquitecture), or in their independent memory (Harvard Arquitecture).
Our STM8 uses a modified version of Harvard Arquitecture with a Reduced Instruction Set (RISC).

The instructions

### Program execution
When an instruction is called by the CU, the different data paths that connect the
buses, ALU and memory registers are rearranged to perform the desired action, this is better explained with an example. Consider
this oversimplified model of a microcontroller/computer programmed to add the values
of two variables (x and y) and store the result in a third one (z):

![machine1](/img/computer_diagram.png)

What we can see in the picture is the following:
* A memory that contains the instructions in the proper order
* A memory that stores the variables and their values
* A Program Counter (PC) that iterates over the instructions
* A Control Unit with an Instruction Decoder (The Instruction Set)
* The ALU with two inputs (A and B), one output (C) and an enable switch (EN)
* A Process Register bank
* The Data Bus that transmits the information
* Two "gates", Data Select (DS) and Register Select (RS)

Next we go though the different steps of the program execution.

#### 1.Loading the value of X
