---
layout: default
title: 预处理
parent: 快速开始
nav_order: 2
---

# 预处理
{: .no_toc }

1. TOC
{:toc}
---

## 提取图片

将视频转换成图片，如果数据已经处理成了图片，可跳过该步骤

```bash
python3 apps/preprocess/extract_image.py ${data} --strip video_ --num 2000 --start 0
```

|参数|含义|
|----|----|
|--strip|将视频名字中的该项去除|
|--num|从视频中提取的最大帧数|
|--start|起始帧数|

## 提取关键点
|参数|含义|
|----|----|
|--openpose|OpenPose的安装路径|
|--hand|同时提取出手部关键点|
|--face|同时提取出脸部关键点|
|--ext jpg|图片后缀，默认jpg|
|--force|强制重新开始，默认不重新开始|

### OpenPose

```bash
data=/path/to/data
python3 apps/preprocess/extract_keypoints.py ${data} --openpose /path/to/openpose
```

同时提取出手部关键点与脸部关键点
```bash
python3 apps/preprocess/extract_keypoints.py ${data} --openpose /path/to/openpose --hand --face
```

### 使用YOLO+HRNet提取身体关键点

自动模式：

手动模式：
只用yolo提取框：
```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode yolo
```

标注框：
```bash
python3 apps/annotation/annot_track.py ${data} --sub <list of specified views>
```

提取关键点：



### mediapipe

#### 提取全身关键点

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode mp-holistic
```

#### 提取手部关键点

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --mode mp-handl
```

### 手部关键点提取
```bash
# Mediapipe
python3 scripts/preprocess/extract_hand.py /nas/users/huangdi/EasyMocap-1v1h/1 --mode mediapipe
# ResNet with bbox tracker
python3 scripts/preprocess/extract_hand.py /nas/users/huangdi/MixCap/2021-10-09-01/capture-0-2 --video
```

## 可视化
可视化提取出来的关键点，参考[标注器](#关键点标注)。

## 数据划分

选择数据集的部分帧，拷贝到新的目录：

```bash
python3 scripts/
```