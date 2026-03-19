---
title: "DIC/模型比较相关问题"
short_title: "DIC/模型比较相关问题"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# DIC/模型比较相关问题

Created: June 11, 2023 9:26 AM
Last edited by: Pan Wanke
Last edited time: June 11, 2023 9:26 AM
Owner: Pan Wanke

- 如何理解DIC

![Untitled]({{ '/assets/images/discussions/discussion-5/dic-comparison-1.png' | relative_url }})

### 单独看一个DIC值 没有其他的作比较 能判断这个模型拟合的好不好吗

不行。DIC只是一个相对指标，所以得看后验预测检查。

loo能够提供信息比DIC多一些，比如单个数据点的paret k指标

PPC可以对比MSE

- 注意，有时候DIC和PPC模型比较的结果会不一样，我记得胡传鹏老师之前遇到过类似问题

![Untitled]({{ '/assets/images/discussions/discussion-5/dic-comparison-2.png' | relative_url }})

[Bayesian Modeling and Computation in Python](https://bayesiancomputationbook.com/markdown/chp_02.html#pareto-shape-parameter)

[DIC + Posterior Predictive Checks](https://groups.google.com/g/hddm-users/c/qY_tUW737ds)

- **DIC为负数正常吗？**
    - DIC是相对的，用来模型比较的
    - 只要你检查过模型收敛没问题，那DIC为负数是没问题的
    
    [Negative DIC using Large Sample: > 200 trials and 500 people](https://groups.google.com/g/hddm-users/c/gs8VEKSA7R0/m/fK-z1VMLDgAJ)
