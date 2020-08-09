---
layout: post
title: Making a better GPIO interface 
subtitle: aka a Hardware Abstraction Layer
gh-repo: daninedo/stm8-tutorial
gh-badge: [star, fork, follow]
tags: [STM, SMT8, C, SDCC, GPIO, HAL]
comments: true
---

Having the ability to access and control the registers responsible for the GPIOs of our microcontroller is great, but let's be honest, this can be rather tedious. This time we will be creating a simple interface to interact with the hardware.

## Hardware Abstraction Layers
The interface we are about to create is commonly know as a Hardware Abstraction Layer (HAL), and it is very useful because it allows to detach the firmware (at least the main application) from the hardware. This comes with the benefit of having a more portable code, that can run on multiple targets. 

Another benefit is that the HALs for the MCUs are usually supplied by the MCU manufacturer or by IDE/compiler provider. For example, ST offers HAL Libraries for the STM32 and the STM8s MCUs for free. But why would we create our own HAL if we already have some available? Well, first of all the official release of the [HAL libraries](https://www.st.com/en/embedded-software/stsw-stm8069.html) are not compatible with SDCC, although the Github user Gicking already made a [patch](https://github.com/gicking/STM8-SPL_SDCC_patch) for it. Secondly, right know we want to learn, and there's no better way than to do the things ourselves.

Furthermore, using of the shelf HALs might have some disadvantages. One of the most remarkable is that sometimes the HAL is a bottleneck for the hardware. Some days ago I was working on an Arduino [project](https://github.com/DaniNedo/Arduino-Extended-UART) and needed to use some special functions of the UART peripheral. The Arduino environment, on its pursue of simplicity for beginners didn't include any method of using this special features so I had to edit the core libraries to make it work. It was not a difficult fix, specially because some people came up with the solution before me, but imagine if the libraries were already pre-compiled and the source was not available... Of course Arduino is an extreme case, but you get the point.
