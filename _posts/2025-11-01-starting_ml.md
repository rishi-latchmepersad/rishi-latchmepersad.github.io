---
title: Starting Machine Learning!
---

# Intro
Wow it's been almost 2 months since my last post. Unfortunately, I came down with the flu 😷, and I also had several key family
events that took up time (such as Divali 🔥). 

Luckily, we're back now! Over the past couple of weeks, I've made some small improvements to the project, and we've had
some exciting news as well!

# Machine Learning
I started working on a Google colab notebook to create a simple ML model to predict temperature. For now, I'm just using an open
API to source the training dataset, but eventually we'll switch over to using our own collected data. 

[![Colab](/assets/posts/2025-11-01-starting_ml/initial_google_colab.JPG){: width="600" }](/assets/posts/2025-11-01-starting_ml/initial_google_colab.JPG)

For now, I just want to understand the overall process of training a model, exporting it, and running it on the microcontroller.
Right now, we're building a simple 1D-CNN model, training it to get a Mean Absolute Error (MAE) of under 1 degree Celsius,
then pruning and quantizing it to run on the microcontroller. We're using TFLite Micro to create the output file, and
I'm currently troubleshooting an issue with deploying the model on my STM32 board. I've downloaded STM32 Cube AI, but I'm 
getting an error when I try to import the TFLite model. Hopefully I can sort it out soon.

[![STM32 Cube AI error](/assets/posts/2025-11-01-starting_ml/ml_error.JPG){: width="600" }](/assets/posts/2025-11-01-starting_ml/ml_error.JPG)

# Other News
While we were running around the Queens Park Savannah a day after work, we luckily bounced up a manager of another team at Shell.
We happened to mention our project to him, and he was very interested! He mentioned that he plays golf, and he would love to be able
to have weather data for the course. He said that he would bring it up with the members of his club, and let us know what can work.

This is really exciting, but also sort of scary. All we have working right now is a rough prototype. It's no where near ready
to go out into the field to collect and display data for anyone. It's just a bundle of wires.

[![Current Project](/assets/posts/2025-11-01-starting_ml/current_project.JPG){: width="600" }](/assets/posts/2025-11-01-starting_ml/current_project.JPG)

Hopefully we can ramp up our development and focus a bit more on the practical side of things. 😀