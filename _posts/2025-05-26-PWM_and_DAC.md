---
title: PWM (Pulse Width Modulation) and DAC (Digital to Analog Conversion)
---


<a href="https://www.youtube.com/watch?v=0N4ECamZw2k&list=PLVfOnriB1RjWvYaTSpsqs9Us0NV1-ares&index=42">Videos 14 and 15</a> covered sinewave generation and DAC.

We mainly used the sinewaves to create smooth transitions in our LED outputs, but I could definitely see the application in other analog signals such as audio signals.

The first video and the first half of the second video were fairly straightforward. I couldn't record any of my own output because I don't have an oscillator yet, but
I was able to debug the code and it seemed to run pretty well.

My problems began when we tried to utilize the CMSIS-DSP library, to optimize the code calculating the cos/sin functions (using the ARM libraries), to use the function:

    arm_cos_f32()

Lars didn't go through the full details of how he got the library set up, so I had to resort to other tutorials.

I eventually got it working using this video: <a href="https://www.youtube.com/watch?v=GH50tEgBpX8">by a channel called Easier in Practice</a>

I'll highlight the important bits here for anyone else using a similar board to me (an STM32 Nucleo F767ZI).

Firstly, the import statement is:

    #include "arm_math.h"

Don't make a mistake like me and use the angle brackets instead of the quotes. Remember the angle brackets are for built-in libraries and the quotes are for user(local) libraries.

These were the files that I ended up importing into my project:

[![File imports](/assets/posts/2025-05-26-PWM_and_DAC/copied_dsp_files.PNG){: width="600" }](/assets/posts/2025-05-26-PWM_and_DAC/copied_dsp_files.PNG)

And these were the strings that I added for my compiler and linker:

[![Compiler](/assets/posts/2025-05-26-PWM_and_DAC/compiler_include_paths.PNG){: width="600" }](/assets/posts/2025-05-26-PWM_and_DAC/compiler_include_paths.PNG)

[![Linker](/assets/posts/2025-05-26-PWM_and_DAC/linker_include_libraries.PNG){: width="600" }](/assets/posts/2025-05-26-PWM_and_DAC/linker_include_libraries.PNG)

I can't tell you how happy I was when I finally got this to work. I'm sure this will be an important library when I have to do more complex signal processing in the future,
so I really invested a ton of hours into it.

At the end, I got my signal to run at around 15 kHZ. By the time I'm ready to actually use this, I should have an external oscillator and an oscilloscope to test, so I'll
probably be able to run at a higher frequency.

[![Serial Output](/assets/posts/2025-05-26-PWM_and_DAC/serial_output.PNG){: width="600" }](/assets/posts/2025-05-26-PWM_and_DAC/serial_output.PNG)

