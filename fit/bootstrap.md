---
layout: default
title: Bootstrap
parent: 离线优化
---

# Bootstrap
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

## 预检测

{: .note }
以下内容均是针对图片中只有单人的情况

{: .note }
目前的版本只实现了top-down的方法，假设bbox检测一定是准确的。

对于人体数据，首先需要使用YOLO进行检测：

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode yolo
```

对检测好的人体，需要人工核实一遍，补上缺失的框

```bash
python3 apps/annotation/annot_track.py ${data} --max_person 1
```

这里不补好像也没关系，后面会自动筛选出来。

确认无误后，使用预训练的网络，OpenPose/HRNet检测一遍所有框。

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode hrnet --force
```

如果需要检测脚，运行OpenPose检测脚部关键点

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode feet --force
```

## 多视角重建

进行单帧的多视角重建：
- 输入：单帧多视角单人2D关键点，相机参数
- 输出：重建出来的3D关键点

{: .note }
这里重建的结果暂时不考虑遮挡问题，直接全部拿去训练。

多视角重建：
```bash
python3 apps/realtime/recon_body.py ${data} --min_view 4
```
这里限定最少使用4个视角。

观察重建之后的结果：
```bash
python3 apps/annotation/annot_keypoints.py ${data} --annot annot_0
```
如果某一帧的结果是错误的，那么对**原始数据**进行标注修改，再重新运行多视角重建。

```bash
python3 apps/annotation/annot_mv_keypoints.py ${data}
```

