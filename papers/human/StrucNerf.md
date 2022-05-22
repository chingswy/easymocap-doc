---
layout: default
title: StructuredNF
parent: SMPL
grand_parent: 相关论文
mathjax: true
nav_order: 1
---

# Structured Local Radiance Fields for Human Avatar Modeling


- **问题：** 从RGB视频里创建可驱动的人体模型
- **关键问题：** 如何处理宽松衣服的运动
- **方法：**
  - 使用一组结构化的局部的辐射场 <= 预定义在模板上的节点上
  - 将衣服的形变分解为骨架的运动、节点的运动、动态的细节变化
  - 使用conditional generative latent space来建模pose的泛化性

## 之前的工作

问题：they all heavily rely on the skeleton or the
surface of SMPL model for cloth motion modeling
缺陷:
- only holds for tightfitting clothes
- wrinkles and non-rigid deformations
- body pose => cloth details 是一个one-to-many的映射

## 方法

提出了新的表达

- global NeRF => a set of structured local radiance fields
  - 定义在SMPL上的定义的nodes上
  - 每个local网络只负责局部的node
  - 可以被骨架驱动，但同时有单独的运动
  - 每个local的有单独的embedding，用来建模node translation无法表达的

coarse-to-fine manner: 骨架运动 => local node的residual movements => time-varying details

这样做的难点：
1. node相关的变量很难直接获得：训练帧可以过拟合，没见过的pose不知道怎么计算。如果用pose直接回归，那么这是一个under-fitting的问题


## 数据

- Real-time deep dynamic characters中的两段裙子数据
- Deepcap: Monocular human performance capture using weak supervision中的一段毛衣的数据
- ZJU-MoCap中的两段数据
- 自己用24个相机采的3段数据

