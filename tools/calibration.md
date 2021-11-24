---
layout: default
title: 相机标定
parent: 实用工具
nav_order: 1
---

# Camera Calibration

{: .no_toc }

这部分文档包含相机标定的内容，以及多个场景下的相机标定。

1. TOC
{:toc}
---

Before reading this document, you should read the OpenCV-Python Tutorials of [Camera Calibration](https://docs.opencv.org/master/dc/dbb/tutorial_py_calibration.html) carefully.

## Some Tips
1. Use a chessboard as big as possible.
2. Use a chessboard as rigid as possible.
3. You must keep the same resolution during all the steps.

## 0. Prepare your chessboard

下载OpenCV官网上的棋盘格，或自己生成一个，去附近的文印店，告诉老板需要打到一个大的KT板上。并告诉老板需要一格有多长。

## 1. 相机内参标定

### 1.1 获取图像

文件目录组织保持树状结构，所有的图片都保存在`images`目录下

```bash
<seq>
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
### 1.2 检测棋盘格

### 1.3 内参标定

## 2. 相机外参标定

### 2.1 使用棋盘格

### 2.2 使用标志点

### 2.3 使用人体关键点

## 3. 检查

### 可视化检查


## 1. Record videos
Usually, we need to record two sets of images, one for intrinsic parameters and one for extrinsic parameters.

First, you should record images with your chessboard for each camera separately. The images of each camera should be placed into the `<intri_data>/images` directory. The following code will take the file name as the name of each camera.

```bash
<intri_data>
└── videos
    ├── 01.mp4
    ├── 02.mp4
    ├── ...
    └── xx.mp4
```

In this tutorial, we use our sample datasets as an example. In that dataset, the intri data is just like the picture below.

<div align="center">
    <img src="assets/intri_sample.png" width="60%">
    <br>
    <sup>Example Intrinsic Dataset<sup/>
</div>


For the extrinsic parameters, you should place the chessboard pattern where it will be visible to all the cameras (on the floor for example) and then take a picture or a short video on all of the cameras.

```bash
<extri_data>
└── videos
    ├── 01.mp4
    ├── 02.mp4
    ├── ...
    └── xx.mp4
```

The sample extri data is like the picture below.


<div align="center">
    <img src="assets/extri_sample.png" width="60%">
    <br>
    <sup>Example Extrinsic Dataset<sup/>
</div>


## 2. Detect the chessboard
For both intrinsic parameters and extrinsic parameters, we need detect the corners of the chessboard. So in this step, we first extract images from videos and second detect and write the corners.
```bash
# detect chessboard
python3 apps/calibration/detect_chessboard.py ${data} --out ${data}/output/calibration --pattern 9,6 --grid 0.1 --seq
```
The results will be saved in `${data}/chessboard`, the visualization will be saved in `${data}/output/calibration`.

To specify your chessboard, add the option `--pattern`, `--grid`.

Repeat this step for `<intri_data>` and `<extri_data>`.

After this step, you should get the results like the pictures below.

<div align="center">
    <img src="assets/extri_chessboard.jpg" width="60%">
    <br>
    <sup>Result of Detecting Extrinsic Dataset<sup/>
</div>


<div align="center">
    <img src="assets/intri_chessboard.jpg" width="60%">
    <br>
    <sup>Result of Detecting Intrinsic Dataset<sup/>
</div>

## 2.5 Finetune the Chessboard Detection Result

It is vital for calibration to detect the keypoints of chessboard correctly. **Thus we highly recommend you to carefully inspect the visualization result in ${data}/output.** If you find some detection results are wrong, we provide you a tool to make some modifications to them.

```bash
python apps/annotation/annot_calib.py $data --mode chessboard --pattern 9,6 --annot chessboard
```

After running the script above, a OpenCV GUI prompt will show, like below:

<div align="center">
    <img src="assets/ft1.png" width="60%">
    <br>
    <sup>Calibration Annotation Toolkit GUI Interface<sup/>
</div>


> This tool is component of our awesome annotation toolkits, so some key mapping is similar. To learn more about our annotation tools, please check [the document](../annotation/Readme.md).

At the same time, you can see that the CLI presents some auxilary information.


<div align="center">
    <img src="assets/ft2.png" width="60%">
    <br>
    <sup>CLI Prompt of the Annotation Tool<sup/>
</div>


You can learn from the CLI prompt to know the information and which point you are labeling.

In the GUI, the current edited corner is highlighted by a red circle. If you want to make some modification, use mouse to click the correct place, and then a white anchor "+" is presented there.


<div align="center">
    <img src="assets/ft3.png" width="60%">
    <br>
    <sup>Use mouse to specify the correct position<sup/>
</div>

If you think the newly specified coordinate(marked as white anchor) should be the correct position for this corner, rather than old one, press `Space` to confirm. Then the corner position will be changed. 

<div align="center">
    <img src="assets/ft4.png" width="60%">
    <br>
    <sup>The result after modifing the position of point<sup/>
</div>

After finish modifying this point, press `Space` to move on to next point.


<div align="center">
    <img src="assets/ft5.png" width="60%">
    <br>
    <sup>Press Space to move on to next point<sup/>
</div>

> Currently we only support move to next point. If you want to move to previous point, please `Space` for many times until it back to start.

If you're satisfied with this frame, you can press `D` move on to next frame.


<div align="center">
    <img src="assets/ft6.png" width="60%">
    <br>
    <sup>Press D to move on to next frame<sup/>
</div>


If you press `A`, you can move back to previous frame.

After finish annotating every frames, press `q` to quit.

<div align="center">
    <img src="assets/ft7.png" width="40%">
    <br>
    <sup>CLI prompt to save the result. Press Y to save and N to discard<sup/>
</div>

Then you can choose whether to save this annotation.

> If your data is on remote server, then the OpenCV GUI may be too slow to operate if you directly run the script via ssh X forwarding. We recommend you use `sshfs` to mount the remote data directory and locally run this script.


## 3. Intrinsic Parameter Calibration

After extracting chessboard, it is available to calibrate the intrinsic parameter.

```bash
python3 apps/calibration/calib_intri.py ${data} --num 200
```

After the script finishes, you'll get `intri.yml` under `${data}/output`.

> This step may take a long time, so please be patient. :-)

## 4. Extrinsic Parameter Calibration


Then you can calibrate the extrinsic parameter.

```
python3 apps/calibration/calib_extri.py ${extri} --intri ${intri}/output/intri.yml
```

After the script finished, you'll get `extri.yml` under `${intri}/output`.

## 5. (Optional)Bundle Adjustment

Coming soon

## 6. Check the calibration

To check whether your camera parameter is correct, we provide several approaches to make verification.

1. **Check the calibration results with chessboard:**
```bash
python3 apps/calibration/check_calib.py ${extri} --out ${intri}/output --vis --show
```

A window will be shown for checking.

<div align="center">
    <img src="assets/vis_check.png" width="60%">
    <br>
    <sup>Use chessboard to check results<sup/>
</div>

**Check the results with a cube.**
```bash
python3 apps/calibration/check_calib.py ${extri} --out ${extri}/output --cube
```

You'll get results in `$data/output/cube`. 


<div align="center">
    <img src="assets/cube.jpg" width="60%">
    <br>
    <sup>Use cube to check results<sup/>
</div>
