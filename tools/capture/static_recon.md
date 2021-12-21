---
layout: default
title: 静止重建
parent: 相机标定
grand_parent: 实用工具
---

# 静止重建

## 相机标定

1. 使用colmap

2. 使用棋盘格确定地面位置与相机尺度

自动标注棋盘格：

```bash
# 数据指定到每一个sequence的目录下
data=/nas/dataset/soccer2/images/recon_2_4/VID_20211219_155655
python3 apps/calibration/detect_chessboard.py ${data} --out ${data}/output/calibration --pattern 9,6 --grid 0.1 --seq --max_step 20 --min_step 4
```

不出意外的话是可以自动检测到棋盘格的，但是为了稳妥期间，需要手动检查与补全棋盘格：

```bash
python3 apps/annotation/annot_calib.py ${data} --annot chessboard --mode chessboard
```

见[annotation](../annotation.md)部分

重建与生成尺度：

## 数据转换

运行脚本，将数据转换为多视角的EasyMocap格式

```bash

```

