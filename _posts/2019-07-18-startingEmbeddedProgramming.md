---
layout: post
title: Starting with Embedded Programming
subtitle: Look mom, no IDE!
gh-repo: daninedo/stm8-tutorial
gh-badge: [star, fork, follow]
tags: [embedded, programming, gcc, windows, wsl, linux]
comments: true
---

Switching to new different development environment can be intimidating, specially when you don't know what tools you need to install and how to use them. In this article we will discuss in a generic way the minimum requirements to start your embedded journey.

If you are reading this I assume that you have at least some experience blinking
an LED with an Arduino board and now you are looking for more exciting challenges.

Arduino is a great tool to learn and it's the way I started
microcontroller programming, but it "hides" lots of stuff from you. As you learn
more and more about programming, you start to question how this magic
that links the firmware and the hardware works, specially when it comes
to such resource-limited applications like microcontrollers.

You are probably familiar with the terms _compile_ and _upload_ or _flash_ used
in the Arduino IDE. The _compile_ function somehow processes your code and as its name suggests _upload_ deploys the processed code to the target device Arduino's _IDE_ stands for _Integrated Development Environment_
and contains the tools to write, compile and upload the code.

So basically, the minimum necessary tools you need for programming a microcontroller are:
- **Text editor**
- **Compiler**
- **Flashing tool**

At this point we have two options:
1. Use and IDE like Arduino does
2. Install and use all the tools independently

There are many of IDE's out there, some paid, some free... like
[Atollic True Studio](https://atollic.com/truestudio/),
[System Workbench](https://www.st.com/en/development-tools/sw4stm32.html),
[IAR](https://www.iar.com/),
[Keil](http://www.keil.com/), [Code Composer Studio](https://www.ti.com/tool/CCSTUDIO) or [Mbed Studio](https://os.mbed.com/studio/) among others. I don't know if its only me, but every time I was starting to work with an IDE I felt overwhelmed by the vast amount of bloat files sometimes taking gigabytes of space. I refused to believe that all that was needed to program my minuscule microcontroller with only few kilobytes of memory.

Another thing that happens with IDEs is that sometimes the line between what is specific to the IDE and what is needed to program your microcontroller is very blurry. The funny thing is that in the end many IDEs end up using the same tools you would use manually but putting a graphical layer over them.

Imagine you never tighten a screw and someone handles you an electrical screwdriver. You might associate that screws are tighten only by pressing the trigger switch. But what happens if now you need to use a good old manual screwdriver? Maybe the example is a bit silly, and perhaps it is just easier to use the electrical screwdriver and call it a day, but sometimes it is good to step back and learn the basics before advancing and learning more complex stuff.

Overall I have nothing against IDEs and I see the value in them, when you know what you are doing they can be very convenient. However, this blog will focus mainly on standalone tools, explaining them in detail to give a better picture of what we are doing. In any case, you should use the tools that better suit you, but it is also nice to know what happens under the hood, I believe that it will give you more flexibility in the long term.
