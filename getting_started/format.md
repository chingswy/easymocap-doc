---
layout: default
title: 数据格式
parent: 快速开始
nav_order: 3
has_children: true
---

# 数据格式
{: .no_toc }

- 考虑通用性，EasyMocap的所有数据使用相同的格式
- 考虑文件可读性与可迁移性，数据基本使用`.json`格式存储，为了增加可读性，在输出到文件的时候需要进行format

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

## 2D Pose

For each image, we record its 2D pose in a `json` file. For an image at `root/images/1/000000.jpg`, the 2D pose willl store at `root/annots/1/000000.json`. The content of the annotation file is:

```bash
{
    "filename": "images/0/000000.jpg",
    "height": <the height of image>,
    "width": <the width of image>,
    "annots:[
        {
            'personID': 0, # ID of person
            'bbox': [l, t, r, b, conf],
            'keypoints': [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]],
            'area': <the area of bbox>
        },
        {
            'personID': 1, # ID of person
            'bbox': [l, t, r, b, conf],
            'keypoints': [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]],
            'area': <the area of bbox>
        }
    ]
}
```

The definition of the `keypoints` is `body25`. If you want to use other definitions, you should add it to `easymocap/dataset/config.py`

If you use hand and face, the annot is defined as:
```bash
{
    "personID": i,
    "bbox": [l, t, r, b, conf],
    "keypoints": [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]],
    "bbox_handl2d": [l, t, r, b, conf],
    "bbox_handr2d": [l, t, r, b, conf],
    "bbox_face2d": [l, t, r, b, conf],
    "handl2d": [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]],
    "handr2d": [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]],
    "face2d": [[x0, y0, c0], [x1, y1, c1], ..., [xn, yn, cn]]
}
```

## 3D Pose

```bash
[
    {
        'id': <id>, # the person ID
        'keypoints3d': [[x0, y0, z0, c0], [x1, y1, z0, c1], ..., [xn, yn, zn, cn]], # x,y,z is the 3D coordinates, c means the confidence of this joint. If the c=0, it means this joint is invisible.
    },
    {
        'id': <id>, # the person ID
        'keypoints3d': [[x0, y0, z0, c0], [x1, y1, z0, c1], ..., [xn, yn, zn, cn]], # x,y,z is the 3D coordinates, c means the confidence of this joint. If the c=0, it means this joint is invisible.
    }
]
```

The definition of the keypoints can be found in `easymocap/dataset/config.py`. We main use the following formats:
- body25: 25 keypoints of body
- bodyhand: 25 body + 21 left hand + 21 right hand
- bodyhandface: 25 body + 21 left hand + 21 right hand + 51 face keypoints

## SMPL参数

```bash
{
    "id": <id>,
    "Rh": <(1, 3)>,
    "Th": <(1, 3)>,
    "poses": <(1, 60)>,
    "expression": <(1, 10)>,
    "shapes": <(1, 10)>
}
```

SMPL骨架定义如图所示：

<div align="center">
    <img src="../images/define/SMPL.png" width="60%">
    <br>
    <sup>SMPL骨架定义<sup/>
</div>

各个关节的旋转视频如下所示：

MANO的各个关节的旋转视频如图所示：

## 输出结果

- keypoints3dfit: 表示拟合的SMPL的关键点
- smpl_keypoints: 表示SMPL自身定义的关键点位置

## 导出到bvh

TODO

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
