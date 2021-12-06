---
layout: default
title: 标注工具
parent: 实用工具
nav_order: 2
---

# 标注工具
{: .no_toc }

这部分包含了多种数据标注的工具，基于OpenCV实现。

1. TOC
{:toc}
---

## 关键点标注

```bash
python3 apps/annotation/annot_keypoints.py ${data}
```

|name|value||
|----|----|----|
|ext|.jpg/.png|图像后缀|
||||

## Mask标注

标注单人的mask 
```bash
python3 apps/annotation/annot_mask.py ${data} --mask mask_cihp
```

标注多人的mask：TODO

## 篮球标注

```bash
python3 apps/annotation/annot_track.py ${data} --annot basketball --max_person 1 --step 5
```

## 标注背景的mask



## 多视角同步

需求：
- 多个窗口一起移动
- 单个窗口选中之后可以单独移动
- 记录关键帧
- 保存关键帧

## 多视角标注

需求：
- 多个窗口一起移动
- 选中某个视角里的某个人
