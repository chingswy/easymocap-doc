---
layout: default
title: 标注工具
parent: 实用工具
nav_order: 2
---

# 标注工具
{: .no_toc }

这部分包含了多种数据标注的工具，基于OpenCV实现。

如果只使用标注部分，安装比较简单，主要依赖于OpenCV库，可以直接在现有的环境上安装
```bash
python3 setup.py develop
```

打开方式：
1. Linux下，使用`ssh -X`登录到服务器，在服务器运行代码。即可使用X server可视化
2. Linux下，使用`sshfs`将远程目录挂载到本地，在本地运行代码，速度会比1更快
3. Windows下使用VNC，在服务器开启虚拟桌面，在本地连接服务器的虚拟桌面，会比较卡

1. TOC
{:toc}
---

## 总体需求

- [ ] 实现一个bbox，然后进行放大的需求

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

## 物体标注

```bash
python3 apps/annotation/annot_track.py ${data} --annot basketball --max_person 1 --step 5
```

使用方式：
- 使用鼠标标注一个物体的框，按`n`键新建。
- 按`g`键自动跟踪，跟踪过程中如果发现错误的框，按`q`停止自动跟踪。

{: .note }
目前还不支持多个物体的标注

{: .note }
需要支持物体类别的标注

重建三维轨迹：
{: .note }
实现多个同类型的物体一起重建的代码；是否可以考虑多个物体与多人一起重建，怎么鲁棒的完成这件事情； 如何把物体的关键点结合进来；

{: .note }
是否可以实现一个通用的多人、多手、多脸、多物体的重建方案。这个方案不用管输入是什么类型的。





## 标注背景的mask



## 多视角同步

需求：
- 多个窗口一起移动
- 单个窗口选中之后可以单独移动
- 记录关键帧
- 保存关键帧

1. 标注多视角的同步（使用场记板）

```bash
python3 apps/annotation/annot_mv_sync.py ${data}
```

- 鼠标点击选中视角，`x`取消选中
- `wasd`：选中视角时，用于切换当前视角的帧；不选中视角时，同步切换所有视角的帧
- `r`：记录当前的关键帧
- `f`：快速切换到同步的帧

2. 导出数据

## 多视角匹配点标注

需求：
- [x] 不同视角之间切换
- [x] 增删一个匹配点
- [x] 可视化匹配的点
- [ ] 按下功能键标一个点就BA一下

1. 新增匹配点序列

在不指定的情况下，5个点为一组
```
python3 apps/calibration/create_marker.py ${data} --N 100
```

2. 进行标注


## 多视角匹配点同时标注

需求：
- [x] 同时可视化所有视角
- [x] 增加多个匹配点
- [ ] 删除当前的匹配点
- [ ] 可视化第i个匹配点
- [ ] 实时重建相机

## 多视角标注

需求：
- 多个窗口一起移动
- 选中某个视角里的某个人
