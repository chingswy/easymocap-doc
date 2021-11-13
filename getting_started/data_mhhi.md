<!--
 * @Date: 2021-11-13 14:54:41
 * @Author: Qing Shuai
 * @LastEditors: Qing Shuai
 * @LastEditTime: 2021-11-13 15:46:14
 * @FilePath: /easymocap-doc/getting_started/data_mhhi.md
-->
---
layout: default
title: MHHI
parent: 数据格式
nav_order: 3
---

# MHHI dataset

使用提供的脚本进行转换：

```bash
python3 python3 scripts/dataset/pre_mhhi.py ${data} ${out} --image --camera
```

检查相机参数：

```bash
python3 apps/calibration/check_calib.py ${out} --out ${out} --mode cube --show --ext .png
```

## 关键点检测

参考[预处理](./preprocess.md)，默认使用OpenPose：

```bash
python3 apps/preprocess/extract_keypoints.py ${data} --openpose ${openpose} --ext .png
```

## 检查关键点检测：

参考[数据标注](../tools/annotation.md)

```bash
python3 apps/annotation/annot_keypoints.py ${data} --ext .png
```

## 多视角重建

```bash
python3 apps/demo/mvmp_realtime.py ${data} --cfg config/exp/mvmp_1920x1080.yml --vis_repro --out ${data}/output --cfg_opts width 1296 height 972 --undis
```


