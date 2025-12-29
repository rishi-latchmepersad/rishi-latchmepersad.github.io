---
title: Final Month Of The Year!
---

# Intro
The last few weeks have been pretty productive. 😀

I have no idea why I keep getting sick. I got the flu again last week. Luckily, this one didn't last that long. I'll need to really keep on-top of my vitamin C.

Over the last few weeks, I've been fine-tuning our CNN temperature prediction model. We've started to see some issues with Google Colab. I thought it was a fantastic platform for a long time, but now I notice how quickly it times out, and how inconvenient it is to not be able to leave your model running while it's training. I'll definitely be trying to build my next model on a local machine.

I added a few more layers to the CNN, and it actually performs much better now! It's able to predict the pattern of temperature really well. The only issue is that the predictions are constantly lower than the actual temperature. I believe this is because a large part of the training data was from the Open-Meteo dataset, which generally had lower temperatures than what's seen at my house.

[![Temperature Range](/assets/posts/2025-12-01-final_month_of_the_year/temperature_different_sources.JPG){: width="600" }](/assets/posts/2025-12-01/temperature_different_sources.JPG)

In this dataset, our local sensor data is to the tail end. You can see that our local temps are much higher than the API data. I believe this is causing the model to forecast much lower temperatures than actual.

# Other News

In other news, I bought a new motherboard! This will be like my 5th one in the last 5-6 years. I have a UPS this time, so maybe it will be better? I can only hope. I'm hoping to spend most of my December (after the paper) playing some new video games. Maybe Red Dead Redemption? 🐎 I'll see once I get it working. I'm also looking forward to spending more time with my wife baking and making Christmas treats.