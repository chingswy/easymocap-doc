---
layout: default
title: 多视角多人姿态估计
parent: 实时捕捉
nav_order: 3
---

# 多视角单人姿态估计
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

## Overview

开启可视化服务器

```bash
python3 apps/vis/vis_server.py --cfg config/vis3d/o3d_scene.yml
```

LightStage数据：
```bash
python3 apps/demo/mvmp_realtime.py ${data} --out ${data}/output --cfg config/exp/mvmp_lightstage.yml --undis --vis3d --host 127.0.0.1
```

FLIR工业相机数据：
```bash
python3 apps/demo/mvmp_realtime.py ${data} --cfg config/exp/mvmp_1920x1080.yml --vis_repro --out ${data}/output --cfg_opts width 1296 height 972 --undis
```
