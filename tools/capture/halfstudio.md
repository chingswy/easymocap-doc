---
layout: default
title: 半圈场景设置
parent: 相机标定
grand_parent: 实用工具
---

# 半身动捕工作环境

## 相机设置

在六个相机的情况，推荐使用在中间位置高低放置两个，两侧每边两个相机。

<div align="center">
    <img src="./halfstudio/halfstudio.jpg" width="60%">
    <br><sup>相机位置设置</sup>
</div>

## 一键运行脚本

```bash
bash ./apps/calibration/
```

脚本的内容为:
```bash
# set the running path
root=${1}
intri=${root}/intri
ground=${root}/ground
ba=${root}/ba

print(){
    m=$1
    text=$2
    if [ ${m} == "info" ]; then
        echo -e "\033[33m${text}\033[0m"
    elif [ ${m} == "error" ]; then
        echo -e "\033[41m${text}\033[0m"
    else
        echo "Please set the mode"
    fi
}


for data in ${intri} ${ground} ${ba};do
print info "[calib] detect chessboard: ${data}"
python3 apps/calibration/detect_chessboard.py ${data} --out ${data}/output/calibration --pattern 9,6 --seq --grid 0.1
done

```

## 相机内参标定

```bash
<intri>
└── images
    ├── 2CDDA343DD1F
    ├── 2CDDA343DD20
    ├── 2CDDA343DD21
    ├── 2CDDA343DD22
    ├── 2CDDA343DD24
    └── 2CDDA3441EB0
```
