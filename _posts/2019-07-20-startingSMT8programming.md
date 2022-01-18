---
layout: post
title: Starting with STM8 microcontollers
subtitle: Windows? Linux? MacOS? No problem
gh-repo: daninedo/stm8-tutorial
gh-badge: [star, fork, follow]
tags: [STM, SMT8, SMT8S SDCC, C, STVP, Windows, Linux, MacOS]
comments: true
---
When learning how to work with a new target the first step is always the hardest: setting up the development environment. Many times this is overlooked, but I would like to go step by step and show what tools are required and how to install them in your preferred OS.

You will often find that tutorials rely on you using some sort of Linux flavour or MacOS. I see the point, sometimes it is just... easier. But I think is fine to get the most of the tools you have in order to learn, letting performance and other criteria aside.

When I was starting with STM8 microntrollers myself I stumbled upon this [exquisite tutorial](https://lujji.github.io/blog/bare-metal-programming-stm8/) from Lujji. These articles inspired on that tutorial and are intended to complement it. Although the author was using some tools for Unix it was really easy to comprehend what tools were needed. The approach of using independent tools for writing, compiling and flashing allows understand better what is happening at the different stages of programming the microcontrollers.

So, as listed before you will need the following software:
- A text editor. I'm using [VS Code](https://code.visualstudio.com/), but you are free to choose.
- The [Small Device C Compiler](http://sdcc.sourceforge.net/) toolchain (SDCC).
- A flashing tool. [ST Visual Programmer](https://www.st.com/en/development-tools/stvp-stm32.html) for Windows, or [STM8Flash](https://github.com/vdudouyt/stm8flash) for Unix.

### Installing SDCC
SDCC is a free open source compiler suite for many embedded targets, including our beloved STM8. The homepage can be found [here](http://sdcc.sourceforge.net/).

<ul id="sdccTabs" class="nav nav-tabs">
    <li class="active"><a href="#sdcc_windows" data-toggle="tab">Windows</a></li>
    <li><a href="#sdcc_linuxwsl" data-toggle="tab">Linux/WSL</a></li>
    <li><a href="#sdcc_macos" data-toggle="tab">MacOS</a></li>
</ul>

<div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="sdcc_windows">
To install SDCC on Windows go to the <a href="http://sdcc.sourceforge.net/snap.php#Windows">downloads page</a> and get the latest installer. Execute it and follow the instalation steps. Once the tool is installed add it to Windows PATH as shown in <a href="https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/">this tutorial</a>. The default binary directory is: <code>C:\Program Files\SDCC\bin</code>.
</div>

<div role="tabpanel" class="tab-pane" id="sdcc_linuxwsl">
To install SDCC on Linux or Windows Subsystem for Linux (WSL) open a terminal and run:
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>sudo apt install sdcc</code></pre></div></div>
</div>

<div role="tabpanel" class="tab-pane" id="sdcc_macos">
To install SDCC on MacOS open a terminal and run:
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>brew install sdcc</code></pre></div></div>
</div>
</div>



### Installing ST Visual Programmer
This is an official flashing tool from ST and you can get it
[here](https://www.st.com/en/development-tools/stvp-stm32.html). You might be asked
to provide your email when downloading. It comes with a Graphical User Interface (GUI) and with a Command
Line Interface (CLI), it took me 8 months to realize that and search for the CLI, don't repeat my mistakes... Anyway, be sure to add the folder that contains the CLI to the Path, and verify the installation typing `STVP_CmdLine -version`.
There's also another tool from ST called [ST-LINK utility](https://www.st.com/en/development-tools/stsw-link004.html)
but is is useful only for STM32. If you are using Unix check the [stm8sflash](https://github.com/vdudouyt/stm8flash) tool.

Slap yourself on the back because you successfully installed all the necessary
software and you are ready to do some STM8 programming.

## The hardware
We have plenty of [options](https://www.st.com/en/evaluation-tools/stm8-mcu-eval-boards.html#2)
to choose from for our hardware, from Dev boards to bare MCUs. I went for the so
called STM8 blue:

![STM8s](/img/stm8s.jpg){: .center-block width="50%" :}

This is a cheap dev board built around the STM8S003F3P6 or STM8S103F3P6 and has a few extra components such
as a ASM1117 3.3V regulator, a reset button, a power LED and a LED connected to one
of the GPIO. It also comes with breakout pins for easy breadboard prototyping and
programming.

More information about the board can be found
[here](https://tenbaht.github.io/sduino/hardware/stm8blue/).

To program the micro we need a flashing tool, in this case clone ST-LINK:

![ST-LINK](/img/stlink.jpg){: .center-block width="50%" :}

Additionally you might want to get a breadboard, LEDs, buttons, jumpers... When
you have everything ready you can step into the next tutorial.
