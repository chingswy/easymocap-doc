---
layout: default
title: 开发者文档
parent: 实用工具
---

# 开发者文档
{: .no_toc }

1. TOC
{:toc}
---

## 总体架构

SMPL的拟合应该与一般的网络优化的结构接近，但是由于一些历史遗留原因，代码最开始的构造便与定义的网络优化不一样，因此整个结构相对来说不够优雅。

使用训练网络的过程来类比，整个过程分为

- Dataset
- Module
- forward
- Loss function
- Optimizer

在这个问题里面，每个部分会有不同的含义：

- Dataset: 这部分通常用来读取输入信息，这里的输入信息一般包括2D关键点，3D关键点，Mask等等
- Module: 这部分在这里指的是不同的模型，例如SMPL模型，SMPLH模型，MANO模型
- forward: 这里指的是根据输入参数，计算输出参数的过程
- Loss function: 这里指的是定义的loss模块，例如3D关键点距离，2D重投影误差
- Optimizer：类似的优化器，这里的区别是优化通常使用的是L-BFGS，而不是常用的Adam

与一般的网络优化的不同的地方在于，由于人体的拟合是一个高度非线性的过程，因此需要一些trick来进行初始化，并且不同的变量不能也最好不要一起优化，避免陷入局部最优解。

## 添加loss

对于一个loss，可以参考`easymocap/optimize/lossfuncs.py`里的一些常用的loss函数的写法