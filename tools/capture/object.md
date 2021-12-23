---
layout: default
title: 物体重建
parent: 相机标定
grand_parent: 实用工具
---

# 物体重建

## 数据标注


## 训练YOLO

运行脚本，将数据转换为多视角的EasyMocap格式

```bash
python3 scripts/postprocess/cvt_yolo.py ${data} /nas/dataset/human/anyball --annots football
```

## 物体检测

依赖：

```bash
opencv-contrib-python==4.5.4.58
opencv-python==4.5.3.56
```

对于完全没有标注的数据，需要首先进行半自动标注。

```bash
python3 apps/annotation/annot_track.py ${data} --annot basketball --max_person 1 --step 5
```

使用方式：
- 使用鼠标标注一个物体的框，按`n`键新建。
- 按`g`键自动跟踪，跟踪过程中如果发现错误的框，按`q`停止自动跟踪。

对于标注好的数据，使用脚本将其转换为COCO格式

```bash
python3 scripts/postprocess/cvt_yolo.py ${data} <output> --annots ball
```

使用yolo进行训练：
> YOLO路径：/home/shuaiqing/Project/yolov5

```bash
python3 train.py --img 640 --batch 16 --epochs 100 --data data/anyball.yaml --weights models/yolov5m.pt
```

数据配置为`data/anyball.yaml`，里面内容为

```python
path: /nas/dataset/human/anyball  # dataset root dir
train: images  # train images (relative to 'path') 128 images
val: images  # val images (relative to 'path') 128 images
test:  # test images (optional)

# Classes
nc: 5  # number of classes
names: ['football', 'basketball', 'ballr', 'ballg', 'ballb']
```

训练之后，看看最后的AP，如果AP小于90，那么说明训练没有收敛，需要增加迭代次数。

确认训练完成后，选择最佳的checkpoint, 可以使用yolo的检测脚本进行预览

```bash
python3 detect.py --weights <path_to_best_weights> --source <path_to_test_image>
python3 detect.py --weights runs/train/exp5/weights/best.pt --source /nas/dataset/soccer/gopro-soccer-211204/h5-seq0-sync/seq0/images/1
```

预览基本没问题之后，使用EasyMocap的脚本，提取相应数据的bbox，存储到`${data}/object2d`目录下：

```bash
python3 apps/preprocess/extract_object2d.py ${data} --ckpt <path_to_best_checkpoints> --out object2d
python3 apps/preprocess/extract_object2d.py ${data} --ckpt ~/Project/yolov5/runs/train/exp5/weights/best.pt --out object2d
```

本地2D可视化
```bash
python3 apps/annotation/annot_track.py ${data} --annot object2d --max_person 1 --step 5
```

## 多视角重建

对于多视角数据，提取出3D关键点的位置：

```bash
python3 apps/realtime/mvmo.py ${data} --ids 'football' --annot ball
```


3D重投影可视化：

```bash

```

本地3D可视化球的位置：
```bash
python3 apps/vis3d/vis_server.py --out ${data}/object3d
```

{ .note }
需要增加时序的补全、去除、平滑、滤波