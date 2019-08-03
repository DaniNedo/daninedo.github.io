---
layout: post
title: How to program STM8 microcontrollers
subtitle: ...on Windows and not die trying.
gh-repo: daninedo/stm8
gh-badge: [star, fork, follow]
tags: [STM, SMT8, SMT8S SDCC, Make, C, STVP, Windows]
comments: true
---

I could't find a complete tutorial for programming STM8 on Windows, so I decided
to write my own explaining how to setup a Windows PC to easily do so. However,
even if you are not using Windows, you can still follow the articles to 
learn how to program an STM8, tutorials for Unix systems will be linked.

Before you tell me to go and buy a Mac or a Think Pad with Linux, I want to say
that at the moment of writing this article I only had my trusty Toshiba laptop running
on Windows and did't want to spend money on a new computer or deal with dual booting.
Even if the Internet is pushing you to use Unix for this kind of applications,
I think is fine to get the most of the tools you have in order to learn, letting
performance and other criteria aside.

## Background
If you are reading this I assume that you at least had some expirience blinking
an LED with an Arduino board and now you are looking for more exciting challenges.

Arduino is not bad, indeed it's a great tool to learn and it's the way I started
microcontroller programming, but it "hides" lots of stuff from you. As you learn
more and more about programming, you start to question how is this magic
that links the firmware you write and the hardware works, specially when it comes
to such resource-limited applications like microcontrollers.

You are probably familiar with the terms _compile_ and _upload_ or _flash_ used
in the Arduino IDE. The _compile_ function somehow processes your code an makes
it machine-understandable and as its name suggests _upload_ deploys the processed
code to the target device. Arduino's _IDE_ stands for _Integrated Development Environment_
and contains the tools to write, compile and upload the code.

So basically the minimum necessary tools you for programming a microcontroller are:
- **Text editor**
- **Compiler**
- **Flashing tool**

Explaining how the complier and the flash tool work is out of the scope of the
article and I don't have the necessary knowledge to explain that anyway. I rather
want to focus on how to setup and use these tools.

## Necessary software
Now that we are ready to setup up our programming environmet we have two options:
1. Use and IDE like Arduino does
2. Install all the tools independently

There are many of IDE's for STM out there, some paid, some free...like
[Atollic True Studio](https://atollic.com/truestudio/),
[System Workbench](https://www.st.com/en/development-tools/sw4stm32.html),
[IAR](https://www.iar.com/),
[Keil](http://www.keil.com/)...
I have tried some the first two, and even managed to blink an LED, however I was
feeling quite lost. I dind't really know how to deal with the so called _projects_,
the filing system seemed odd with lots of misterious files...
to many things to digest when you are just starting, but maybe it was only me.

Fortunately, one day I found this
[exquisit tutorial](https://lujji.github.io/blog/bare-metal-programming-stm8/) from Lujji.
These articles inspired on that tutorial and are intended to complement it.
Although the author was using some tools for
Unix it was really easy to comprenhend what tools were needed. The approach of
using independent tools for writing, compiling and flashing allows understand
better what is happening at the different stages of programming the microcontrollers.

So, as listed before you will need the following software:
- A text editor. I'm using [Atom](https://Atom.io), but you are free to choose.
- [Small Device C Compiler](http://sdcc.sourceforge.net/) (SDCC)
and [Make](https://sourceforge.net/projects/gnuwin32/).
- [ST Visual Programmer](https://www.st.com/en/development-tools/stvp-stm32.html),
for Unix you can use [STM8Flash](https://github.com/vdudouyt/stm8flash).

### Installing SDCC
I said that we are going to do all this on Windows, but I lied...SDCC is GNU tool
that with some magic help from [MINGW](http://www.mingw.org/) works on Windows as
well (you don't have to install MINGW directly). SDCC is downloadable 
from [here](https://sourceforge.net/projects/sdcc/files/).
Get the latest version suitable for your machine and follow the instructions of
the installer. After the installation you have to add SDCC to your machine's Path,
You can check if it is correctly installed typing `SDCC --version` in the CMD, and
you will see the version of the software.

### Installing Make
Make is not an indispensable tool, but very useful, we will see it later.
It is also a GNU tool, but can be downloaded from [here](https://sourceforge.net/projects/gnuwin32/).
If you prefer to download the [complete version](https://osdn.net/projects/mingw/releases/)
of MINGW, Make also comes with it. Download the Installation Manager and select
_Basic Setup_, Make will be included in the installation. I tried both options and
both worked for me. Don't forget to add Make to the Path! Again you can check if
all went good typing `Make --version`.

### Installing ST Visual Programmer
This is an official flashing tool from ST and you can get it
[here](https://www.st.com/en/development-tools/stvp-stm32.html). You might be asked
to provide your email when downloading. Eventhough it is called Visual Programmer
and comes with a Graphical User Interface (GUI) it also comes with a Command
Line Interface (CLI), it took me 8 months to realize that and search for the CLI
in the folder, don't repeat my mistakes... Anyway, be sure to add the folder that contains
the CLI to the Path, and verify the installation typing `STVP_CmdLine -version`.
There's also another tool from ST called [ST-LINK utility](https://www.st.com/en/development-tools/stsw-link004.html)
but is is useful only for STM32.

Slap yourself on the back because you successfully installed all the necessary
software and you are ready to do some STM8 programming.

## The hardware
We have plenty of [options](https://www.st.com/en/evaluation-tools/stm8-mcu-eval-boards.html#2)
to choose from for our hardware, from Dev boards to bare MCUs. I went for the so
called STM8 blue:

![STM8s](/img/stm8s.jpg){: .center-block width="50%" :}

This is a cheap dev board built around the STM8S003F3P6 or STM8S103F3P6 and has a few extra components such
as a ASM1117 3.3V regulator, a reset button, a power LED and a LED conected to one
of the GPIO. It also comes with breakout pins for easy breadboard prototyping and
programming.

More information about the board can be found
[here](https://tenbaht.github.io/sduino/hardware/stm8blue/).

To program the micro we need a flashing tool, in this case clone ST-LINK:

![ST-LINK](/img/stlink.jpg){: .center-block width="50%" :}

Additionally you might want to get a breadboard, LEDs, buttons, jumpers... When
you have everything ready you can step into the next tutorial.
