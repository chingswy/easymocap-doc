---
layout: default
title: 多视角单人姿态估计
parent: 实时捕捉
nav_order: 1
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

运行代码

```bash
python3 apps/realtime/mv1p.py ${data} --host3d 127.0.0.1:9999 --robust --out ${data}/output
```

此时可以在3D可视化窗口看到如下结果：

<div align="center">
<video width="70%" playsinline="" autoplay="autoplay" loop="loop" preload="" muted=""><source src="../images/realtime/mv1p-scene.mp4" type="video/mp4">
</video>
</div>

使用vis工具进行可视化：

```bash
python3 apps/vis/vis.py --cfg config/vis2d/skel_mv_repro.yml images ${data} subs "['01', '07', '13', '19']" skel ${data}/output/keypoints3d out ${data}/output/skel make_video True scale 0.4
```

将会看到这样的输出结果：

<div align="center">
<video width="70%" playsinline="" autoplay="autoplay" loop="loop" preload="" muted=""><source src="../images/realtime/mv1p-repro-01071319.mp4" type="video/mp4">
</video>
</div>
