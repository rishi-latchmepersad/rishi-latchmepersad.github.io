---
title: Some Surprising Twists on Our First ML Paper
---

# Intro

Last month we got some pretty bad news. Our first ML paper was rejected by the journal. We had initially submitted a paper on forecasting temperature to the West Indian Journal of Engineering at the end of December last year. We had pretty high hopes, but the review notes were brutal. We initially thought that was the end of it.

# Hope

However, right after the rejection email, the journal owner sent a follow-up email, asking us to actually implement the changes proposed and submit the paper to another journal. This was really unexpected, but we felt good about it. We felt particularly good because the review notes were actually quite detailed. I got the feeling that the author actually wanted us to fix the mistakes and publish the paper.

[![Journal Owner](/assets/posts/2026-03-12-surprising-twists-on-our-paper/wije_feedback.JPG){:width="600"}](/assets/posts/2026-03-12-surprising-twists-on-our-paper/wije_feedback.JPG)

# Fixing the Mistakes

In a nutshell, the feedback was around the structure of the paper. I mistakenly focused too much on the basics of Neural Networks, while the paper demanded more rigorous research and novelty. Additionally, besides jumping to a NN, the reviewer wanted to see a baseline model (forecasting temperature without a NN) and and ablation model (a NN without engineered features). We therefore set out to fix these mistakes.

# Initial outcomes

Unfortunately, when we implemented the baseline and our ablation model, we found out just how poorly our models did against them. The baseline model performed the best (was able to forecast closet to the true temperatures), the ablation model was in the middle, while our fully-featured model performed the worst. We knew we had no way to justify this and publish a paper that we would be proud of.

# Another twist

We therefore emailed the journal coordinator, and informed him that we wouldn't be able to complete our updates by the time required for his journal. However, he surprised us again by shifting the deadline back a month. This was amazing! With our extra month, we could comfortably investigate the 3 types of models, and design a model that could truly beat the baseline and an ablation.

# Conclusion

I've just completed the development of a new CNN with modified features. Hopefully it can outperform the baseline and an ablation. Once I've captured the results and submitted the paper, I will post again 🙂
