---
layout: default
title: 多视角单人
parent: 离线优化
nav_order: 1
---

# 多视角单人
{: .no_toc }

<details close markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>
---

## 多视角单人
拟合
```bash
python3 apps/fit/fit_model.py --cfg_data config/data/mv1p3d2d.yml --cfg_model config/model/smpl_neutral.yml --cfg_exp config/optimize/mv1p.yml --out ${out} --opt_data k2d ${data}/output/keypoints2d k3d ${data}/output/keypoints3d camera ${data}
```

可视化

```bash
subs="['01', '07', '13', '19']"
python3 apps/vis/vis.py --cfg config/vis2d/smpl_image.yml input_args.images ${data} input_args.subs ${subs} result_args.skel_path ${out}/smpl output_args.out ${out}/mesh input_args.scale 0.5
```

渲染结果如图所示：

<div align="center">
<video width="70%" playsinline="" autoplay="autoplay" loop="loop" preload="" muted=""><source src="../images/fit/mv1p/mv1p-smpl.mp4" type="video/mp4">
</video>
</div>

## 帧率相同、开始时间不同

这部分主要面向LightStage数据，LightStage相机使用了硬触发，但是由于相机距离不同，导致不同相机的开启时间有<20ms的差异，因此需要各个视角优化一下。

算法假设：
- 各相机帧率一致
- 各相机起始点不一致
- 各相机的时间差小于1帧

拟合

```bash
python3 apps/fit/fit_model.py --cfg_data config/data/mv1p3d2d.yml --cfg_model config/model/smpl_neutral.yml --cfg_exp config/optimize/mv1p-unsync.yml --out ${out} --opt_data k2d ${data}/output/keypoints2d k3d ${data}/output/keypoints3d camera ${data} --write_mv
```

可视化

```bash
python3 apps/vis/vis.py --cfg config/vis2d/smpl_unsync_image.yml input_args.images ${data} subs ${subs} result_args.skel_path ${out}/smpl output_args.out ${out}/mesh-unsync input_args.scale 1
```

## 少视角单人
少视角与多视角的区别是：
- 无法使用多视角一致性来过滤错误的关键点，只能在拟合的时候使用鲁棒性的核函数来避免噪声。
- 无法预先重建出3D关键点，只能通过SPIN或其他方式来进行初始化

拟合：

```bash
python3 apps/fit/fit_model.py --cfg_data config/data/mvmp2d-hand.yml --cfg_model config/model/smplh_male.yml --cfg_exp config/multistage/sv1p-hand.yml --out ${out} --opt_data k2d ${data}/annots camera ${data} --opt_exp cache ${data}/output/spin
```

可视化：

```bash

```

## 单视角单人

如果没有特殊需求，单视角单人部分可以使用少视角单人的代码。
