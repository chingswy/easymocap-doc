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
python3 apps/vis/vis.py --cfg config/vis2d/smpl_image.yml input_args.images ${data} input_args.subs ${subs} result_args.skel_path ${out}/smpl output_args.out ${out}/mesh input_args.scale 0.5
```

## 帧率相同、开始时间不同的多相机场景

这部分主要面向LightStage数据，LightStage相机使用了硬触发，但是由于相机距离不同，导致不同相机的开启时间有<20ms的差异，因此需要各个视角优化一下。

算法假设：
- 各相机帧率一致
- 各相机起始点不一致
- 各相机的初始时间偏移在平均值的两侧分布


