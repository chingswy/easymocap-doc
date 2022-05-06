---
layout: default
title: StructuredNF
parent: SMPL
grand_parent: 相关论文
mathjax: true
nav_order: 1
---

# Structured Local Radiance Fields for Human Avatar Modeling

**问题：** 从RGB视频里创建可驱动的人体模型
**关键问题：** 如何处理宽松衣服的运动
**方法：**
1. 使用一组结构化的局部的辐射场 <= 预定义在模板上的节点上
2. 将衣服的形变分解为骨架的运动、节点的运动、动态的细节变化
3. 使用conditional generative latent space来建模pose的泛化性

## 数据

1) Real-time deep dynamic characters中的两段裙子数据
2) Deepcap: Monocular human performance capture using weak supervision中的一段毛衣的数据
3) ZJU-MoCap中的两段数据
4) 自己用24个相机采的3段数据

