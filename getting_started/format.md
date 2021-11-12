---
layout: default
title: 数据格式
parent: 快速开始
nav_order: 3
---

# 数据格式
{: .no_toc }

1. TOC
{:toc}
---

## 目录结构
数据需要按以下格式存储：
```bash
<seq>
├── intri.yml
├── extri.yml
└── videos
    ├── 000001.mp4
    ├── 000002.mp4
    ├── 00000...
    ├── 000008.mp4
    └── 000009.mp4
```
对于单目视频序列，如果没有相机参数，可不放置。

如果有图片，可直接存放图片。如果是以上视频，可使用脚本提取出图片：

```bash
python3 apps/preprocess/extract_image.py ${data}
```

提取后的数据目录需要如下所示：

```bash
<seq>
├── intri.yml
├── extri.yml
└── images
    ├── 1
    │   ├── 000000.jpg
    │   ├── 000001.jpg
    │   ├── 000002.jpg
    │   └── ...
    ├── 2
    │   ├── 000000.jpg
    │   ├── 000001.jpg
    │   ├── 000002.jpg
    │   └── ...
    ├── ...
    ├── ...
    ├── 8
    │   ├── 000000.jpg
    │   ├── 000001.jpg
    │   ├── 000002.jpg
    │   └── ...
    └── 9
        ├── 000000.jpg
        ├── 000001.jpg
        ├── 000002.jpg
        └── ...
```

请注意，如果有超过10个相机，并且如果使用数字命名的话，一定要给视频名称前面补0。

## 相机参数

For example, if the name of a video is `1.mp4`, then there must exist `K_1`, `dist_1` in `intri.yml`, and `R_1((3, 1), rotation vector of camera)`, `T_1(3, 1)` in `extri.yml`. The file format is following [OpenCV format](https://docs.opencv.org/master/dd/d74/tutorial_file_input_output_with_xml_yml.html).

相机参数的读写：参考`easymocap/mytools/camera_utils.py`=>`write_camera`, `read_camera`函数。

## 人体参数
## 输出结果

- keypoints3dfit: 表示拟合的SMPL的关键点
- smpl_keypoints: 表示SMPL自身定义的关键点位置


## 配置文件

本项目配置文件系统基于`yacs`库修改，主要增加的功能有：
1. 实现parents功能，可以使得配置文件进行继承
2. 实现`_parents_`，使配置文件中的每一个模块都可以继承自其他文件

## FAQ

> 1. 输出的结果不包含模型信息

确实

> 2. SMPLH, SMPLX的手部的参数在poses里面，容易混淆

确实

> 3. MANO的时候手部的参数也叫poses，容易混淆

确实
