# Paper List: 与声呐相关的目标/纹理/显著检测
*不仅仅是多波束前视声呐，也包含侧扫、合成孔径等*

[[1984 JASA T.K. Stanton Sonar Estimates of Seafloor Microroughness.pdf]]

[[2002 Ronald Kessel Using Sonar Speckle to Identify Regions of Interest and for Mine Detection.pdf]]

[[2003 Ronald Kessel Texture-based Discrimination of man-made and natural objects in sidescan sonar imagery.pdf]]

[[2003 JOE An Automatic Approach to the Detection and Extraction of Mine Features in Sidescan Sonar.pdf]]
***利用 MRF 分割阴影、背景、目标***。利用了被检测物体在声呐图像中的大小的先验信息。

[[2007 JOE Mean–Standard Deviation Representation of Sonar Images for Echo Detection - Application to SAS Images.pdf]]
利用 Mean-Standard Deviation 表示法计算一二阶统计特性来代替传统的基于强度的阈值方法来检测 SAS 中的水下目标。
但 Mean-Standard Deviation Plane 失去了图像的空间信息 

[[2011 ICIRS David P. Williams On Adaptive Underwater Object Detection.pdf]]
这篇文章又简单又复杂。服了。
highlight-shadow pattern recognition 是过去水下目标检测的重要手段，缺有以下几点问题：
1. 检测算法任务图像质量是均匀的
2. 未考虑目标物回波和阴影的距离依赖性（物理传播模型可补偿）
3. 没有考虑到执行任务时的地理环境，而采用了来自不同环境的训练图片
4. 检测阈值与物理意义无关，是一个经验值
5. 采用 ..... （不知道您在说什么）
文章的算法由三个部分组成：
1. shadow detection
	1. 采用 integral-image 表示
	2. 估计背景。Background Score 就是该像素点周围的均值。
	3. 估计阴影。已知 AUV 高度
2. ripples detection
3. object echo detection


[[2012 OCEANS Integrated MCM missions using heterogeneous fleets of AUVs.pdf]]
文章更多描述了一个系统。系统中包含了 SAS、SSS、FLS 的 ATR 效果。

 
注重 man-made objects。**但方法部分没有讲 man-made 特殊在哪。**
并非整个图像，而只关注手动选取的 ROI。计算背景时排除滑动窗口内的像素，计算回声时只计算滑动窗口内的元素。
![[Pasted image 20220726215903.png]]

---
[[2014 Ferreira Improving Automatic Target Recognition with Forward Looking Sonar Mosaics.pdf]]
[[2015 Ferreira Forward looking sonar mosaicing for Mine Countermeasures.pdf]]
应用声呐拼接图像提高信噪比，提升 MCM 的成功率。
避免从各个角度照射物体，比如 parallel and perpendicular 来造两个图

> This ensures a consistent representation of the target

同样避免 loop closure 以防止离谱的优化
采用 DGPS/INS 与 Correlation 结合的方式
提出简单的融合方法，利用新的图片和过去 mosaic 的平均。同样无法避免目标信息被地面反射淹没的问题。
有明显的缝隙

---

[[2015 GRSS David P. Williams Fast Unsupervised Seafloor Characterization in Sonar Imagery Using Lacunarity.pdf]]
Supervised 有其固有的缺陷：1. 遇到新地形时；2. 水下地形往往是复杂的复合，而很少能简单的区分。利用 Lacunarity 计算图片以区分地形，采用 ***integral image*** 加速。

---
[[2020 JASA 中科院 Detecting moving targets in active sonar echograph of harbor environment using high-order time lacunarity.pdf]]
将 Lacunarity 拓展到时间维度，并增加了阶数。
当所截取的时间窗口无穷大时，方差趋近于无穷小（假设大部分时间是静止的），则 $Lac$ 趋近无穷小，无法区分 ***Target***。因此，对时间窗口的选择是一个需要考虑的问题。

***Lacunarity*** 由 Mandelbrot（1983）于分形几何中提出，并由 Plotnick（1996）扩展至可以对灰度图像的像素点变化进行评估。
关于 ***Lac*** 的计算：
$$
L=E\left[\left(\frac{P_{t}}{E\left[P_{t}\right]}-1\right)^{2}\right]
$$
当 $P_t$ 越偏离期望值，则 $L$ 的值越大。

---
[[2022 JOE Narcís Palomeras Automatic_Target_Recognition_for_Mine_Countermeasure_Missions_Using_Forward-Looking_Sonar_Data.pdf]]
利用神经网络自动检测目标物

[[2022 JOE Validation of Targets in Sonar Imagery Using Multispectral Analysis.pdf]]
todo...



# Paper List: 图像融合相关
[[2004 OCEANS Fusion of Multiple Side-scan Sonar Views.pdf]]
利用 Gabor Wavelet Decomposition 分离空间频率，进行融合，最后再合成频率，获得最终的侧扫图像拼接。

[[2006 TIP The Fusion of Large Scale Classified Side-Scan Sonar Image Mosaics.pdf]]


[[2012 David P. Williams SAS and Bathymetric Data Fusion for Improved Target Classification.pdf]]
将一个物体不同视角的 SAS 图像融合成单张多视角 SAS 图像。
但文中使用的示例并没有解决目标被背景平滑的问题，而是像右子图所示的那样尽可能使其重合。基于特征点、区域、模板的方法均不适用于 SAS 图像（提到了基于已知物体的投影，但 Yusheng Wang 2022 OCEANS 并未引用），故提出了利用额外的信息源：Bathymetry Map 来恢复物体的高度。并且只假设了位移的存在，并没有考虑到旋转的因素。（手动旋转）
![[Pasted image 20220719155330.png]]

[[2013 OCEANS Hurtos A novel blending technique for two-dimensional forward-looking sonar mosaicing.pdf]]
有两种图像融合的技术类别：1. 柔和边缘，2. seam finding 来寻找合适的切割线
提到了无用的信息会 fading out 有用的信息，但只考虑了精准配准的情况，没有考虑到不精准回环时将有用信息中和的是海底背景，无法通过 saliency map 去除。

[[2015 Ferreira Real-time mosaicing of large scale areas with forward looking sonar.pdf]]
利用 DGPS 或 USBL 的信息得到已拼接图像与新图像的相关关系。不知道具体怎么做的，反正也没啥大用。

[[2018 IROS Pedro V. Teixeira Multibeam Data Processing for Underwater Mapping.pdf]]
算法的特殊之处：1. 窄波束宽度，2. 探测悬浮空中物体。
NOTE: Related Work 可以做很多参考
做了预处理，使分割的结果更加准确。
分割的效果依赖于先验分布

[[2020 Gloabl Oceans Jose Melo A Belief Propagation based algorithm for autonomous mapping of underwater targets.pdf]]
运用复杂的 BP 方法（似乎是近年才提出的），整合了一个目标物多次 Detection 的信息。


# 图像分割 Review
## 概率图模型 Review
# Roadmap
## 目的


**2D 多波束前视声呐**相较于**侧扫声呐**具有更多波束，相较于**多波束回声测深仪**具有更大的波束宽度，一次成像时对更大的体积空间进行声探测，是潜器近底自主导航的基础。相较于**机械扫描式声呐**成像帧率较高，不易受到潜器自身运动的影响。相较于 **3D 多波束前视声呐**，**合成孔径声呐**，**水下激光雷达** 价格较低，体积较小。相较于**光学探测设备，如：摄像头** 不易受到水体浑浊程度的影响。高频 2D FLS 具有厘米级的近距离观测精度，是现代化探测的重要设备。

其成像原理虽然已经得到了很好的几何分析，但由于传感器信噪比相对较低，声波作用于物体表面时的散射建模较为复杂，且海底感兴趣物体的尺寸通常较小，成像效果受到 vintage point 的巨大影响等因素的影响，2D FLS 的成像分析仍然不够精准。这个不精准直接导致了精准的物体重建和目标检测领域的重要挑战，目前仍然是研究的热点。

尽管如此，仍然有很多先前的工作意在利用 2D FLS 处理 low level 的视觉问题，如：自我运动估计[........]、图像拼接[......]、三维重建[........]。其中，2012年 Girona 的工作因其引入了傅里叶变换而获得了稳定的帧间估计，以及引入 Pose Graph 进行全局平滑处理，生成了极其振奋人心的高清声学图像拼接结果，而备受关注。其中，回环检测对高精度图像的生成起到了巨大的作用。我们知道，回环检测在[.....]得到了广泛应用，其可以通过平滑的方式 bound error。在探测巡航任务中，为了确保覆盖待探测区域，潜器往往回覆盖式地进行走航式任务，导致图像和图像之间（非时间上相邻，而是条带相邻）具有较大的重合。这个重合是回环检测的基础之一。如 Girona 所示的那样，如果没有回环检测，由于帧间配准的累积误差，图像会变得模糊。而有了回环检测，图像会清晰很多。我们发现由于声呐图像受 vintage point 的影响较大，Girona 的工作中声呐的 膛线瞄准方向 垂直于潜器行进方向，且利用了潜器可以左右平移的特点使得声穿透始终来自一个（大致）的方向，使得不同条带的图像具有相似的成像效果，这是回环检测的基础之二。而普通 AUV 往往不具备这样的能力，但 浙大的 AUH 和 [....] 具备，或者利用电机，如 SoundMetrics 官方就有提供的 AR2/AR3，也可做到类似结果。然而，前视声呐如其名字所阐释的那样，对 forward saliency detection 有着重要的意义。2018 Florence 虽然将声呐配置调整为前向，但其并没有使用回环检测，也可以在拼接结果中看到救生浮筒的光影反映出了 vintage point 的不同。2019 JFR 基于相对位置进行回环检测，但所使用的数据集与常见的回环图像不类似，.......。因此，回环检测是一个还未较好解决的问题。因此，如 Girona 所示的那样清晰的图像也许适用于小范围可控环境的拼接图像生成，而并不适用于实际的大范围未知环境的扫测任务。

巡航任务中，目标检测往往是终极目标，如：海底管道跟踪、反雷工作等 [..............]。2022 年 JOE 的反雷工作是目前可知的最新的反雷工作，工作区域小，利用了神经网络。Debris 那片论文提供了数据集，但相比于传统 CV 界，数据量很小。且如上述，图像模态受到大量因素影响。因此，基于数据驱动的方法还需要大量的数据去补充。虽然有很多模拟器出现，但其往往只考虑了几何成像原理，导致生成结果和实际图像有较大的隔阂。Yusheng Wang 有 CycleGAN 进行模态生成，但......。2022 年 Naval 那一片考虑了物理，但受限于声呐的物理性质，大规模生成图像仍然较早。传统的方法由于较小的分辨率往往容易失效。

为了使 Correlation 的方法更加鲁棒，我们采取更高的帧率。但大量的图片会影响拼接结果。

声学图像拼接可以将几百上千张图像拼接为完整的大图，是扫测区域的一种更紧凑的表达方式。一张清新的地图可以方便人眼快速找出感兴趣位置，定位可疑目标物位置。然而，主要有两类误差使得获得清晰的可见的声学地形图较为困难：
1. imaging pattern. 
	1. ARIS 和 Blueview 似乎可以调节到底部的距离，但 Oculus 无法调节距离。这一距离需要结合声呐俯仰角度和高度计才能撇去，在没有高度计的情况下需要估计黑色的部分。这虽然较为麻烦，但也可以去除，哪怕精度不是很高。如 Oculus 声呐的一个 pattern，会加大去除的复杂程度。（2014 Blending 中也有使用）
	2. 远距离截止点。容易受到多径和旁瓣的影响，甚至不一定呈现黑色区域，导致目标物进入视野的时候不一定出现在图像边缘处。
2. accmulated error.
	1. 在来回扫测时同一个物体出现在不同的全局坐标位置，因此，本应该出现两处的目标被背景反射平均，导致不明显甚至消失。

有不少的方法尝试建立 3D 模型，可以一定程度上将目标物分离出物体。但其复杂的几何逻辑和假设，以及在面对复杂物体时，3维重建往往丢失了很多细节，与人眼清晰可见的目的背道而驰。

2014/2015 Ferreira 利用图像拼接进行反雷，但没有考虑以上的细节，仅仅使用拼接增强图像质量，再输入 ATR 算法。

因此，文章的目的就是在上述两个图像融合的误差下，使得目标物体清晰可见。


我们需要提出几个问题：
1. 假设已经拿到每一张图像中目标物完美分割的轮廓，如何设定 Blending Method 才能较好得突出目标物显示？
2. 如何获得图像中目标物的分割轮廓？这是一个分割问题，需要把图像分割成前景、背景，或者目标、阴影、背景。
	1. 利用 SpatioTempo Lacunartiy 为什么不行？


# 相关软件
[SAMM](https://www.oicinc.com/samm.html)
Oceanic Imaging Consultants
支持众多声呐，但拼接基于 GPS 数据

[ClearSight](https://acousticview.com/)
AcousticView
只支持 Didson，上限 1000 frames。

[SoundTiles](https://iquarobotics.com/soundtiles)
IQUA Robotics
支持众多声呐

都没有明显地考虑到外场实验的配置


LBL/USBL 设备以及其自身的标定工作都是相对于探测设备的额外成本