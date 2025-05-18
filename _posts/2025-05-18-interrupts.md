---
title: External Interrupts
---


<a href="https://www.youtube.com/watch?v=eLrsCw1UDJU&list=PLVfOnriB1RjWvYaTSpsqs9Us0NV1-ares&index=50">This </a> was another really exciting video.

Lars showed us how to configure buttons as external interrupts, which helps us immediately respond to an input. I know that this is going to be extremely important,
in situations such as emergency crashes in cars, preventing accidents in oncoming objects, preventing fires etc.

It was actually pretty easy! 

I just needed to consult my reference manual to find the pin that maps to the User button on my Nucleo board, then configure it in the same way as Lars did.

According to the manual:

    B1 USER: the user button is connected to the I/O PC13 by default (Tamper support, SB173
    ON and SB180 OFF) or PA0 (Wakeup support, SB180 ON and SB173 OFF) of the STM32
    microcontroller.

So I just configured PC13 as an external interrupt, and enabled the EXTI line.

[![Interrupt Pin](/assets/posts/2025-05-18-interrupts/interrupt_pin.png){: width="350" }](/assets/posts/2025-05-18-interrupts/interrupt_pin.png)

This is especially interesting to me because we don't use interrupts much in web development. I guess it's comparable to an OnClick() event.

I also spent a lot of time trying to mess with the printf -> UART redirect function. I wanted to see if I could run printf() without having to append \r\n or even \n by itself,
but it didn't seem to be easy. I'll keep the standard function for now.