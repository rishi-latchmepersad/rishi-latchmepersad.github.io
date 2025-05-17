---
title: Oscillators
---

This will be a fairly short post. I decided to continue with the video series by <a href="https://www.youtube.com/@stm32world">STM32World</a>, because I really think I
can learn a lot from Lars.

I went through the STM32 Tutorial videos #3 and #4, with a link to one of them
<a href="https://www.youtube.com/watch?v=rQDa_vxYM2Q&list=PLVfOnriB1RjWvYaTSpsqs9Us0NV1-ares&index=52&pp=iAQB0gcJCY0JAYcqIYzv">here</a>

He went over another blink LED tutorial. I was able to follow along pretty easily because I had done it before, however I ran into some interesting issues.

Firstly, when I enabled the USART3 interface, I saw that the STM32 CubeMX interface was configuring the wrong pins. I tested it, and as I expected I wasn't getting any output
on PuTTY. I disabled the pins, and re-enabled the correct pins according to the manual for my board. This tells me that STM32 CubeMX is not always correct by default,
and you need to have your manual handy at all times.

Secondly, once the serial output actually started coming through, I started seeing rubbish output like this:

    ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒

I thought that this was an incorrect baud rate issue, but even after I triple checked for matching baud rates, I couldn't get good outputs.

It turned out that my Nucleo board doesn't have an external oscillator. Instead, it uses an internal 16MHz oscillator.

![![STM32CubeMX HSI](/assets/posts/2025-05-17-oscillators/hsi.PNG){: width="350" }](/assets/posts/2025-05-15-oscillators/hsi.PNG)

Lars explained that he always uses an external oscillator for higher speeds and precision, but it seems that I'll have to leave mine as internal for now.