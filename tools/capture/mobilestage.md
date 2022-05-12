---
layout: default
title: MobileStage
parent: 相机标定
grand_parent: 实用工具
---

# MobileStage工作流

## 数据拷贝

在目录`/nas/dataset/XiaoMiMocap`下建立当前日期的文件夹，将采集目录拷贝或软链接到日期目录。例如

```bash
<20220511>
├── ba
│   └── videos -> ../../../xiaomi_test/20220511/160231/videos
├── ground
│   └── videos -> ../../../xiaomi_test/20220511/155834/videos
└── test
    └── videos -> ../../../xiaomi_test/20220511/160429/videos
```

## 提取图片

```bash
root=/path/to/root
for seq in $(ls ${root});do python3 apps/preprocess/extract_image.py ${root}/${seq} --transpose 2;done
```

查看采集的图片同步情况

```bash
python3 apps/annotation/annot_mv_sync.py ${root}/ba
```

拷贝地面的1帧

```bash
python3 scripts/preprocess/copy_dataset.py ${root}/ground ${root}/ground1f --start 0 --end 1
```



## 相机标定

```bash
python3 apps/calibration/detect_chessboard.py ${root}/ground1f --out ${root}/ground1f/output --pattern 11,8 --grid 0.06
python3 apps/annotation/annot_calib.py ${root}/ground1f --annot chessboard --mode chessboard --pattern 11,8
```

主要操作:
- `shift+c`：清除所有点
- `q`: 退出，在终端选择是否保存
- `Q`: 退出不保存
- `鼠标选中框+e`：对选中区域重新检测棋盘格

这一步需要清除掉所有的错误的检测；如果错误的太多，那么后续步骤就不会使用

（可选）使用colmap标定相机:

```bash
python3 apps/calibration/calib_by_colmap.py ${root} ${out} --seqs ground1f --step 100 --no_camera
```


## 附录

### 1. ffmpeg旋转操作

|-vf||
|1|顺时针旋转画面90度|
|2|逆时针旋转画面90度|
|3|顺时针旋转画面90度再水平翻转|
|0|逆时针旋转画面90度水平翻转|
|hflip|水平翻转视频画面|
|vflip|垂直翻转视频画面|
