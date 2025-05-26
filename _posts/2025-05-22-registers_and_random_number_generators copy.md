---
title: Registers & Random Number Generators
---


<a href="https://www.youtube.com/watch?v=EFjRXGrDehM&list=PLVfOnriB1RjWT_fBzzqsrNaZRPnDgboNI&index=12">This video </a> was quite good (as usual!).

Just last night, I was reading about using bitwise operations and masks to toggle bits in registers in the book 'Making Embedded Systems' by Elecia White.
I was sort of worried, because I have been using HAL since I started with the Nucleo board, and I didn't really know how to interface with peripherals without it.

Luckily, Lars spent this video walking us through the setup of the hardware random number generator (RNG) without HAL. He did a lot of prework, so he was copy/pasting
a lot of his code, but I was able to pause the video and follow along pretty well. 

Using the OR operator to turn on the bit here:

    *trng_cr |= TRNG_CR_RNGEN;

was definitely the highlight of the video to me. I also needed to turn on the RNG peripheral using CubeMX, but it was fairly straightforward otherwise.

[![RNG output](/assets/posts/2025-05-22-registers_and_random_number_generators/rng_output.PNG){: width="300" }](/assets/posts/2025-05-22-registers_and_random_number_generators/rng_output.PNG)

I'm not sure if other vendors implement HAL as well as STM does, so I'm glad I was able to learn this stuff. However, I can definitely see why people say to use HAL
when you have it available :)

It really lets you focus on the unique functionality you want to implement on the board, rather than understanding the board inside and out.