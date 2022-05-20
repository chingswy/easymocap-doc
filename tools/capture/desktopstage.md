---
layout: default
title: DesktopStage
parent: 相机标定
grand_parent: 实用工具
---

# DesktopStage工作流

## 相机标定

```bash
# 检查相机是否正常工作
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display
# 设置输出路径
root=desktop/210511
# 拍摄静止的棋盘格的
data=${root}/ground
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 100 --out ${data}
# 拍摄棋盘格移动旋转的
data=${root}/ba
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 900 --out ${data}
# 拍摄测试数据
data=${root}/test
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 900 --out ${data}
```

运行标定代码

```bash
python3 scripts/preprocess/copy_dataset.py ${root}/ground ${root}/ground1f --start 0 --end 1
python3 apps/calibration/detect_chessboard.py ${root}/ground1f --out ${root}/ground1f/output --pattern 11,8 --grid 0.03
python3 apps/annotation/annot_calib.py ${root}/ground1f --annot chessboard --mode chessboard --pattern 11,8
python3 apps/calibration/detect_chessboard.py ${root}/ba --out ${root}/output --pattern 11,8 --seq --grid 0.03 --max_step 32 --min_step 4
# 标定内参
python3 apps/calibration/calib_intri.py ${root}/ba --num 200 --share_intri
python3 apps/calibration/calib_extri.py ${root}/ground1f --intri ${root}/ba/output/intri.yml
# 检查相机参数
python3 apps/calibration/check_calib.py ${root}/ground1f --mode cube --out ${root}/ground1f --show --grid_step 0.1
python3 apps/calibration/check_calib.py ${root}/ground1f --mode match --out ${root}/ground1f --show --grid_step 0.1
```

使用人体关键点检查
```bash
python3 apps/preprocess/extract_keypoints.py ${root}/test --mode mp-holistic
cp ${root}/ground1f/*.yml ${root}/test
python3 apps/demo/mocap.py ${root}/test --mode halfh-3d --subs_vis 0
vlc ${root}/test/output-halfh-3d/smplmesh.mp4
```