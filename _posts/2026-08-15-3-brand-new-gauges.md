---
title: "3 brand new gauges"
excerpt: "We got 3 brand new gauges, and our model pipeline generalized almost perfectly to them."
classes: wide
---

[![Three brand new gauges](/assets/posts/2026-08-15-3-brand-new-gauges/image_of_gauges.jpg){:width="100%"}](/assets/posts/2026-08-15-3-brand-new-gauges/image_of_gauges.jpg)

# Intro 

I just got 3 new gauges from Amazon! These are really good because they look very similar to the oil pressure gauges etc. at industrial sites, but they actually measure things at home. The gauge set consists of a temperature gauge, a humidity gauge and a pressure gauge.

I created 3 new gauge profile mappings (to map needle angle to the true value), and it turned out that our existing models worked really well on them! The models were able to generally find the needle and map it to the true value consistently across the ranges.

# Testing & Measurements

For each gauge, we chose three true points across the gauge face. We then compared the model's reading with the true value and recorded the absolute error below.

| Gauge | Point | True value | Model reading | Absolute error |
| --- | --- | ---: | ---: | ---: | 
| Temperature | 1 | -30.0 | -30.1 | 0.1 | 
| Temperature | 2 | 0.0 | 3.0 | 3.0 | 
| Temperature | 3 | 45.0 | 47.5 | 2.5 | 
| Humidity | 1 | 8.2 | 4.0 | 4.2 | 
| Humidity | 2 | 48.5 | 45.6 | 2.9 | 
| Humidity | 3 | 94.0 | 92.6 | 1.4 | 
| Pressure | 1 | 950.0 | 949.1 | 0.9 | 
| Pressure | 2 | 1010.0 | 1002.7 | 7.3 | 
| Pressure | 3 | 1072.0 | 1073.8 | 1.8 | 

The absolute error for each reading was calculated as:

```text
absolute error = |model reading - true value|
```

# Discussion

We can see that the models are generally able to track the positions of the needles in the gauges well, even though they haven't been specifically trained on these gauges. The few exceptions I noticed were when the gauge was against a bright glare spot (i.e. my lightbulbs at night, or the sun in the morning). I'll need to figure out how to alleviate that issue later.

The biggest reason for this good generalizable performance is because of the size of the training dataset. I was able to assemble over 8000 images of a diverse array of gauges. In each image, I labelled the gauge face, the needle center and the needle tip. I then trained a CNN to learn the features that would enable it to first find the gauge face from the entire photograph, then to crop the photo to that gauge face, then to pass this cropped image to a second model that would find the needle center and tip in the cropped photo. Seems that it's holding up pretty well so far!

[![CNN model process diagram](/assets/posts/2026-08-15-3-brand-new-gauges/process_diagrams-CNN_Model_Process.png){:width="100%"}](/assets/posts/2026-08-15-3-brand-new-gauges/process_diagrams-CNN_Model_Process.png)


# Next Steps

Next, I need to extend my hardware to allow the board to run off a 12V battery. If things work out well, I could actually get the chance to deploy my gauge reader in a live location soon!