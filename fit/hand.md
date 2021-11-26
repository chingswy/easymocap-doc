---
layout: default
title: 手部优化
parent: 离线优化
nav_order: 2
---

# 手部优化
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

## 固定手部姿态拟合SMPLH

拟合

```bash
python3 apps/fit/fit_model.py --cfg_data config/data/mv1h.yml --cfg_model config/model/smplh_male_full.yml --cfg_exp config/multistage/fixhand.yml --out ${out} --opt_data k2d ${data}/annots camera ${data}
```

可视化

```bash
python3 apps/vis/render.py --cfg config/render/model_image.yml model config/model/smplh_male_full.yml images ${data} camera ${data} result ${out}/smpl out ${out}/mesh
```

## 拟合MANO左手

```bash
python3 apps/fit/fit_model.py --cfg_data config/data/mv1h.yml --cfg_model config/model/mano_noflat_pca_comps45.yml --cfg_exp config/multistage/sv1p-mano.yml --out ${out} --opt_data k2d ${data}/annots camera ${data} --opt_exp initialize.init_spin.args.cache_path ${data}/output/manol
```


## 固定手部姿态拟合MANO左手

拟合
```bash
out=${data}/output/mano
python3 apps/fit/fit_model.py --cfg_data config/data/mv1h.yml --cfg_model config/model/mano.yml --cfg_exp config/multistage/fixmano.yml --out ${out} --opt_data k2d ${data}/annots camera ${data}
```

可视化
```bash
python3 apps/vis/render.py --cfg config/render/model_image.yml model config/model/mano.yml images ${data} camera ${data} result ${out}/smpl out ${out}/mesh input_args.image_args.scale 0.25
```

