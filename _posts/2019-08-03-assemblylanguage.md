---
layout: post
title: Introduction to machine language
subtitle: Speaking to the microcontroller!
gh-repo: daninedo/stm8
gh-badge: [star, fork, follow]
tags: [STM, SMT8, Assembly, C]
comments: true
---

Last time our compiler generated a few files but we only used the _main.ihx_ one
to flash the microcontroller. In this brief entry we are going to take a look at
some other files to understand what is happening under the hood.

To begin with let's quickly explain how the code is ran in the microcontroller.

## Arquitecture
The very basic building blocks of a microcontroller or even a computer are the
Central Processing Unit (CPU) and the Memory. The memory stores the data and the
CPU processes it.

The CPU itself is composed of the Arithmetic Logic Unit (ALU), that performs arithmetic
and logic operations, processor registers that supply operands to the ALU and store
the results of its operations and the Control Unit (CU) that manages all the processes.

The different components are linked together by one or more Data Buses.

## Computer Instructions
In order to make something to happen, hopefully what is writen in our code, we
need to tell the CPU where to get the data and how to process it. This is done using
Instructions, which are a representation of a real physical process, you can think of
them as the last frontier between firmware and hardware. They are an array of bits
composed of an identifier and some arguments that can be data or memory locations.

Instructions can be strored in the same memory as the data and
the program (Von-Neumann Arquitecture), or in their independent memory (Harvard Arquitecture).
Our STM8 uses a modified version of Harvard Arquitecture with a Reduced Instruction Set (RISC).

## Program execution
When an instruction is called by the CU, the different data paths that connect the
buses, ALU and memory registers are rearranged to perform the desired action, this is better explained with an example. Consider
this oversimplified model of a microcontroller programmed to add the values
of two variables (x and y) and store the result in a third one (z):

![basic computer](/img/computer_diagram.png)

What we can see in the picture is the following:
* A memory that contains the instructions in the proper order
* A memory that stores the variables and their values
* A Program Counter (PC) that iterates over the instructions
* A Control Unit with an Instruction Decoder (The Instruction Set)
* The ALU with two inputs (A and B), one output (C) and an enable switch (EN)
* A Process Register bank
* The Parallel Data Bus that transmits the information
* Two "gates", Data Select (DS) and Register Select (RS)

Next we go though the different steps of the program execution.

### Step 1: Loading the value of "x"
![load x value](/img/computer_diag_loading_x.png)
The program counter is at 0x00 and the instruction 0x00 (Load x) is passed to the
CU and decoded. After that the DS and RS gates are set according to the arguments
of the instruction and the value of 0x3A (x) is loaded into the register 0x10.

In our model this operation takes just 1 clock cycle (in a real case this can be different)
and it happens almost inmediately if we ignore the propagation delay of the hardware.
Also we are making the assumption that the parallel bus is as wide as the registers so
that the data can be copied in one go.

### Step 2: Loading the value of "y"
![load y value](/img/computer_diag_loading_y.png)
The program counter increases by one and the next instruction (0x01) is loaded
and the previous process is repeated.

### Step 3: Adding the values
![add values](/img/computer_diag_adding.png)
Next the Add instruction is executed. This time the data bus is not used, instead
the the proccess registers 0x10 and 0x11 are passed to the ALU and the
result is stored into the 0x12 register. In our case, the ALU can only perform addition,
so we only have to set the enable pin (EN) to perform the operation.

### Step 4: Storing the result
![store result](/img/computer_diag_storing.png)
Finally the last instuction is executed and the result value stored in the process
register is copied to the memory.

As you can see with this simple example the proccess is quite straigh forward. Of
course you have to take into account that a real microcontroller or computer is
much more complex, and includes many more instructions to perform arithmetics,
logic and conditional jumps... but this can serve as a good base to understand how
the things work.

## The machine code
When we executed the compiler last time many files were generated including the
_main.asm_ file. The .asm extension stands for [Assembly Language](https://en.wikipedia.org/wiki/Assembly_language)
and it is a lenguage that explicitly shows the instructions that compose a program
in a format that a human can read.

In the _main.asm_ file our blinky code starts at the line 90. The lines prior to
it are initializations for the microcontroller.

```
;main.c: 6: PB_DDR = (1 << 5);
	mov	0x5007+0, #0x20
;	main.c: 8: while(1){
00103$:
;main.c: 9: PB_ODR ^= (1 << 5);
	bcpl	20485, #5
;main.c: 10: for(int i = 0; i < 30000; i++){;}
	clrw	x
00106$:
	cpw	x, #0x7530
	jrsge	00103$
	incw	x
	jra	00106$
;main.c: 13: }
	ret
```

In this bit of code we can differentiate a few elements:
* Instructions (e.g. `clrw x`)
* Tags (e.g. `_main:` or `00106$:`)
* HEX representation (e.g. `0082C 5F`)
* Comments (e.g. `;main-c: 8: while(1){`)

### Reading the assembly code
The comments give us a general idea of that is happening next, but we are going
analize the code in detail to understand it better. Keep in mind that as many other
languages, this is executed from top to bottom line by line, furthermore I suggest
you keeping the blinky _main.c_ code on hand get a copy of the the STM8
[Programming Manual](https://www.st.com/content/ccc/resource/technical/document/programming_manual/43/24/13/9a/89/df/45/ed/CD00161709.pdf/files/CD00161709.pdf/jcr:content/translations/en.CD00161709.pdf)
where you can find what the different instructions do. I guarantee the previous
code would be much clearer if you understand the meaning of the name acronyms.

Nevertheless, let's begin:
```
_main:
```
This is an identifier tag for the main function. It is called at the after the initialization of the microcontroller.
```
;main.c: 6: PB_DDR = (1 << 5);
	mov	0x5007+0, #0x20
```
The `mov` (move) instruction takes two arguments, a memory address and a value. The
memory address is `0x5007+0` (PB_DDR) and the value `(1 << 5)` is directly converted to `0x20`,
with the `#` meaning that is a numerical value. As you remember, we were not creating
any variable two first lines of the main.c files, and that is reflected in the assembly
code.
```
;main.c: 8: while(1){
00103$:
```
Another tag to indicate where the _while_ loop starts.
```
;main.c: 9: PB_ODR ^= (1 << 5); // Toggle PB5
	bcpl	20485, #5
```
`bcpl` (Bit Complement) flips the nth bit of a memory address provided. In
this case the 5th bit of the memory address `20485` is fliped. Again, the address
is not arbitrary, converted to decimal it is 0x5005 which is the address of the
PB_ODR.

Next the _for_ loop structure starts and it's composed of few lines.
```
;main.c: 10: for(int i = 0; i < 30000; i++){;}
	clrw	x
```
`clrw` (Clear word) clears the content of the proccess register x. More info about
the process registers can be found in the section **3.2 CPU registers** of the
programming manual.
```
00106$:
	cpw	x, #0x7530
	jrsge	00103$
	incw	x
	jra	00106$
```
A tag is created and the `cpw`(Compare word) instruction compares the content of the
`x` register with `0x7530` (30000 in decimal) subtracting the two values. The `N`
(Negative) flag is set if the result is negative and the `V` (Overflow) flag is
set if the subtraction causes an overflow. Other flags might be set, you can check them
on the page 113 of the P.M. Next the `jrsge` (Jump Relative if Signed Greater or Equal)
checks if the logic [XOR](https://en.wikipedia.org/wiki/Exclusive_or)
operation between `N` and `V` and jumps to the `00103$` tag if the result is 0.

In other words, if the result of `cpw` is negative or is causes an overflow but
not both at the same time the _for_ loop finishes and it returns to the _while_ loop.

If we haven't jumped to `00103$` the `incw` (Increase word) instruction is called and
the content of `x` is increased by one and the `jra` (Jump Relative Always) causes
the program to jump to the `00106$` tag.

In the end of the program we see a `ret` (Return from subrutine), which returns from
the main to the caller. However this instruction won't be ever executed because
we are in an infinite _while_ loop.
