<!--
 * @Date: 2021-11-17 14:40:59
 * @Author: Qing Shuai
 * @LastEditors: Qing Shuai
 * @LastEditTime: 2021-11-17 17:40:18
 * @FilePath: /easymocap-doc/tools/annotation.md
-->
---
layout: default
title: 标注工具
parent: 实用工具
nav_order: 2
---

# 标注工具

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
