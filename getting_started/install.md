---
layout: default
title: 安装
parent: 快速开始
nav_order: 1
---

# 安装
{: .no_toc }

1. TOC
{:toc}
---

## 基础安装
### 1. PyTorch安装
PyTorch版本需要与cuda保持一致，用户可自行安装。torchvision版本需要与pytorch版本一致。

以下为测试过的版本：
- torch==1.4.0+cu100 torchvision==0.5.0
- torch==1.7.0+cu110 torchvision==0.8.0

### 2. pyrender安装

有显示器的情况，直接使用pip进行安装

```bash
python3 -m pip install pyrender
```

无显示器的情况，需要使用`osmesa`或`egl`。请参考[getting-pyrender-working-with-osmesa](https://pyrender.readthedocs.io/en/latest/install/index.html#getting-pyrender-working-with-osmesa)

主要包含以下步骤
1. 安装osmesa
在实验室服务器上，非管理员用户请联系管理员安装，一般的服务器都安装了

2. 安装修改过的PyOpenGL
```bash
git clone https://github.com/mmatl/pyopengl.git
pip install ./pyopengl
```

3. 在渲染前指定环境变量
```bash
export PYOPENGL_PLATFORM=osmesa
python3 xxxxx
```

### 3. 其他依赖库安装

使用requirements.txt进行安装
```bash
python3 -m pip install -r requirements.txt
```

### 4. SMPL模型下载

This step is the same as [smplx](https://github.com/vchoutas/smplx#model-loading).

To download the *SMPL* model go to [this](http://smpl.is.tue.mpg.de) (male and female models, version 1.0.0, 10 shape PCs) and [this](http://smplify.is.tue.mpg.de) (gender neutral model) project website and register to get access to the downloads section. 

To download the *SMPL+H* model go to [this project website](http://mano.is.tue.mpg.de) and register to get access to the downloads section. 

To download the *SMPL-X* model go to [this project website](https://smpl-x.is.tue.mpg.de) and register to get access to the downloads section. 

**Place them as following:**

```bash
data
└── smplx
    ├── J_regressor_body25.npy
    ├── J_regressor_body25_smplh.txt
    ├── J_regressor_body25_smplx.txt
    ├── J_regressor_mano_LEFT.txt
    ├── J_regressor_mano_RIGHT.txt
    ├── smpl
    │   ├── SMPL_FEMALE.pkl
    │   ├── SMPL_MALE.pkl
    │   └── SMPL_NEUTRAL.pkl
    ├── smplh
    │   ├── MANO_LEFT.pkl
    │   ├── MANO_RIGHT.pkl
    │   ├── SMPLH_female.pkl
    │   └── SMPLH_male.pkl
    └── smplx
        ├── SMPLX_FEMALE.pkl
        ├── SMPLX_MALE.pkl
        └── SMPLX_NEUTRAL.pkl
```

### 5. SPIN模型下载

```bash
# checkpoint
mkdir -p data/models
wget -c http://visiondata.cis.upenn.edu/spin/model_checkpoint.pt
mv model_checkpoint.pt data/models/spin_checkpoint.pt
# smpl_mean_params
mkdir -p data/spin &&tar xvf data.tar.gz --directory data/spin
cp data/spin/data/smpl_mean_params.npz data/models/
```

### 6. (Optional) OpenPose安装
TODO

### 7. (Optional) OpenGL安装

Ubuntu使用sudo安装:
```bash
sudo apt-get install build-essential libgl1-mesa-dev
sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
sudo apt-get install libglfw3-dev libglfw3
```

### 8. (Optional) PyTorch3d
使用版本v0.4.0
1. conda安装
2. 源码编译

### 9. (Optional) Mediapipe安装
****

## 多视角多人安装
## 多视角单人安装
## 离线优化安装
