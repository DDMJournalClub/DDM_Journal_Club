---
title: "HDDM安装问题"
short_title: "HDDM安装问题"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# HDDM安装问题

Created: June 11, 2023 9:26 AM
Last edited by: Pan Wanke
Last edited time: June 11, 2023 9:26 AM
Owner: Pan Wanke

## 最近更新

![Untitled]({{ '/assets/images/discussions/discussion-9/hddm-install-update.png' | relative_url }})

具体请参考 [HDDM安装以及问题汇总 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/389906139?)

[Docker 安装 HDDM](Docker%20%E5%AE%89%E8%A3%85%20HDDM%2020ed2f28244743a1b866b74199801127.md) 

## 安装的一般流程

来自于 @朱昱豪

HDDM安装完整流程(WIN)
首先，把老版本的包删除干净
#1创建环境 conda create -n py37 python=3.7
conda activate py37
以下步骤均在py37虚拟环境中执行 （你也可以试试直接在你的3.9里执行下面的操作）
#2安装PyMC pip install pymc==2.3.8
#3 安装kabuki 和hddm
1)下方链接下载解压
链接：[https://pan.baidu.com/s/1KRzcNJaMafkoKEzimKDYRw?pwd=HDDM](https://pan.baidu.com/s/1KRzcNJaMafkoKEzimKDYRw?pwd=HDDM) 提取码：HDDM
2)conda中 cd 到目录 python [setup.py](http://setup.py/) install
坑： 1直接pip安装：可以装，但是版本太低，没法用LAN
2 github安装：版本最新，但是网络老有问题解决办法：从github上下载源码code，解压，本地install（已经打包在链接里）
#4 安装pytorch(可选，如果要用LAN内容，如HDDMnn，需要这个)
pip install torch torchvision -i [https://pypi.mirrors.ustc.edu.cn/simple/](https://pypi.mirrors.ustc.edu.cn/simple/)
坑：网速太慢   解决办法：换源加速
#5 创建jupyter
kernel python -m ipykernel install --user --name 环境名称 --display-name "Python (环境名称)"
#6 完成，检验
执行代码：
import hddm
print(hddm.**version**())

## 通过 docker 进行安装

[A Hitchhiker's Guide to Bayesian Hierarchical Drift-Diffusion Modeling with dockerHDDM](https://psyarxiv.com/6uzga/)

## pymc问题

### container_value

![Untitled]({{ '/assets/images/discussions/discussion-9/pymc-container-value.png' | relative_url }})

https://github.com/pymc-devs/pymc/issues/428

- [ ]  为解决

对了下午关于pymc的安装，宇航的方法非常好用，conda install pymc==2.3.8在安装过程会自己适配好需要的东西，并且安装成功。不过也确实需要注意numpy的版本问题，这种方法的安装后的numpy会变成1.13版本，导致hddm无法调用一个随机变量的生成函数，需要更新到最新的即可。还有一个就是补充安装下scipy的包，别的都ok啦
