---
layout: default
title: 代码分析
parent: 实用工具
---

# 代码分析工具
{: .no_toc }

1. TOC
{:toc}
---

## 周报生成器

之前的调试方案为：fit -> vis -> observe，这一步需要完成的事情是替代原有的肉眼观察部分。肉眼观察主要实现的功能有：
1. 观察动作的时序抖动，异常值附近通常会有抖动，或者突变
2. 观察与图像的一致性，肉眼可以判断实际的图像的位置，因此尽管2D错误，肉眼可以知道正确的2D位置，通过与重投影的比较，即可知道错误的帧

主要的异常检测的方法有：
- 多视角情况，使用多视角鲁棒三角化重建的关键点检查SMPL拟合的结果
- 少视角情况，判断重投影误差，选出误差大的帧
- 任意情况，计算SMPL参数的速度与加速度，选出速度与加速度超过阈值的帧


对于异常帧，主要实现的功能：
- 裁剪图片：分为身体、左手、右手单独考虑
- 根据异常检测，输出自然语言：
    `第[n]帧由于第[v]个相机的[手腕|左手|右手]关键点检测[失败|遗漏|误匹配]，导致[速度突变|加速度突变|重投影误差大]`
- 自动保存图片与文字，生成markdown/latex文档

检查参数：

```bash
python3 apps/analyze/check_param.py ${out}/smpl --out ${out}
```

生成报告：

```bash
# 可视化一般的情况
python3 apps/analyze/report_outlier.py ${out} ${out}/log --cfg_data config/data/mv1h.yml --opt_data k2d ${data}/annots camera ${data} data.keypoints2d.args.undis False
# 处理MANO的情况
python3 apps/analyze/report_outlier.py ${out} ${out}/log --cfg_data config/data/mv1h.yml --opt_data k2d ${data}/annots camera ${data} data.keypoints2d.args.undis False --mano
```

TODO: 可视化的时候增加SMPL的渲染结果


## 性能分析

## 自动调参

这部分用于在优化的时候进行超参搜索。