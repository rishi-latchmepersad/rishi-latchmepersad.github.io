---
title: Our first samples of inference
excerpt: "The first examples of inference from our STM32 N6 board and our analog temperature gauge."
description: "The first examples of inference from our STM32 N6 board and our analog temperature gauge."
---

# Overview

After a few more weeks, I was able to take the raw image on the sd card, and run inference on it, to convert the image into a temperature value. The model required a ton of finetuning from the initial dataset, because its predictions were 20-30 degrees off at first, which was pretty bad. But once I got the model working, I was able to get some interesting results.

For example, this image:
[![Example Image](/assets/posts/2026-04-06-working_inference/example.png){:width="600"}](/assets/posts/2026-04-06-working_inference/example.png)

Was inferred to be 31 degrees! Pretty close!
```
[CAMERA][CAPTURE] Processed ROI stats (processed-capture): roi=32x32 start=(96,96) y_min=16 y_max=61 y_mean=49.60 center8=[38,37,38,39,39,40,39,39].
[CAMERA][CAPTURE] Saving YUV422 capture to /captured_images/capture_2026-04-06_11-29-04.yuv422 (100352 bytes)...
[WATCHDOG] pulse
[WATCHDOG] pulse
[WATCHDOG] pulse
[WATCHDOG] pulse
[CAMERA][CAPTURE] Stored 100352-byte YUV422 image at /captured_images/capture_2026-04-06_11-29-04.yuv422.
[AI] Dry-run snapshot copied: src=0x24160000 dst=0x340a9b20 len=100352 first8=[10 84 10 81 10 84 10 82]
[AI] Queueing dry-run request: ptr=0x340a9b20 length=100352
[CAMERA][THREAD] Capture and inference completed successfully.
[AI] Worker dequeued frame: ptr=0x340a9b20 length=100352 semaphore_status=0
[AI] Inference value: 31.2
[INFER_LOG] Inference value: 31.2
[INFER_LOG] Logged: 2026-04-06 11:29:23,31.1
[WATCHDOG] pulse
```

I've made some changes to the initial image format from the last post. I now capture the image in a 224x224 colour format, since this is what MobileNetV2 was trained on, and my current model is a fine-tuned version of MobileNetV2.

I've also set up the inference to run in a loop, and boot the firmware from flash instead of only loading the firmware from the PC in development mode. This means I can power the system from anywhere and it will run the inference loop without me having to keep the PC running.

# The Paper

Tevin and I also finished the paper this weekend. It really reads much better now! Hopefully this one is accepted by the IEMJ. We'll have to wait a few weeks and see.

[![Example Image](/assets/posts/2026-04-06-working_inference/tinyml_submission_2.png){:width="600"}](/assets/posts/2026-04-06-working_inference/tinyml_submission_2.png)

# Next Steps
The biggest issue right now is that the jumper cables on the board aren't making great contact, so I need to keep wiggling them around. I need to order a solderable PCB and start mounting these components to that instead, and use solderable jumper wires as well. While I'm waiting for those parts to reach, I'll try to neaten up my code so that we can scale the firmware going forward, because right now it's a bit clunky and bloated.
