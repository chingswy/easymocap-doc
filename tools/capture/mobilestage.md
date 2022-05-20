---
layout: default
title: MobileStage
parent: 相机标定
grand_parent: 实用工具
---

# MobileStage工作流

## 数据采集

采集一个场景时，按以下步骤核对：
- [ ] 记录当前时间、使用相机数目、ISO 
- [ ] 采集空白背景
- [ ] 采集标定板放在地上
- [ ] 使用一个额外的手机，采集场景
- [ ] 采集运动数据
  - [ ] 开启手机
  - [ ] 打板
  - [ ] 保持静止一帧
  - [ ] 运动
  - [ ] 结束时保持静止
  - [ ] 打板
  - [ ] 结束采集

## 数据拷贝

在目录`/nas/dataset/XiaoMiMocap`下建立当前日期的文件夹，将采集目录的videos目录拷贝或软链接到日期目录。例如

```bash
<20220511>
├── ba
│   └── videos -> ../../../xiaomi_test/20220511/160231/videos
├── background
├── ground
│   └── videos -> ../../../xiaomi_test/20220511/155834/videos
└── test
    └── videos -> ../../../xiaomi_test/20220511/160429/videos
```

注意，不要直接把数据文件夹链接过来，正确的操作为

```bash
mkdir -p ba/videos && cd ba/videos
ln /nas/dataset/xiaomi_test/<data>/<seq>/videos/* ./
```

## 提取图片

手机充电口朝右的时候拍摄的视频，使用`--transpose 2`参数将视频旋转正确。

```bash
root=/path/to/root
for seq in $(ls ${root});do python3 apps/preprocess/extract_image.py ${root}/${seq} --transpose 2;done
python3 scripts/preprocess/copy_dataset.py ${root}/ground ${root}/ground1f --start 0 --end 1
python3 scripts/preprocess/copy_dataset.py ${root}/background ${root}/background1f --start 0 --end 1
```

查看采集的图片同步情况

```bash
python3 apps/annotation/annot_mv_sync.py ${root}/ba
```


## colmap相机标定流程

自动标定流程

```bash
# 自动检测
python3 apps/calibration/detect_chessboard.py ${root}/ground1f --out ${root}/ground1f/output --pattern 11,8 --grid 0.06
# 确认
python3 apps/annotation/annot_calib.py ${root}/ground1f --annot chessboard --mode chessboard --pattern 11,8
```

对于一个大一点的户外场景，这个办法通常不会奏效，需要手动标定棋盘格位置

```bash
colmap=<path/to/your/colmap>
# 使用colmap标定，这一步需要dense重建，可能比较久
python3 apps/calibration/calib_by_colmap.py ${root}/background1f ${out} --no_camera --share_camera --colmap ${colmap}
# 检查colmap的输出
$colmap gui --database_path ${out}/background1f_000000/database.db --image_path ${out}/background1f_000000/images
# 创建一个空的角点坐标
python3 apps/calibration/create_marker.py ${root}/ground1f --grid 0.66 0.48 --corner
# 标注角点
python3 apps/annotation/annot_calib.py ${root}/ground1f --annot chessboard --mode chessboard --pattern 2,2
# 对齐colmap的输出
python3 apps/calibration/align_colmap_ground.py ${out}/background1f_000000/sparse/0 ${out}/ --plane_by_chessboard ${root}/ground1f
# 检查输出
python3 apps/calibration/check_calib.py ${root}/ground1f --mode cube --out ${out} --show --grid_step 0.66
```

## 相机标定

主要操作:
- `shift+c`：清除所有点
- `q`: 退出，在终端选择是否保存
- `Q`: 退出不保存
- `鼠标选中框+e`：对选中区域重新检测棋盘格

这一步需要清除掉所有的错误的检测；如果错误的太多，那么后续步骤就不会使用

## 重建

```bash
cp ${out}/*.yml ${data}

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
