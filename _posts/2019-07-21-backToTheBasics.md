---
layout: post
title: Hello, World! on the STM8
subtitle: Back to the basics!
gh-repo: daninedo/stm8
gh-badge: [star, fork, follow]
tags: [STM, SMT8, SMT8S003F3, SDCC, C]
comments: true
---

If you have read the last article you should have everything ready to start programming your
STM8, if not I strongly recommend going through it. This time we will be finally
typing some lines of code and blinking some LEDs!

## The STM8S003F3P6
We are going to use the STM8S003F3P6, a value line 8-bit microcontroller with the
following specs:
- 8 KB of flash memory
- 1 KB of RAM
- 128 KB of EEPROM
- Hardware I2C, UART & SPI
- 10 bit ADC
- 16 GPIOs

Together with it we sould get the Datasheet and the Reference Manual. Those are
indispensable files for being able to program the microcontroller and I would recommend
having them on hand during the reading.

## Pointers
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

## Memory
When programming we don't usually know what memory addresses are used, and we don't really
care as long as everything is inside the proper boundries. However, to interact with the
hardware we actually need to know "some" specific memory addresses to access the so called
Special Function Registers (SFR). This registers are presented in the datasheet and
explained in detail in the reference manual.

In the page 30 of our target microcontroller's datasheet the memory map is shown, and we
can see that the SFR addresses for GPIO and peripherials go from 0x005000 up to 0x0057FF:

![Memory map](/img/gpiomemorymap.jpg){: .center-block width="50%" :}
