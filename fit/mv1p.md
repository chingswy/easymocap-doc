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