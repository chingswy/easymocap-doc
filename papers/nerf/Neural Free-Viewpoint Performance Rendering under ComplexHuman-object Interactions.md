---
layout: default
title: Neural Free-Viewpoint Performance Rendering under ComplexHuman-object Interactions
parent: NeRF
grand_parent: 相关论文
mathjax: true
---

# Neural Free-Viewpoint Performance Rendering under ComplexHuman-object Interactions

[pdf](https://arxiv.org/pdf/2108.00362.pdf)

- 动机:  Recentadvances still fail to recover fine geometry and texture results fromsparse RGB inputs, especially under challenging human-object in-teractions scenarios
- 提出:  In this paper, we propose a neural humanperformance capture and rendering system to generate both high-quality geometry and photo-realistic texture of both human andobjects under challenging interaction scenarios in arbitrary novelviews, from only sparse RGB streams
- 难点:  complex occlusions raised by human-object interactions
- 方法：layer-wise scene decoupling strategy，同时考虑人体重建、物体重建与他们的联系
    - Occlusion-aware human reconstruction
    - Robust human-aware object tracking
    - Combines direction-aware neural blending weight learning and spatial-temporal texture completion

## Method

- Occlusion-aware Implicit Human Reconstruction: 从3D和2D里面提取feature
- Human-aware Object Tracking: template-based object alignment，用可微渲染获得pose。同时考虑物体mask，遮挡，mesh交叉
- Direction-aware Neural Texture BlendingL 
- Spatial-temporal Texture Completion：对遮挡区域使用纹理补全

## Limitations

1. 无法采集非刚性形变的物体，或者拓扑改变的，例如撕纸 => 打算采取关键帧volume更新
2. 无法处理手指细节，或者训练的时候没有见过的pose => 更大规模与高质量的数据
3. 无法处理透明物体，难以分割
