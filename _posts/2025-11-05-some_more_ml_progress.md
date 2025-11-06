---
title: Some More ML Progress!
---

# Intro
Okay I'm trying to build the habit of posting more frequently, so here's another quick update on the ML front!

I've sort of figured out how the whole TinyML process works now in STM32. We basically build our model in Google Colab,
export it as a TFLite file, then import it into STM32 Cube AI to generate the C code that we can run on the microcontroller.

Cube AI is a package inside of STM32 CubeIDE (CubeMX), which I was able to install fairly easily inside the IDE. It took me a while,
but I was able to import the TFLite file, and generate the C code by just saving the CubeMX (.ioc) window once I added the file and validated it.

I learned that the 'Device Application' box is likely just a demo project, and doesn't need to be selected. With that selected,
I got a bunch of errors which looked like old version issues. Once I deselected it, I was able to generate the code without any issues!

After I saved it, I saw a lot of new C files appear in the project tree under X-CUBE-AI/App. 

[![X-CUBE-AI](/assets/posts/2025-11-05-some_more_ml_progress/x-cube-ai.JPG){: width="600" }](/assets/posts/2025-11-05-some_more_ml_progress/x-cube-ai.JPG)

# Running the Model

Next, I needed to understand how to actually wire these C files into my main application code. Apparently, Cube AI makes it really easy,
and we just need to set up a FreeRTOS task to call the initialization functions and reference the arrays etc. that were generated.

This task will poll the sensors and wait until we have a full hour of data (for the sliding window). Once we have that, it will
calculate the engineered features (sin(hour of day), delta temp, delta pressure, etc.), then build the sliding window and call the 
inference function to get the temperature prediction.

Another task will periodically store the latest predicted temperature to the sd card. Seems simple enough! Will report back on how
it runs.

# Other News

In other news, this week has been pretty exciting! I had a hand in recording a video for World Mental Health Day at Shell. Hopefully
I get featured in the final cut. I also attended a few University Of Calgary events, so I'm learning a bit more about how to apply
for my masters program. Overall, things are looking up!