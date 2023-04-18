[[2004 Acoustic Camera Image Mosaicing and Super-resolution.pdf]]
OCEANS 2004
1. 进行预处理平衡 **insonification profile**。用高斯核平滑图像作为 **profile** 的估计，添加正则项防止低反射区域数值爆炸；
2. 检测采用 Harris 角点，确定一个小图像 patch，在待配准图像同点周围利用 cross-correlation 暴力搜索。
3. 融合
	1. SNR 最大化
	2. 最大似然
	3. **（注意，上述是递进关系。有一些先验假定，说不清楚）**

[[2004 Image Mosaicing of Noisy Acoustic Camera.pdf]]
[[2004 Image Registration and Mosaicing of Acoustic Camera Images.pdf]]
[[2005 Mosaicing of acoustic camera images.pdf]] 该篇为 IET 的，引用最多。
以上四篇一致（多投？）

[[2005 Non-iterative Construction of Super-Resolution Image from an Acoustic Camera Video Sequence.pdf]]
相较之前修改了图像成形的模型，增加了下采样和 PSF 的内容。但还是讲不清楚。

[[2006 Video Enhancement for Underwater Exploration Using Forward Looking Sonar.pdf]]
![[Pasted image 20221017150923.png]]
1. Retinex Separator：高低频分离，LF 是 illumination profile，HF 是 reflectance profile （什么鬼）
除了高低频分离，以及对 reg 的一些详细说明（一对多的配准），核心还是 MAP fusion，和之前一样。

[[2008 MAP fusion method for superresolution of images with locally varying pixel quality.pdf]]
建模成卡尔曼滤波形式，主要的贡献是在融合过程中考虑了不同位置的不确定性是不同的。