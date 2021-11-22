---
layout: default
title: Keep it SMPL
parent: SMPL
grand_parent: 相关论文
mathjax: true
nav_order: 1
---

## Keep it SMPL: Automatic Estimation of 3D Human Pose and Shape from a Single Image

这篇文章从单张图像估计一个SMPL模型的pose参数和shape参数，将这个问题分为两步进行。首先，使用DeepCut来预测2D身体关节坐标；其次用SMPL模型拟合到这些2D关节坐标上。

![1534408669771](assets/1534408669771.png)



### 实现方法

这篇论文将这个估计问题转化为了参数优化问题，而不是使用端对端的方法来实现。这样做的最大的坏处就是慢。它的目标函数定义比较复杂，通过最小化这个目标函数，来找到最适合的模型参数。目标函数总共有5项，分别为

- $$E_J(\beta,\theta;K,J_{est}) $$ 
  $$
  E_J(\beta,\theta;K,J_{est})  = \sum_{joint~i}w_i\rho(\Pi_K(R_\theta(J(\beta)_i))-J{est,i})
  $$
  这一项表示模型投影后的关节位置与前面的关节位置的距离的加权和。$$\Pi_K$$ 表示使用相机参数$$K$$投影到平面上，每个关节位置的权重是根据其位置估计的置信度来的，由前一步给出。对于被挡住的关节，这个值一般比较小，然后这个时候的参数就主要受到pose priors来控制。为了抑制测量的噪声，使用的一个鲁棒的可微的Geman-McClure惩罚函数$$\rho$$ 来自一篇1987年的文章，不知道作者怎么弄到的

- $$E_a(\theta)$$ 
  $$
  E_a(\theta) = \sum_i \exp(\theta_i)
  $$
  这一项用来惩罚膝盖和手腕的不正常的弯曲，就是反关节这种。关节没有弯的时候，$$\theta_i$$ 是0，如果是负的时候，就是正常的弯曲。正的时候就是不正常的，需要进行惩罚。（这一项考虑得也太细致了一点）

- $$E_\theta(\theta)$$ 
  $$
  E_\theta(\theta)\equiv -\log\sum_j(g_j\mathcal{N}(\theta;\mu_{\theta,j},\Sigma_{\theta,j})) \approx -\log\max_j(cg_j\mathcal{N}(\theta;\mu_{\theta,j},\Sigma_{\theta,j})) \\=\min_j-\log(g_j\mathcal{N}(\theta;\mu_{\theta,j},\Sigma_{\theta,j})) 
  $$
  这一项就是pose prior，文章根据CMU数据集来训练，用来表示可能出现的姿态。由于计算求和太费劲了，因此使用最值来进行替代。尽管这一项不是可微的，但是他在每一处都用雅克比矩阵来近似。

  - $$g_j$$ 代表混合模型的权重
  - $$c$$,正的常数

- $$E_{sp}(\theta;\beta)$$

  这一项是为了约束身体避免碰撞，然后由于直接计算是否碰撞太困难了，因此作者使用了一些capsule来对各个部位进行近似，然后判断这些圆柱的碰撞关系，来进行惩罚。

  ![1534407306445](assets/1534407306445.png)

  

  误差项是与胶囊交叉部分的体积有关联的，但是这样仍然难以计算，因此再次进行简化。将胶囊简化为一个球，球心为胶囊轴上的$$C(\theta,\beta)$$，半径为$$r(\beta)$$ ，与胶囊半径一样。
  $$
  E_{sp}(\theta;\beta) = \sum_i\sum_{j\in I(i)} \exp \left(\dfrac{||C_i(\theta,\beta) - C_j(\theta,\beta)  ||^2}{\sigma_i^2(\beta) + \sigma_j^2(\beta)}\right)\\
  \sigma(\beta) = r(\beta)/3
  $$
  在优化shape的时候没有使用这一项。

- $$E_{\beta}(\beta) = \beta_T\Sigma_\beta^{-1}\beta$$ 

  这一项是定义的shape prior，中间的矩阵是通过主成分分析得来的。

### 优化

首先假设相机的位置和身体的朝向是位置的。然后假设人是平行站立于图片平面的，初始化相机的位置，接着通过模型的躯干的平均长度，和我们预测得到的关节计算得来的躯干长度，利用相似三角形来估计人站立的深度。这样只是初步估计，将来会通过$$E_J$$来优化这个参数。接着会固定shape参数，来优化身体姿势。

开始的时候，prior项的系数取大一点，然后再逐渐减小这两个权重。这样据说可以避免局部最优。

如果看到的是人的侧面的话，人朝向哪个方向比较容易引起混淆。因此判断人的肩膀的距离小于阈值的时候，就给他两个方向都进行求解。然后选择损失函数最小的哪个。

### 代码实现

作者使用的是Powell's dogleg来进行优化求解，投影是基于OpenDR的，模型主要是基于Chumpy的。作者优化求解一张图需要一分钟左右。我在i5-5500,8G上跑大概需要两分钟的样子。反正就是很慢。要做到实时是相当难的。
