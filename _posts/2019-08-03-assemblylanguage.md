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
To begin with let's quickly go through how the code is ran in the microcontroller.

### Arquitecture
The very basic building blocks of a microcontroller or even a computer are the
Central Processing Unit (CPU) and the Memory. The memory stores the data and the
CPU processes it.

The CPU itself is composed of the Arithmetic Logic Unit (ALU), that performs arithmetic
and logic operations, processor registers that supply operands to the ALU and store
the results of its operations and the Control Unit (CU) that manages all the processes.

Finally, the different components are linked together by one or more Data Buses.

### Instruction set
In order to make something to happen, hopefully what is writen in our code, we
need to tell the CPU where to get the data and how to process it. This is done using
the so called Instructions, that are integrated as a Set. They can be strored in the same memory as the data and
the program (Von-Neumann Arquitecture), or in their independent memory (Harvard Arquitecture).
Our STM8 uses a modified version of Harvard Arquitecture with a Reduced Instruction Set (RISC).

The instructions are a representation of a real physical process, you can think of
them as the last frontier between firmware and hardware. They are an array of bits
composed of an identifier and some arguments that can be data or memory locations.

When an instruction is called by the CU, the different data paths that connect the
buses, ALU and memory registers are rearranged to perform the desired action. Consider
this oversimplified example of a machine with the following components:
![machine1](/img/simplemachine1.jpg)
Imagine that we want to add the values of two variables (a and b) and store the result in a
third one (c), what in C would be represented as `c = a + b`.
