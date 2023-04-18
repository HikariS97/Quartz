场景：partially structured scene

[[2015 LARS A Topological Descriptor of Acoustic Images for Navigation and Mapping.pdf]]
用阈值的方式找出兴趣点，扩展成高斯椭圆形成节点，暴力求解图结构的相似性。算法结果仅对 ***match*** 解释，没有里程计、回环检测等内容

[[2016 OCEANS A Modified topological descriptor for forward looking sonar images.pdf]]
通过单独分析每一 ***beam*** 自适应调整阈值，取消图配准时对 ROI 旋转角的依赖

[[2016 LARS Semantic Mapping on Underwater Enviroment Using Sonar Data.pdf]]
进一步利用算得的信息和 SVM 进行分割

[[2017 ICMLA Forward Looking Sonar Scene Matching using Deep Learning.pdf]]

[[2017 IFAC Description and Matching of Acoustic Images Using a Forward Looking Sonar -- A Topological Approach.pdf]]
16 年 OCEANS 的扩展

[[2018 ICMLA Underwater Place Recognition in Unknown Environments with Triplet Based Acoustic Image Retrieval.pdf]]
深度学习的方法暴力匹配，没什么有效内容

[[2018 IROS Reliable fusion of black-box estimates of underwater localization.pdf]]

[[2018 JFR Underwater place recognition using forward‐looking sonarimages -- A topological approach.pdf]]
***Important***
1. 增强
2. 分割
3. 高斯椭圆拟合
4. 图匹配
	1. 顶点匹配
		1. approximated solution?
		2. factorized graph match (FGM)
		3. maximum common subgraph MC strategy
重点在于图匹配，以下为实操：
1. 顶点匹配：用边误差小于一定值的边的个数来定义顶点的相似程度
2. 快速图配准算法：
	1. 为 $v_i^1 \in G_1$  找到两个最相似的 $v_x^2, v_y^2 \in G_2$
	2. 若该两组匹配的差大于一定值，则取最大的值；反之，认为两组都不行。

实验：
1. Object Correspondence？
2. Loop Closure Detection
	1. ***暴力匹配，没有快速搜索***


[[2019 LARS 3D Surfaces Reconstruction and Volume Changes in Underwater Environments Using MSIS Sonar.pdf]]
机械扫描声呐对超大场景的重建，注重应用（冰山观测）而非 general algorithm。