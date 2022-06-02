---
layout: default
title: DesktopStage
parent: 相机标定
grand_parent: 实用工具
---

# DesktopStage工作流

## 数据采集

```bash
# 检查相机是否正常工作
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display
# 设置输出路径
root=desktop/210511
# 拍摄静止的棋盘格的
# 黑色的角落在左手
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 100 --out ${root}/ground1f
# 拍摄静止的背景
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 1 --out ${root}/background1f
# 拍摄棋盘格移动旋转的
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 900 --out ${root}/ba
# 拍摄测试数据1：测试左手
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 500 --out ${root}/test-handl
# 拍摄测试数据2：测试右手
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 500 --out ${root}/test-handr
# 拍摄测试数据3：测试身体
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 500 --out ${root}/test-body
# 拍摄测试数据4：测试身体+手
python3 apps/camera/realtime_display.py --cfg_cam config/camera/usb-logitech.yml --display --num 600 --out ${root}/test-half
```

## 相机标定

```bash
python3 apps/calibration/detect_chessboard.py ${root}/ground1f --out ${root}/ground1f/output --pattern 11,8 --grid 0.03
python3 apps/annotation/annot_calib.py ${root}/ground1f --annot chessboard --mode chessboard --pattern 11,8
python3 apps/calibration/detect_chessboard.py ${root}/ba --out ${root}/ba/output --pattern 11,8 --seq --grid 0.03 --max_step 32 --min_step 4
# 标定内参
python3 apps/calibration/calib_intri.py ${root}/ba --num 200 --share_intri
python3 apps/calibration/calib_extri.py ${root}/ground1f --intri ${root}/ba/output/intri.yml
# 检查相机参数
python3 apps/calibration/check_calib.py ${root}/ground1f --mode cube --out ${root}/ground1f --show --write --grid_step 0.21
python3 apps/calibration/check_calib.py ${root}/ground1f --mode match --out ${root}/ground1f --show --write --annot chessboard
```

## 检测人体关键点

```bash
python3 apps/preprocess/extract_keypoints.py ${root}/test-handl --mode mp-handl
cp ${root}/ground1f/*.yml ${root}/test
python3 apps/demo/mocap.py ${root}/test --mode halfh-3d --subs_vis 0
vlc ${root}/test/output-halfh-3d/smplmesh.mp4
```

## 实时测试

### 手部

```bash
# 开启可视化
python3 apps/vis/vis_server.py --cfg config/vis3d/o3d_scene_manol.yml --opts host 0.0.0.0 port 9999 block False
# 测试本地数据
# 1. 测试左手
python3 apps/fit/triangulate1p.py --cfg_data config/recon/mv1p.yml --cfg_exp config/recon/fit_manol.yml --vis3d localhost:9999 --opt_data args.camera ${root}/ground1f args.path ${root}/test-handl --no_write
# 2. TODO测试右手
python3 apps/fit/triangulate1p.py --cfg_data config/recon/mv1p.yml --cfg_exp config/recon/fit_manol.yml --vis3d localhost:9999 --opt_data args.camera ${root}/ground1f args.path ${root}/test-handr --no_write
# 3. 测试半身
python3 apps/fit/triangulate1p.py --cfg_data config/recon/mv1p.yml --cfg_exp config/recon/fit_halfbody.yml --vis3d localhost:9999 --opt_data args.camera ${root}/ground1f args.path ${root}/test-half --no_write

python3 apps/fit/triangulate1p.py --cfg_data config/recon/mv1p.yml --cfg_exp config/recon/fit_halfbody+hand.yml --vis3d localhost:9999 --opt_data args.camera ${root}/ground1f args.path ${root}/test-half --no_write
```


```bash
# 接收实时输入的关键点
python3 apps/fit/triangulate1p.py --cfg_data config/recon/socket_mv1p.yml --cfg_exp config/recon/fit_manol.yml --vis3d localhost:9999 --det2d 127.0.0.1:12345:associator_input --opt_data args.camera ${root}/ground1f
```

