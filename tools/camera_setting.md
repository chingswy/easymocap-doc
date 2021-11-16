---
layout: default
title: 相机配置
parent: 实用工具
nav_order: 4
---

# 相机配置
{: .no_toc }

这部分文档介绍常用多相机环境的设置，包含相机选型、相机连接、抓取与保存图像

1. TOC
{:toc}
---

## 海康相机配置

## 工业相机配置

这部分文档叙述如何使用FLIR工业相机，对应代码位于`library/EasyFLIR`。

### 相机配置

1. 硬件安装

方法一：使用一台有多个千兆网口的电脑

方法二：使用一台有POE供电的万兆交换机

方法三：使用POE供电+万兆路由器

2. 同步安装

使用同步线，设置相机触发，主要参考[FLIR官方网址](https://www.flir.com/support-center/iis/machine-vision/application-note/configuring-synchronized-capture-with-multiple-cameras/)。具体步骤如下：
* 检查同步线PIN与Color之间的对应关系。不同厂商的PIN与Color之间的对应关系不同。
* 根据工业相机的型号，在官方网址中找到对应的连线方式，进行连接。注意，在相机数量少的情况下，可以不使用对应的电阻。（在电路简单的情况下，可以直接使用绝缘胶带进行缠绕连接）
* 连接完成后，使用Spinnaker的GUI进行验证。开启Master的capture后，任意slave都应可以正常capture；关闭Master的capture后，任意slave应该停止。
* 验证同步误差。通过[ufo](https://www.testufo.com/)检查同步时间差。

3. 安装Spinnaker

运行官方提供的脚本：
```bash
./install_spinnaker.sh
```

进行网络配置，其中`ens3f1`为网卡名称：

```bash
sudo ifconfig ens3f1 mtu 9000
sudo ethtool -G ens3f1 rx 4096 tx 4096
sudo ethtool -C ens3f1 rx-usecs 125
sudo cp /etc/sysctl.conf /etc/sysctl.conf.bak
sudo sh -c "echo '
net.core.rmem_max=10485760
net.core.rmem_default=10485760
' >> /etc/sysctl.conf"
sudo sysctl -p
```

打开软件检查相机是否能正常显示，软件的安装位置在`/opt/spinnaker/bin/SpinView_QT`

以上步骤可使用docker替换。@wangyize

4. 使用Spinnaker GUI进行采集

* 参考[FLIR官方网址](https://www.flir.com/support-center/iis/machine-vision/application-note/configuring-synchronized-capture-with-multiple-cameras/)进行多相机同步GUI采集。
* 用GUI采集会收到带宽的限制而掉帧。用Streaming模式来避免OOM；用RAW模式来加速存图。
* 确保保存的文件夹存在。
* 在做NeRF相关的采集时，需要关闭white balance、Exposure Auto和Gain Auto。

5. 使用cpp代码进行采集

运行脚本，可以预览多个相机同步捕捉的图像

```bash
todo
```


6. 相机标定

见[相机标定](./calibration.md)文档。
