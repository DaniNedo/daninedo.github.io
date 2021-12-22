---
layout: post
title: Starting with Embedded Programming
subtitle: Say hello to GCC and friends
gh-repo: daninedo/stm8-tutorial
gh-badge: [star, fork, follow]
tags: [embedded, programming, gcc, windows, wsl, linux]
comments: true
---

Have you been playing with Arduino and want to step up the game? Switching to a different development environment can be intimidating, specially when you don't know what tools you need to install and how to use them. In this article we will discuss in a generic way the minimum requirements to start your embedded journey.

## Background
If you are reading this I assume that you have at least some experience blinking
an LED with an Arduino board and now you are looking for more exciting challenges.

Arduino is a great tool to learn and it's the way I started
microcontroller programming, but it "hides" lots of stuff from you. As you learn
more and more about programming, you start to question how this magic
that links the firmware and the hardware works, specially when it comes
to such resource-limited applications like microcontrollers.

You are probably familiar with the terms _compile_ and _upload_ or _flash_ used
in the Arduino IDE. The _compile_ function somehow processes your code making
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
Now that we are ready to setup up our programming environment we have two options:
1. Use and IDE like Arduino does
2. Install all the tools independently

There are many of IDE's for STM out there, some paid, some free...like
[Atollic True Studio](https://atollic.com/truestudio/),
[System Workbench](https://www.st.com/en/development-tools/sw4stm32.html),
[IAR](https://www.iar.com/),
[Keil](http://www.keil.com/)...
I have tried some the first two, and even managed to blink an LED, however I was
feeling quite lost. I didn't really know how to deal with the so called _projects_,
the filing system seemed odd with lots of mysterious files...
to many things to digest when you are just starting, but maybe it was only me.