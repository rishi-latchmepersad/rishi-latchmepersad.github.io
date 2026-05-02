---
title: First Trial Results - Classical CV vs CNN
excerpt: "Running my first comparison between the classical computer vision baseline and the CNN-based approach on the STM32 N6."
description: "Running my first formal comparison between the classical computer vision baseline and the CNN-based approach on the STM32 N6."
classes: wide
header:
  teaser: /assets/posts/2026-05-02-first_results/setup.jpg
---

# Overview

It's been a while since the last update, but I've made some solid progress! I finally got the whole pipeline working end-to-end on the STM32 N6: capture an image from the camera, run inference, and log the results to the SD card. More importantly, I ran my first formal trial comparing two approaches side-by-side.

Here's the current setup:

[![Current Setup](/assets/posts/2026-05-02-first_results/setup.jpg){:width="600"}](/assets/posts/2026-05-02-first_results/setup.jpg)

The STM32 N6 board is at the bottom, connected to a breadboard with the SD card module and a few jumper cables. The camera is hung on the edge of the box, with another box holding it in place, pointed at the analog temperature gauge. I use a ruler to measure the distance between the camera and the gauge, and a phone app to measure the brightness of the room.

# Two Approaches

I'm running two different algorithms on the same hardware, to systematically measure how well (or poorly) I'm doing:

## 1. Classical CV Baseline

I have set up a traditional computer vision pipeline that processes the raw YUV422 frames directly on the MCU. It uses centroid detection, polar edge voting, and some ad-hoc heuristics to figure out where the needle is pointing. Here's the full process:

[![Classical CV Process](/assets/posts/2026-05-02-first_results/process_diagrams-Classical_CV_Baseline_Process.drawio.png){:width="100%"}](/assets/posts/2026-05-02-first_results/process_diagrams-Classical_CV_Baseline_Process.drawio.png)

The nice thing about this approach is that it doesn't need any training data — it's all hand-crafted logic. But as you'll see in the results, it struggles with certain edge cases. It's really accurate when it actually finds the correct edge for the needle, but it jumps around a lot, likely because it's detecting other dark edges and interpretting it as the needle.

## 2. CNN Model

This is the first real model I built. A fine-tuned MobileNetV2 that runs on the Neural Processing Unit (NPU). The model takes a cropped image of the gauge face and outputs a temperature directly. The pipeline looks like this:

[![CNN Process](/assets/posts/2026-05-02-first_results/process_diagrams-CNN_Model_Process.drawio.png){:width="100%"}](/assets/posts/2026-05-02-first_results/process_diagrams-CNN_Model_Process.drawio.png)

The OBB (Oriented Bounding Box) localizer was a bit tricky to get working, but it helps keep the crop aligned even if the camera shifts or reorients slightly. The model itself is quantized to int8 and runs entirely on the NPU, which allows inference to compute quickly and keeps the main CPU free for other tasks (writing the data to the SD card etc.).

# The Results

I ran a controlled trial across the full temperature range (-30°C to 50°C) under ideal conditions: camera 15cm from the gauge, bright room (62 lux), no occlusion or dirt. Here's what I got:

[![Trial 1 Results](/assets/posts/2026-05-02-first_results/trial_1_results.JPG){:width="100%"}](/assets/posts/2026-05-02-first_results/trial_1_results.JPG)

Some observations:

- **The CNN is generally more accurate** - look at the errors for -20°C, -15°C, and -5°C. The baseline is off by 20+ degrees in some cases, while the CNN stays within a few degrees.
- **The CNN struggles at the extremes** - at -30°C and 50°C, the CNN really leans on the calibration layer to improve its readings.
- **Around room temperature (20-30°C), both work reasonably well** — this is where most of my training data was, so it's not surprising.
- **The baseline has some weird outliers** - at 15°C it reads 36.4°C, which is way off. I think this is where the centroid detection gets confused by the gauge's internal humidity dial, or where the other lines on the gauge face throw the polar voting system off the needle.

Overall, the CNN is the clear winner for accuracy, but it's important to implement the baseline so that I show the merits of Neural Networks properly. After I moved the needle each time, the baseline took a lot longer to produce a reasonable value. In some rows, you'll see that it never produced anything reasonable. The CNN immediately produced something in the realm of possibility.

Across this controlled trial, the classical CV baseline achieved approximately 10.9 °C MAE, while the CNN achieved approximately 2.4 °C MAE, reducing average error by about 78%. I believe we can improve this further and make it more generalizable to more gauges in different (non-ideal) conditions.

# What's Next

There's still plenty to do:

1. **Train to improve the CNN's performance at the temperature extremes** - I don't want to keep relying on the calibration layer at the outer temperatures.
2. **Test under non-ideal conditions** - different lighting, camera angles, dust on the gauge, etc.
3. **Hardware cleanup** - those jumper cables are still flaky. I need swap them out to proper dupont cables, and eventually get a proper PCB made and solder everything down.
4. **ViT** - I believe a Vision Transformer model may also be a viable candidate to do this task on this board. I want to try to implement it and see.
5. **NPU vs CPU** - I want to test to what extend the NPU speeds up inference. There should be a compilation flag I can set to disable it.
6. **Proper power measurement** - I just bought an INA219 chip and a USB A cable. I want to reroute the power to the board through this chip, so that I can sample power quickly while inference is being performed. This will allow me to compare the power usage of each model, compare the NPU vs CPU in terms of power usage etc.

More updates soon!
