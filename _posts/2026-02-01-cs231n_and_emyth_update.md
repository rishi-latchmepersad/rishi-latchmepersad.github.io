---
title: Completed CS231n and The EMyth Revisited!
excerpt: "Reflections on finishing CS231n, revisiting The E-Myth, and clarifying a long-term direction in applied AI and engineering work."
description: "Reflections on finishing CS231n, revisiting The E-Myth, and clarifying a long-term direction in applied AI and engineering work."
---

# CS231n
This weekend, I managed to complete my audit of the CS231n course (the first iteration of the course in 2016 led by Andrej Karpathy at Stanford ). It was an excellent course, mainly because I could feel the excitement and the experience from Andrej on the subject. I could tell that he really liked the entire domain, and that he had been working in this domain for years. I definitely learnt some new things about CNNs and image recognition tasks. It wasn't a perfect course, because it's a bit dated now and because my own domain is a specific subset of what he taught, but I think it was a really good experience. It also gave me the chance to hear for the first time from true leaders in the field like Fei Fei Li and Jeff Dean. I'm hoping that I can attend a conference from one of them some time in the future.

[![CS231n](/assets/posts/2026-02-01-cs231n_and_emyth_update/cs231n.jpg){:width="600"}](/assets/posts/2026-02-01-cs231n_and_emyth_update/cs231n.jpg)

# Main Takeaways from CS231n

I didn't focus as much on the deep technical bits of the course, because I didn't challenge myself to actually complete the assignments in it. Instead, I wanted to focus on the broad generalizable concepts that could apply to my own plans for the future. Here's what I took away in a nutshell:

- AI is a giant field and there are lots of brilliant people working in all sorts of areas within it. The 3 main areas I picked up were AI Model Research (studying how models are built and improving their output, creating new models, stacking different models together etc.), Applied AI (taking standardish models and finding a domain to use them properly in real world scenarios) and AI Performance (finding out how to write frameworks to code the AI models in, and optimise them to be the best that they can be in terms of speed, memory usage etc.). I definitely want to focus on Applied AI.
- There are lots of little tricks you learn from building muscle memory and learning to use a particular framework. (eg. some hyperparameters that you just learn to set for a particular model). I hope that I can learn things like this as I use Keras and TFLM more.
- Transfer learning (taking a pretrained model and fine-tuning it to a new task) and data augmentation (applying random transformations to the data to make it look more like the real world and extend the dataset) are two fantastic tools that we should almost always use. I believe that both of these are applicable to my own work, and I hope that I can figure out exactly how to do them. 

# The E-Myth Revisited

I also got the chance to finish the book: The EMyth Revisited by Michael Gerber. There were a lot of great concepts in here, and I know for sure a lot of them went over my head, but there are a few main concepts I'll capture below to help me remember them later.

[![E-Myth](/assets/posts/2026-02-01-cs231n_and_emyth_update/emr.png){:width="600"}](/assets/posts/2026-02-01-cs231n_and_emyth_update/emr.png)

# Main Takeaways from The E-Myth Revisited

- Most people who start businesses look from an inward -> outward approach, starting with the skills they have and looking for a way to sell those skills or that product. Successfully businesses look outward -> in, finding out what customers want and how they could possible meet them there.
- Businesses are not sustainable and scalable with any business person at the center. They need to be designed as a system that can be replicated and operated independently. This can be achieved through a lot of hard work in documentation of processes, design language, diagrams etc.
- There are 3 main people that we need to become to become successful entrepreneurs. We need to be technicians, entrepreneurs and managers. Technicians are responsible for the design of the product or service that we are selling. Entrepreneurs are responsible for the sales and marketing of the business, and managers are responsible for ensuring a smooth day-to-day running of the business. All 3 of these are necessary to make a business work. 

# Conclusion

I feel like both of these things have really solidified my foundation of where I want to go. I've also taken my first set of training images on my gauge, and I'm currently labelling the images to use in my initial pipeline work. Over the next month, I plan to start this pipeline build and hopefully build a CNN on my laptop that's able to recognise my temperature gauge needle and recognise the temperature on the gauge with certain level of confidence.
