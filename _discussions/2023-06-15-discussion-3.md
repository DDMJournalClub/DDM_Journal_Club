---
title: "DDM 与 confidence 的讨论"
short_title: "DDM 与 confidence 的讨论"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# DDM 与 confidence 的讨论

Created: June 19, 2023 4:33 AM
Last edited by: Pan Wanke
Last edited time: June 19, 2023 5:10 AM
Owner: Pan Wanke

### 目前在众多“confidence”的版本中，哪种构念(construct)/构想(formulation)是最简洁或者general的？

信心是否可以形式化为「对预测误差的预测」。

- 即 “信心”是对于“A是B”或者“A等于B”的确定程度的信念/知觉/衡量；即为一种对（主观）不确定性的预测。
- 举例子的话：“我有信心目击到的确实是XXX”，意味着我觉得「目击记忆」等于「实际事件」的确定程度是高的，或者说我预测两者的差异很小或没有；“我有信心发表文章到XXX”意味着我觉得「实际投稿刊物」等于「目标投稿刊物」的确定程度是高的，或者说我预测两者的差异很小或没有。

confidence的构念，大的来说有两种，贝叶斯和非贝叶斯。

- 贝叶斯的confidence构念就等价于概率，尤其是贝叶斯推断当中的后验概率。
    - 即在观测到某个evidence以后，结合对event的先验概率，来推理event发生的后验概率是多少。
    - 虽然有一些计算模型会额外假设在生成后验概率之后，在report阶段还有一些额外的noise，但核心还是后验概率。
- 非贝叶斯的构念就有一些不同的观点了，
    - 如果从信号检测论的角度出发，confidence和evidence strength的强度是密切相关的。比如在一个知觉阈限实验中，要判断我是否探测到一个知觉刺激。那么我感知到的知觉强度越高，就意味着我越相信我探测到这个刺激。此时，信号检测论模型会直接在strength axis上设立一组criterion将axis划分为几段，strength越高的部分代表confidence越高。
    - 在DDM领域，确实有一种根据差异程度来定义confidence的例子，比如在决策过程截止的时候，如果我实际积累得到的evidence与boundary的位置越接近，那么我认为这个boundary代表的选项成立的信心程度就越高。

### confidence模型分为 one stage还是two stage。

- 为了简化，我在上面提出的几个构念其实都是one stage，就是说在decision形成的时候，confidence同时就通过某些方法形成了。
- 但是现在的另一种主流观点认为confidence的形成是在decision之后，它评估的要么是decision过程的某种属性，要么是（部分）独立于decision过程的另一个evidence accumulation或criterion-based decision的过程。
- 虽然确实two stage模型更靠近真实情况，但是这类模型在实际数据的拟合上比较复杂，实操层面有时候还是会使用one stage模型来简化模型拟合。

decision value/decision 和confidence的神经活动时空动态的问题；

- 之前就有研究发现在vmpfc，decision相关信号比confidence相关信号更早；
- 这篇nhb是结合颅内脑电和decoding，发现decode decision和confidence的电极点大部分是不重合的，即decision confidence的空间解剖基础是很不一样的；而且 成功decode decision的时间是大概250ms，而decode confidence的时间是450ms左右。

> Peters, M. A. K., Thesen, T., Ko, Y. D., Maniscalco, B., Carlson, C., Davidson, M., … Lau, H. (2017). Perceptual confidence neglects decision-incongruent evidence in the brain. *Nature Human Behaviour*, *1*(7), 0139. [https://doi.org/10.1038/s41562-017-0139](https://doi.org/10.1038/s41562-017-0139)
> 

### confidence 与 uncertainty

此外，uncertainty和confidence是无法分离的两个概念，confidence模型的核心是怎么定义uncertainty，以及怎么评估这个uncertainty

- meta其实就是higher level的意思，本质是针对decision本身的uncertainty

### 是否 race model 比 ddm 更适合 capture confidence，好像Fleming 2012年有篇nn是这样做的
