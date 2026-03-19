---
title: "DDM是否能用于长RT任务？"
short_title: "DDM是否能用于长RT任务？"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# DDM是否能用于长RT任务？

Created: June 11, 2023 9:26 AM
Last edited by: Pan Wanke
Last edited time: September 6, 2023 1:37 PM
Owner: Pan Wanke

> we applied the diffusion model to a wide range of different **slow RT tasks** including arithmetic and semantic tasks. All of these tasks had mean RTs **higher than 1.5 s but smaller than 5 s** and provided a good model fit.

according to Ratcliff et al. (2004), the “model is designed to apply only to relatively fast two-choice decisions and to decisions that are composed of a single-stage decision process (as opposed to the multiple-stage decision processes that might be involved in, for example, reasoning tasks or card-sorting tasks). As a rule of thumb, the model would not be applied to experiments in which mean RTs are much longer than about 1–1.5 s.” (p. 279).  

Lerche, V., & Voss, A. (2019). Experimental validation of the diffusion model based on a slow response time paradigm. *Psychological Research*, *83*(6), 1194–1209. [https://doi.org/10.1007/s00426-017-0945-8](https://doi.org/10.1007/s00426-017-0945-8)
> 

> 技术角度上这些模型可以用在任何rt。从rt形状的角度来看，ddm产生的预测更适合偏态分布。而很多时候rt很长的实验，偏态的程度会比较小一些。而别的一些模型，例如lba或者collapse boundary，它们的预测值没有ddm那么偏态，可能拟合的程度会好一些。 —— 郭鸣谦
>
