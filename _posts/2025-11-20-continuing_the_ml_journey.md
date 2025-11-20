---
title: Continuing the ML Journey
---

# Intro
It's been two weeks or so since my last post. I've been trying a ton of different things with the ML model, and I finally feel like I've made some real progress.

```
timestamp_iso8601,predicted_temperature_c
2025-11-10T07:47:19Z,26.633173
2025-11-10T07:52:17Z,26.633173
2025-11-10T07:57:15Z,26.633173
2025-11-10T08:02:13Z,26.979059
2025-11-10T08:07:11Z,26.979059
```
I've been able to train and run inference on a model that uses a sliding window of the past 15 mins to predict a single temperature value 15 mins into the future.

This is a small step towards what we actually want, which is to predict almost an entire day of temperature values.

The next thing I've been trying to do is modify the last layer (the output layer) of the model to output multiple temperature values instead of just one.

This has proven to be tricky so far. I spent almost a week troubleshooting random issues with the board. The board stopped writing new values to the SD card,
and it would randomly freeze and reboot.

```
[MEASUREMENT_LOGGER] REOPEN f_stat(0:/logs/measurements_2025-11-19.csv) -> fr=4 size=0 attr=0x00 
[MEASUREMENT_LOGGER] REOPEN open/create -> fr=0 
[MEASUREMENT_LOGGER] REOPEN header write bw=46 sync fr=0 
[MEASUREMENT_LOGGER] REOPEN new file seek eof -> 46 bytes 
[MEASUREMENT_LOGGER] Flushed 504 bytes to file 0:/logs/measurements_2025-11-19.csv on 2025-11-19T20:36:28Z.
[MEASUREMENT_LOGGER] CHECKPOINT: syncing and closing 0:/logs/measurements_2025-11-19.csv
```

Today I finally made some good progress on it. I reformatted the card, and new files seem to be showing up. I also added a MUTEX and some delays for the DS3231 RTC module
because it was returning no time values sometimes, which was throwing off the writing of data to the files.

I've let it run for the past 6 hours or so, and it seems to be much better now.

# Other News

In other news, we have a last video to record for our team in Shell for this year. We had the creative idea to record some videos of a pipeline
atop San-Fernando hill, so we'll see how it comes out. I also gave a pretty good talk on AI (specifically AI builder) to our subsurface/WRFM team. 
They were surprisingly really interested in it!

I just have 2 more weeks or so to wrap up my Shell work, after which I can focus on finishing the paper and preparing submitting that first draft to our journal.