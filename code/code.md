---
layout: default
title: Develop
has_children: true
---

# Develop
{: .no_toc }

1. TOC
{:toc}
---

## body model

这部分定义了所使用的多种人体参数化模型，规定了函数的输入输出，每个参数化模型的输入是一组参数，输出是

- Skeleton
- SMPLv1.1

```bash
cd data/bodymodels
mkdir smplv1.1.0
cp SMPL_python_v.1.1.0/SMPL_python_v.1.1.0/smpl/models/basicmodel_* smplv1.1.0
```

- SMPLHv1.2

Download `smplh.tar.xz` from [here]() and place it in `data/bodymodels/`.

- MANO
- FLAME
- STAR: A Sparse Trained Articulated Human Body Regressor: Register and download [here](https://star.is.tue.mpg.de/)

## dataset

这部分代码定义了基础的数据集，复杂类型的数据集需要从基础类型继承而来。

## Related works

- [Shape-aware Multi-Person Pose Estimation from Multi-View Images](https://openaccess.thecvf.com/content/ICCV2021/papers/Dong_Shape-Aware_Multi-Person_Pose_Estimation_From_Multi-View_Images_ICCV_2021_paper.pdf)
- [Direct Multi-view Multi-person 3D Pose Estimation](https://openreview.net/pdf?id=rG2ponW2Si)
- [3D Human Pose and Shape Estimation Through Collaborative Learning and
Multi-view Model-fitting](https://openaccess.thecvf.com/content/WACV2021/papers/Li_3D_Human_Pose_and_Shape_Estimation_Through_Collaborative_Learning_and_WACV_2021_paper.pdf)
