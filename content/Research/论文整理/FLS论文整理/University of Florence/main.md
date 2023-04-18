## 2018 AUV A Forward Looking Sonar Based System for Underwater Mosaicing and Acoustic Odometry
1. 拼图
2. 测速

### FeelHippo AUV
FeelHippo AUV 由佛罗伦萨大学工业工程学院建造，小型、短途、浅海，用于参加面向学生的国际机器人比赛。搭载
- U-blox 7P GPS
- Nortek DVL1000（可充当测深仪）
- Xsens MTi-300 AHRS，加速度计、陀螺仪、磁力计
- KVH DSP 1760 单轴高精度光纤陀螺仪用于指北
- EvoLogics S2CR 18/34 声学节点
- Teledyne BlueView M900 2D FL
- Ubiquiti PicoStation M2 高速短距宽带水面 WiFi 节点
- Radio Modem 868+ RFDesignable 长距海面通信
- 朝底 ELP 720p MINI 摄像头
- Microsoft Lifecam Cinema 前视摄像头
- 两个 ELP 1080p MINI 侧视摄像头
- 60 厘米见方
- 35 千克
- 35 水深
- 2~3 小时巡航
- 最大航速约 1m/s

### Mosaic
前视声呐在横滚角上的变化会引起基于傅里叶域的图像配准算法退化。关于这一点，FeelHippo AUV 没有控制横滚角的推进器，维持横滚角不变的客观条件是静态水域。

图像配准仅用于估计水平位移，角度估计来自 FOG。水平位移的估计与惯性导航的选择依赖于水平位移估计的尖峰与均值的比值，没有融合。

~~Global Alignment 就是把相邻的两帧拼接起来。作者提到 SLAM 不适合在实时性要求较高的 AUV 上做。~~

Mosaic Blending 前的增强：
1. 平均以去除 insonification pattern
2. 利用 Contrast Limited Adaptive Histogram Equalization（CLAHE）
3. 去除没有信息的区域

### Odometry Estimation
有什么意义？
1. 消除 DVL 和 FLS 的串扰
2. 深水环境下超过了 DVL 的作用距离（通常有好几十米，这个情况下 FLS 也好不到哪里去大概）
3. 减少传感器数量

### 实验
2000 $m^2$
![[The competion arena at the NATO STO CMRE (La Spezia, Italy).png]]


## 2019 OCEANS Experimental Evaluation of a Forward-Looking Sonar-Based System for Acoustic Odometry

在 [[#2018 AUV A Forward Looking Sonar Based System for Underwater Mosaicing and Acoustic Odometry]] 的基础上增加了以下几点分析：
- FeelHippo AUV 增加了横向方向的速度约 0.3m/s，深度减小到 30米。
- 建立 AUV 模型以将推力转换为速度估计
- **利用 UKF 融合基于 AUV 模型的速度估计和 FLS 的速度估计，而不是简单的惯导与 FLS 速度估计的非此即彼的策略**

### AUV 运动模型
$$
M \dot{\boldsymbol{\nu}}+C(\boldsymbol{\nu}) \boldsymbol{\nu}+D(\boldsymbol{\nu}) \boldsymbol{\nu}+\mathbf{g}(\boldsymbol{\eta})=\boldsymbol{\tau}(\boldsymbol{\nu}, \mathbf{u})
$$
具体数值并不了解，但 $\tau$ 代表了 AUV 线速度与推动器旋转角速度之间的关系。

在假设海流影响较小的情况下，对 FeelHippo AUV 做了一些运动学上的假设。

### FLS Registration
增加了对所用滤波器的描述：
在逆变换后的矩阵上应用非自适应低通巴特沃斯滤波器。
$$
H(u, v)=\frac{1}{1+\left(\frac{D(u, v)}{D_{0}}\right)^{2 n}}
$$


## 2020 OE A forward-looking SONAR and dynamic model-based AUV navigation strategy: Preliminary validation with FeelHippo AUV
这篇文章是 2018 年 AUV 的扩展 [[#2018 AUV A Forward Looking Sonar Based System for Underwater Mosaicing and Acoustic Odometry]]，包含了 [[#2019 OCEANS Experimental Evaluation of a Forward-Looking Sonar-Based System for Acoustic Odometry]] 中的模型，但不包含滤波器。

作者认为优于 DVL 的估计在近几年是不可能做到的，但与 DVL 的合作估计是可行的（但并未提到如何与 DVL 合作。

[2010 JOE Autonomous Underwater Vehicle Navigation](https://ieeexplore.ieee.org/abstract/document/5546885) 提到 DVL 可能会失效：
- 离地太近？
- 角度太倾斜或者面临悬崖等，导致多个波束失效等

文章主要注重后处理，但尽可能地模拟 online 处理的方式。

进一步详细了算法步骤，新增了 FeelHippo AUV 更多的参数。

平均 0.9 米，最大误差 2m。





## 2020 JRF Underwater navigation with 2D forward looking SONAR: An adaptive unscented Kalman filter‐based strategy for AUVs
这篇文章是 2019 OCEANS 的扩展 [[#2019 OCEANS Experimental Evaluation of a Forward-Looking Sonar-Based System for Acoustic Odometry]]，在 [[#2020 OE A forward-looking SONAR and dynamic model-based AUV navigation strategy Preliminary validation with FeelHippo AUV]] 的基础上加入了 [[#2019 OCEANS Experimental Evaluation of a Forward-Looking Sonar-Based System for Acoustic Odometry]] 中的滤波器。