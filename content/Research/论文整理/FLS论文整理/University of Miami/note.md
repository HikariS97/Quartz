# Shahriar Negahdaripour
[[1998 OCEANS Use of Forward Scan Sonar Images for Positioning and Navigation by an AUV.pdf]]
使用了 220KHz 的自制声呐对沉船拍了 50 张。初步使用 Optical Flow 进行位移的估计

[[1998 TPAMI Revised Definition of Optical Flow.pdf]]

[[2005 Epipolar Geometry of Opti-Acoustic Stereo Imaging.pdf]]

[[2005 OCEANS Calibration of DIDSON Forward-Scan Acoustic.pdf]]
利用多张已知目标物的照片来确定镜头畸变参数，从而校准 DIDSON 声呐

[[2005 On Processing and Registration of Forward-Scan Acoustic Video Imagery.pdf]]
提到[[2005 OCEANS Calibration of DIDSON Forward-Scan Acoustic.pdf]]进行预处理，纠正了beams和beams的角度畸变，通过平均图像的方法纠正声透射不均匀的问题，并沿着beams的方向取平均得到beams pattern。通过图像拼接的方式（对一个地方的成像，通过配准校正重叠）评估噪声，得到bimodel噪声。讨论conformal和affine变换对声呐图像变换来说是合理的近似。利用手动提取配准特征解conformal模型以及自动提取配准特征解仿射变换模型比对拼图结果，并认为投影变换参数较多，而匹配特征较少，所以结果不稳定。

[[2007 3D Motion from 2-D FLS Video.pdf]]
运用 Harris 角点和 Lucas-Kanade 追踪匹配点，求解 9 自由度 Homography Matrix。无详细数据分析和过程。

[[2010 OCEANS 3-D Motion Estimation in passive navigation by acoustic imaging.pdf]]
没有提什么是 passive navigation。将 arc 分为 low、medium、high 三个区域。没有提到如何建立特征点的 match，利用 hough array 估计角度、平移，并有一些评价指标，如角度估计倾向于两个特征点为沿着声呐横向移动的方向。角度估计误差约 1.5 °，距离约厘米至10厘米级。提及该工作可以为双声呐的点对关系打下基础。

[[2010 OCEANS On 3-D reconstruction from stereo FS sonar imaging.pdf]]
相较于 2009 年第一篇 Herriot Watt 的垂直方向上叠放的双声呐系统，这篇文章解决任意摆放（非严格对准）情况下的声呐双目视觉，并分析退化问题。文章假设该任意摆放的位置是可以通过配准标定的，已知的。

[[2010 OCEANS Performance and accuracy in visual motion computation from FS sonar video sequences.pdf]]
主要为误差分析。仍然没有自动的特征点提取配准

[[2011 OCEANS Dynamic scene analysis and mosaicing of benthic habitats by fs sonar imaging-issues and complexities.pdf]]
提出动态环境下声呐图像配准、拼接等问题，包含特征点选取、跟踪、融合图像处理等

[[2012 CVIU Visual motion ambiguities of a plane in 2-D FS sonar motion sequences.pdf]]

---
[[2012 OCEANS On feature extraction and region matching for forward scan sonar imaging.pdf]]
[[2013 JFR On feature matching and image registration for two‐dimensional forward‐scan sonar imaging.pdf]]
相比 [[2010 IROS Johannsson.pdf]] 可以多估计声呐的俯仰，直接采用 3D 声呐运动模型，采用 Gaussian Map 而不是 NDT Map（NDT map 需要选择方格大小，从而平衡了精度和计算时间）
特征点的检测基于强度的信息，总觉得处理起来不是那么方便。
利用 object-shadow pair 计算 occluding contour 的三维点，该三维点至起始点位之间的俯仰角用线性插值补齐（没有利用亮度区域的强度值）

仅利用
$$
\mathbf{s}^{\prime}=H \mathbf{s}, \quad H=\left[\begin{array}{ccc}
\left(1-\frac{t_{z} \sin \phi}{R}\right) & \omega_{z} & -\frac{t_{x}}{\cos \phi} \\
-\omega_{z} & \left(1-\frac{t_{z} \sin \phi}{R}\right) & -\frac{t_{y}}{\cos \phi}
\end{array}\right]
$$
进行分析有这么五个量有影响，然后计算出 $\phi$ 后，就可以换成普通的 $\vec{t}$  进行优化了。
但 $R$ 应该是受约束的，文中却说 Matlab 求解时没有约束。

---
1. [[2012 OCEANS On 3-D Motion estimation from 2-D sonar image flow.pdf]]
2. [[2012 OCEANS On 3-D Scene Interpretation from F-S Sonar.pdf]]
3. [[2013 OCEANS Forward-look 2-D sonar image formation and 3-D reconstruction.pdf]]
4. [[2013 T-RO On 3-D motion estimation from feature tracks in 2-D FS sonar video.pdf]]

1 中介绍 image flow 的微分

2 中介绍如何利用高度计、斜度计辅助判断每一个特征点的**俯仰角，以及所在平面（平面假设）的法向量**。并利用 radiometric 方向上 object-shadow 辨识，提取 object-shadow 轮廓，估计
物体轮廓。但表述并不清楚，且轮廓提取、特征点提取没有对应的方法介绍。

3 同时测试了在水中的物体（鱼类）以及置底的物体。假设物体在声呐坐标系中**距离单调变化**。虽然考虑了两片 patches 同时反射声波的情况，但考虑得较为简单粗暴。也考虑了地面反射抵达物体表面从而生成的回波，该回波被简单（人工）去除。方法为：根据 object-shadow 得到物体的起始、结束三维点，从底开始连续地根据回波强度计算物体表面至顶部。

---
[[2015 OCEANS Modeling 2-D forward-scan sonar imagery for diffuse reflectors.pdf]]
[[2016 JOE Modeling 2-D lens-based forward-scan sonar imagery for targets with diffuse reflectance.pdf]]

---
space carving
[[2015 OCEANS Modeling 2-D forward-scan sonar imagery for diffuse reflectors.pdf]]
[[2017 JOE Space Carving.pdf]]
[[2017 OCEANS Refining 3-D object models constructed from multiple FS sonar images by space carving.pdf]]

---
双目
[[2018 OCEANS Analyzing epipolar geometry of 2-D forward-scan sonar stereo for matching and 3-D reconstruction.pdf]]
[[2020 JOE Application of forward-scan sonar stereo for 3-D scene reconstruction.pdf]]