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

### Installing SDCC
SDCC is a free open source compiler suite for many embedded targets, including our beloved STM8. The homepage can be found [here](http://sdcc.sourceforge.net/).

<ul id="sdccTabs" class="nav nav-tabs">
    <li class="active"><a href="#sdcc_windows" data-toggle="tab">Windows</a></li>
    <li><a href="#sdcc_wsl" data-toggle="tab">WSL</a></li>
    <li><a href="#sdcc_linux" data-toggle="tab">Linux</a></li>
    <li><a href="#sdcc_macos" data-toggle="tab">MacOS</a></li>
</ul>

<div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="sdcc_windows">
<p>Go to the <a href="http://sdcc.sourceforge.net/snap.php#Windows">downloads page</a> and get the latest installer. Execute it and follow the instalation steps. Once the tool is installed add it to Windows PATH. If you don't know how to do it follow <a href="https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/">this tutorial</a>. The default binary directory is: <code>C:\Program Files\SDCC\bin</code>.</p>
</div>

<div role="tabpanel" class="tab-pane" id="sdcc_wsl">
<p>Open a terminal and run:</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>sudo apt install sdcc</code></pre></div></div>
</div>

<div role="tabpanel" class="tab-pane" id="sdcc_linux">
<p>Open a terminal and run:</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>sudo apt install sdcc</code></pre></div></div>
</div>

<div role="tabpanel" class="tab-pane" id="sdcc_macos">
<p>Open a terminal and run:</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>brew install sdcc</code></pre></div></div>
</div>
</div>

If everything went well, you should be able to check the SDCC version typing:
```
sdcc --version
```

### Installing a flashing tool

The flash tool is necessary to load the compiled binaries to the desired target.

<ul id="sdccTabs" class="nav nav-tabs">
    <li class="active"><a href="#flash_windows" data-toggle="tab">Windows</a></li>
    <li><a href="#flash_wsl" data-toggle="tab">WSL</a></li>
    <li><a href="#flash_linux" data-toggle="tab">Linux</a></li>
    <li><a href="#flash_macos" data-toggle="tab">MacOS</a></li>
</ul>

<div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="flash_windows">
<p>ST Visual Programmer is the official ST tool for flashing devices and  comes with a Graphical User Interface (GUI) as well as with a Command Line. It can be downloaded from <a href="https://www.st.com/en/development-tools/stvp-stm32.html">here</a>, you might be asked to provide your email when downloading. Be sure to add the folder that contains the CLI to the PATH, and verify the installation typing:</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>STVP_CmdLine -version</code></pre></div></div>
</div>

<div role="tabpanel" class="tab-pane" id="flash_wsl">
<p>There is no support for USB devices on WSL (apart from USB drives and USB to Serial Converters in WSL1). Fortunately, Windows commands can be invoked from WSL as well. ST Visual Programmer is the official ST tool for flashing devices and  comes with a Graphical User Interface (GUI) as well as with a Command Line. It can be downloaded from <a href="https://www.st.com/en/development-tools/stvp-stm32.html">here</a>, you might be asked to provide your email when downloading. Be sure to add the folder that contains the CLI to the PATH, and verify the installation typing (notice the ".exe"):</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>STVP_CmdLine.exe -version</code></pre></div></div>
</div>

<div role="tabpanel" class="tab-pane" id="flash_linux">
<p>stm8flash</p>
</div>

<div role="tabpanel" class="tab-pane" id="flash_macos">
<p>stm8flash</p>
</div>
</div>


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
