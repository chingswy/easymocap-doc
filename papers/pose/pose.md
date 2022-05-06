---
layout: default
title: Pose
parent: 相关论文
mathjax: true
has_children: true
---

# Pose Estimation

## 2D Pose, tracking

- [DanceTrack: Multi-Object Tracking in Uniform Appearance and Diverse Motion](https://arxiv.org/pdf/2111.14690.pdf), [data](https://github.com/DanceTrack/DanceTrack)


## 3D Pose

- [Camera Distortion-aware 3D Human Pose Estimation in Videowith Optimization-based Meta-Learning](https://arxiv.org/pdf/2111.15056.pdf)
- [ElePose: Unsupervised 3D Human Pose Estimation by Predicting Camera
Elevation and Learning Normalizing Flows on 2D Poses](https://arxiv.org/pdf/2112.07088.pdf)

## SMPL

- [Shape-aware Multi-Person Pose Estimation from Multi-View Images](https://openaccess.thecvf.com/content/ICCV2021/papers/Dong_Shape-Aware_Multi-Person_Pose_Estimation_From_Multi-View_Images_ICCV_2021_paper.pdf)
- [CVPR2022,Physics-aware Real-time Human Motion Tracking from Sparse Inertial Sensors](https://xinyu-yi.github.io/PIP/): 从inertial传感器获得姿态
- [CVPR2022, Capturing Humans in Motion: Temporal-Attentive 3D Human Pose and Shape Estimation from Monocular Video](https://mps-net.github.io/MPS-Net/)
- [HSC4D: Human-centered 4D Scene Capture in Large-scale Indoor-outdoor Space Using Wearable IMUs and LiDAR](https://arxiv.org/pdf/2203.09215.pdf)

## Performance Capture
- [Human Performance Capture from Monocular Video in the Wild](https://arxiv.org/pdf/2111.14672.pdf)
- [High-Fidelity Human Avatars from a Single RGB Camera](http://cic.tju.edu.cn/faculty/likun/projects/HF-Avatar/index.html)

## Hand Pose

- [Local and Global Point Cloud Reconstruction for 3D Hand Pose Estimation](https://arxiv.org/pdf/2112.06389.pdf)
- [Semi-Supervised 3D Hand Shape and PoseEstimation with Label Propagation](https://arxiv.org/pdf/2111.15199.pdf)
- [Constraining Dense Hand Surface Tracking with Elasticity](https://scontent-hkt1-1.xx.fbcdn.net/v/t39.8562-6/10000000_1117938378951628_2483836343619479338_n.pdf?_nc_cat=108&ccb=1-5&_nc_sid=ad8a9d&_nc_ohc=zTK2O7WcWn8AX_hHyYC&_nc_ht=scontent-hkt1-1.xx&oh=00_AT8_pFN8rAP34x6Z8k6w66SMJzJwx9ssqwh-FQIyylcC6Q&oe=62136BFB)
- [HybridCap: Inertia-aid Monocular Capture of Challenging Human Motions](https://arxiv.org/pdf/2203.09287.pdf)
- [CVPR22: HandOccNet: Occlusion-Robust 3D Hand Mesh Estimation Network](https://arxiv.org/pdf/2203.14564.pdf)
- 
### dataset
- [InterHand2.6M](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123650545.pdf)
- [Capturing Hands in Action using Discriminative Salient Points and Physics Simulation](https://arxiv.org/pdf/1506.02178.pdf)

## Human-Object

### 2D

- [Improving Human-Object Interaction Detection via Phrase Learning and Label Composition](https://arxiv.org/pdf/2112.07383.pdf): HOI detection
- [Decoupling Object Detection from Human-Object Interaction Recognition](https://arxiv.org/pdf/2112.06392.pdf)

### 3D


## Garment

- [Robust 3D Garment Digitization from Monocular 2D Images for 3D VirtualTry-On Systems](https://arxiv.org/pdf/2111.15140.pdf)
- [ICON: Implicit Clothed humans Obtained from Normals]()

## Body Representation

- [HVH: Learning a Hybrid Neural Volumetric Representation for Dynamic Hair Performance Capture](https://arxiv.org/pdf/2112.06904.pdf)
- [Learning to Fit Morphable Models](https://arxiv.org/pdf/2111.14824.pdf)
- [LatentHuman: Shape-and-Pose Disentangled Latent Representationfor Human Bodies](https://arxiv.org/pdf/2111.15113.pdf)
- [Learning Body-Aware 3D Shape Generative Models](https://arxiv.org/pdf/2112.07022.pdf)

## Implict head/body model

[I M Avatar: Implicit Morphable Head Avatars from Videos](https://arxiv.org/pdf/2112.07471.pdf)


## Performance Capture

- [Siggraph18: MonoPerfCap: Human Performance Capture from Monocular Video](https://vcai.mpi-inf.mpg.de/projects/wxu/MonoPerfCap/content/monoperfcap.pdf); [home](https://vcai.mpi-inf.mpg.de/projects/wxu/MonoPerfCap/)
- [CVPR20: DeepCap: Monocular Human Performance Capture Using Weak Supervision](https://people.mpi-inf.mpg.de/~mhaberma/projects/2020-cvpr-deepcap/): 训练一个泛化的网络，给定一个人的重建的模型，输入一张分割好的图像，通过PoseNet和DefNet输出关节旋转角度与节点形变，获得mesh的形变；训练的时候使用关键点，轮廓进行监督.


# Dataset
- [HUMBI: A Large Multiview Dataset of Human Body Expressions](https://openaccess.thecvf.com/content_CVPR_2020/papers/Yu_HUMBI_A_Large_Multiview_Dataset_of_Human_Body_Expressions_CVPR_2020_paper.pdf)
