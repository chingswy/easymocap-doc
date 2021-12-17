---
layout: default
title: 足球数据采集
parent: 相机标定
grand_parent: 实用工具
---

# 足球数据采集

## 采集事项

1. 运动不要太快
2. 不要特别近距离的抢球，会导致人挤在一起
3. 动作 3v3，两个人守门，

这部分面向户外场景使用三脚架搭建的GoPro拍摄。

准备工具：
- 场记板
- 标定板
- 三脚架
- 卷尺
- iPad: 用于拍摄与重建场景
- 黑胶带: 用于在没有标记的地方贴标记，或用于遮住一些会透露身份信息的标志
- 剪刀: 剪胶带

拍摄过程：

1. 确定采集中心。搭建相机支架，成八角星分布；检查所有相机都使用了同样的配置，推荐配置为1080,60fps,线性；调整相机高度与角度，角度尽量保持平视，中心区域站一个人，每个相机都需要看到中心区域的人。
2. 距离较近时，使用同步模块控制相机同时开始拍摄。距离较远时，手动开启。
3. 使用场记板，在所有视角都可见的情况迅速打板
4. 人体面朝第1个相机，摆出Tpose，手指张开向前。保持静止三秒。面朝与其垂直的相机，摆出Tpose，静止三秒。
5. 拍摄人员进场，进行动作。
6. 使用场记板，在所有视角都可见的情况打板。
7. 关掉相机。

注意事项：
1. 每次拍摄记得拍摄相机摆放图
2. 如果在室外，场记板需要面对影子的方向

## 相机设置

使用8个GoPro相机与三个三脚架。

<div align="center">
    <img src="./football/football.jpg" width="60%">
    <br><sup>相机位置设置</sup>
</div>

### 后处理

0. 创建文件夹`root`
1. 使用读卡器，将每个相机上的同样数量的视频放置到`root/i`中
2. 确认每个相机的文件夹下对应的视频数量一致、顺序一致
3. 使用脚本分别拷贝到不同目录，输入每段序列的名称
```bash
data=/path/to/data
python3 scripts/preprocess/copy_gopro.py ${data} --names seq0 seq1 seq2 seqn
```
4. 使用`ffmpeg`解压图片与音频，借助脚本:
```bash
data=/path/to/sequence
python3 apps/preprocess/extract_image.py ${data}
```

5. (可选)使用音频粗糙同步，获得声音最大值的区域
6. 使用脚本肉眼进一步同步，同步帧记录在`${data}/sync.json`里面
7. 对每段数据，截取：空白帧，棋盘格帧，人体标定帧，数据帧

```bash
# 选择前第400帧为地面帧，放置了标定板的
python3 apps/annotation/copy_mv_sync.py ${data} --out ${data}/../seq0-sync/ground --frames -400
# 选择 250,400,1000帧用于标定
python3 scripts/preprocess/copy_dataset.py ${data} ${data}/h5-seq0-sync/calibhuman --frames 250 400 1000
```


### 女足数据采集

|||
|----|----|
|场景|模拟足球场景|
|设备|8个GoPro，360度环绕，单反，手机|
|内容|足球遛猴动作(约1min)|
|   |足球3v3攻防(约1min)|
|   |足球点球模拟(约20s)|
|   |单人带球,颠球(约1min)|
|   |多人带球传球(约20s)|
|   |单人自转视频|
|   |单人静止扫描视频|
|注意事项|运动员身穿球衣，不要显示学校信息|

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
python3 train.py --img 640 --batch 16 --epochs 10 --data data/anyball.yaml --weights models/yolov5m.p
```

数据配置为`data/anyball.yaml`，里面内容为

```python
path: ../datasets/anyball  # dataset root dir
train: images  # train images (relative to 'path') 128 images
val: images  # val images (relative to 'path') 128 images
test:  # test images (optional)

# Classes
nc: 5  # number of classes
names: ['football', 'basketball', 'ballr', 'ballg', 'ballb']
```

训练10个epoch后，可以使用yolo的检测脚本进行预览

```bash
python3 detect.py --weights runs/train/exp5/weights/last.pt --source /nas/dataset/soccer/gopro-soccer-211204/h5-seq0-sync/seq0/images/1
```

预览基本没问题之后，使用EasyMocap的脚本，提取相应数据的bbox，存储到`${data}/object2d`目录下：

```bash
python3 apps/preprocess/extract_object2d.py ${data} --ckpt ~/Project/yolov5/runs/train/exp5/weights/last.pt --out object2d
```

对于多视角数据，提取出3D关键点的位置：

```bash
python3 apps/realtime/mvmo.py ${data} --ids 'football' --annot ball
```

本地2D可视化
```bash
python3 apps/annotation/annot_track.py ${data} --annot object3d --max_person 1 --step 5
```

本地3D可视化球的位置：
```bash
python3 apps/vis3d/vis_server.py --out ${data}/object3d
```

{ .note }
需要增加时序的补全、去除、平滑、滤波

## 重建

