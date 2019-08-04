---
layout: post
title: Hello, World! on the STM8
subtitle: The bare metal approach
gh-repo: daninedo/stm8
gh-badge: [star, fork, follow]
tags: [STM, SMT8, SMT8S, SDCC, C, Hello World]
comments: true
---

If you have read the last article you should have everything ready to start programming your
STM8, if not I strongly recommend going through it. This time we will be finally
typing some lines of code and blinking some LEDs!

## The STM8S
I'm going to use the STM8S003F3P6, a value line 8-bit microcontroller with the
following specs:
- 8 KByte of flash memory
- 1 KByte of RAM
- 128 Bytes of EEPROM
- Hardware I2C, UART & SPI
- 10 bit ADC
- 16 GPIOs

{: .box-note}
**Note:** It is not a very recommended microcontroller for development because it is only
guaranteed to work for up to 100 flash cycles. If you need to buy one I suggest you
to get the STM8S103F3P6 instead, it supports up to 10000 flash cycles and you can
find it with the same breakout board.

Together with it we sould get the Datasheet and the Reference Manual. Those are
indispensable files for being able to program the microcontroller and I would recommend
having them on hand during the reading.

## Bare metal programming
In these articles we are not going to use any libraries apart from those that come with
standard C, we will have to directly interact with the hardware of the
microcontroller without using any API or hardware abstraction layers.

This approach is almost at the bottom of the low level human-readable codes, preceded
probably only by code written in [Assembly Language](https://en.wikipedia.org/wiki/Assembly_language).
Then of course there is [Machine Code](https://simple.wikipedia.org/wiki/Machine_code),
that has hexadecimal or binary formats and it is not readble by humans.
Although a bit intimidating for a begginer, it is a good way to undestad what
is really going on.

### Pointers
Ha! Did you think we could get by without these bad boys? Well no. As a quick summary
the memory is organized in blocks, commonly know as [registers](https://en.wikipedia.org/wiki/Hardware_register),
where information can be stored, each of this blocks is identified by an address:

| Address | Value |
| :------ | :---- |
| 0x00 | 0xFF |
| 0x01 | 0x00 |
| 0x02 | 0x0A |

A pointer is a coding element that stores the address of a register in its value field,
essentially it points to its address. Pointers can also be dereferenced to access the values
stored in that register, e.g.
```
uint8_t var = 0xD3; // var stores 0xD3
uint8_t *ptr = &var; // ptr stores var's address
*ptr = 0x0C // var now stores 0x0C
```

### Memory
When programming we don't usually know what memory addresses are used, and we don't really
care as long as everything is inside the proper boundries. However, to interact with the
hardware we actually need to know "some" specific memory addresses to access the so called
Special Function Registers (SFR). This registers are presented in the datasheet and
explained in detail in the reference manual.

In the page 30 of our target microcontroller's datasheet the memory map is shown, and we
can see that the SFR addresses for GPIO and peripherials go from `0x005000` up to `0x0057FF`:

![Memory map](/img/gpiomemorymap.jpg){: .center-block width="50%" :}

In the next pages, we can see a list of the different registers and their addresses
and a summary of their purpose. In the reference manual, a more detailed
explanation can be found about each one.

For different examples the same procedure will be followed to write to code:
1. Identify the needed hardware
2. Look in the datasheet and ref. manual for the proper register addresses
3. Write the firmware

## Blinking an LED
For our first program we are going to flash the built-in LED on the STM8 blue pill.
In the image below the pinout of the breakout board is showed:

![STM8 Blue Pill Pinout](/img/stm8blue.png){: .center-block width="50%" :}

More information about the GPIO can be found in the datasheet (page 25):

![STM8 TSSOP20 Pinout](/img/pinout.JPG){: .center-block width="90%" :}

We need to pay attention to the bottom note: Pins marked with a "T" are
true open drain, and there's no protection diode to VDD nor P-Buffer implemented.
What this means is that the pin state can only be Low (connected to GND) or floating
(high impedance) and we need to be extra carefull with the voltage applied to them.

![STM8 GPIO Implementation](/img/gpioblockdiagram.JPG){: .center-block width="90%" :}

The built-in LED is connected to the fifth pin of the Port B (PB5) on the microcontroller,
following this circuit:

![Built-in LED Circuit](/img/stm8blue-schematic.png){: .center-block width="50%" :}

PB5 happens to be a true open drain pin, but it is fine because the LED is tied to
3.3V so pulling the pin low will turn it on.

### GPIO registers
GPIO have various configuration options that can be set through these registers:
- Data Direction Register (DDR)
- Control Register (CR1 & CR2)
- Output Data Register (ODR)
- Input Data Register (IDR)

We need to setup PB5 as an open drain output. In the reference manual (page 107)
all the posible configurations are presented:

![Port configuration](/img/portconfig.JPG){: .center-block width="90%" :}

We just have to set to 1 the proper DDR bit, because PB5 just supports open drain mode
as an output.

Checking again the datasheet (page 31) and the reference manual (page 111) we can find
the addresses for the necessary registers:

![Port B](/img/portb.JPG){: .center-block width="90%" :}

The addresses we are interested in are `0x005000` for `ODR` and `0x005007` for `DDR`.

### The code
To start with, we need to create a blank main.c file using our prefered text editor.
I suggest keeping the files organized and using on folder per example. The code
for this tutorials is available on [Github](https://github.com/DaniNedo/stm8).

Accessing the registers is done by dereferencing a pointer that points to the desired address:
```
#define PB_ODR *(volatile char*)0x5005
#define PB_DDR *(volatile char*)0x5007
```
The keyword `volatile` indicates the compiler to not optimize that element, this is
necessary because the SFR may change during run time by unknown source
(not known to compiler). Check [this](https://barrgroup.com/Embedded-Systems/How-To/C-Volatile-Keyword)
for more information. Also notice that we are not defining any variable, for instance,
whenever we use `PB_ODR` we are actually copying `*(volatile char*)0x5005`.

Next we create the main function and inside we configure the `PB_DDR` to set PB5 as an output.
```
void main(){
  PB_DDR = (1 << 5); // Or 0b00100000 or 0x20
  ...
}
```
Finally we implement the loop where we toggle the LED pin using bitwise operations:
```
while(1){
  PB_ODR ^= (1 << 5);
  for(int i = 0; i < 30000; i++){;} // Basic delay
}
```
If you are not familiar with bitwise operations take a look
[here](https://stackoverflow.com/questions/47981/how-do-you-set-clear-and-toggle-a-single-bit/47990#47990).

The final code should look something like this:

{% highlight cpp linenos %}
#define PB_ODR *(volatile char*)0x5005
#define PB_DDR *(volatile char*)0x5007

void main(){

  PB_DDR = (1 << 5);

  while(1){
    PB_ODR ^= (1 << 5);
    for(int i = 0; i < 30000; i++){;}
  }

}
{% endhighlight %}

### Compiling the code
To compile the code we are going to use the SDCC from the Comand Line Interface.
Open `cmd.exe` on Windows or `bash` on Unix and navigate to the folder that contains
your `main.c` file using `cd` and `dir` comands.
Tip: [How to navigate in CMD](https://riptutorial.com/cmd/example/8646/navigating-in-cmd).

Now execute the following command to start the compilation:
```
sdcc -mstm8 --out-fmt-ihx --std-sdcc11 main.c
```
Let's analize the different parts:
* **-mstm8** define the microcontroller (stm8)
* **--out-fmt-ihx** define the compiled file format (.ihx)
* **--std-sdcc11** follow the [C11](https://en.wikipedia.org/wiki/C11_(C_standard_revision))
standard but allow some conflicts
* **main.c** the file to be compiled

{: .box-note}
**Note:** More information about the compilation options can be found executing `sddc --help`
or reading the [SDCC user guide](http://sdcc.sourceforge.net/doc/sdccman.pdf)
(starting from page 26).

A few files will be generated after the compiler has done its job. For now we are
mostly interested in the main.ihx file. The extension .ihx stands for Intel HEX and it
contains the compiled firmware that we need to upload to the microcontroller.

### Flashing the firmware
If you are using stm8flash, you can simply call:
```
stm8flash -c stlinkv2 -p stm8s003f3 -w main.ihx
```

On the other hand, if you are using ST Visual Programmer as I do,
it only supports .hex or motorola's .s19 extensions. The .s19 file can be automatically generated
changing the `--out-fmt-ihx` to `--out-fmt-s19`, however I had some issues trying to flash my
device with the .s19 file generated from SDCC. The way I found it works the best is generating
the .ihx file as we did before and then calling:
```
packihx main.ihx > main.hex
```
This will generate the necessary .hex file.

Next we need to physically connect our dev board to the ST-LINK like this:

| ST-LINK | STM8 Board |
| :------ | :---- |
| GND | GND |
| 3.3V | 3.3V |
| SWIM | SWIM |

The final step is to upload our firmware to the microcontroller calling. This can be done in
two ways:

#### Using the STVP GUI
Start the GUI and go to _Configure_ -> _Configure ST Visual Programmer_, then select ST-LINK as
_Hardware_, SWIM as _Port_, and your microcontroller model in _Device_, in my case STM8S003F3.
Next go to _File_ -> _Open..._ and select the .hex file and finally go to _Program_ and press
_Current Tab_. At this point the buil-in LED of your board should start blinking.

#### Using the STVP_CmdLine
Open the CMD and again make sure that you are in the project folder, then execute:
```
STVP_CmdLine -Device=STM8S003F3 -FileProg=main.hex
```

In reality we are executing the following:
```
STVP_CmdLine -BoardName=ST-LINK -Port=USB -ProgMode=SWIM -Device=STM8S003F3
-FileProg=main.hex
```
These flags are the default ones and they have the proper settings for our case.
Probably there are a couple more, but I think those are the most relevant.

{: .box-note}
**Note:** The flag names are self explanatory, but if you want to know more call `STVP_CmdLine -help`

Finally, some information will be printed and you will be asked to hit 'Space' to finish or any other
key to loop. The loop function is nice if you have to program more than one microcontroller.
Additionally a Result.log file will be generated, this can be disabled adding the `no-log` flag.
Now the built-in LED should be blinking as well.

Congratulations you just wrote and flashed your first bare minimum firmware for STM8!
