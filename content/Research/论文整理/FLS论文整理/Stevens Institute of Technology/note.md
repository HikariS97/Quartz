[[2020 IROS John McConnell Fusing Concurrent Orthogonal Wide-aperture Sonar Images for Dense Underwater 3D Reconstruction.pdf]]
两个 Oculus 视角正交防止，集成于 BlueROV2。Association 仅作用于**交叉**视场。
1. 特征提取：利用改进后的 CFAR，即 SOCA-CFAR
2. 聚类与聚类联合：计算聚类以限定特征点的配对。聚类联合基于类别的描述子，描述子由点的均值、方差、最大最小距离构成。
3. 特征联合：水平、竖直方向计算均值形成两个描述子

所存在的问题：
1. CFAR 适合距离检测，然而 wide-aperture 声呐的目的是探测一定的空间，而不是一个平面
2. 交叉视场角太小，非交叉的部分没有得到利用
3. H (Horizontal) 与 V (Vertical) 的图像之间的特征点不应该是 bijective，因为显然两者的点不是一一对应的关系，而是 m 对 n (m < n) 的概率关系
4. 聚类的部分**应该**起到主要作用的是距离（从文章所呈现的结果上来看亦是如此），其余的没有什么用，也没什么道理（尤其是方差，没有相似性）
5. 虽然目标是复杂环境，但桥墩、防喷器都是高度对称的结构物体。

***误差？***
5m 距离下，5cm 左右的误差

[[2021 ICRA John McConnell Predictive 3D Sonar Mapping of Underwater Environments via Object-specific Bayesian Inference.pdf]]
3DOF

为 2020 IROS 的拓展。2022 IROS 中提到交叉区域小，2021 ICRA 这篇在原作基础上，通过语义分类的方式对非交叉区域的进行语义分类、预测。分类通过一个手工标注的 toy-model CNN，预测通过贝叶斯。此外，加入了基于 ICP 地理近邻的回环检测。

**误差？**
等做重建的时候再关心误差

[[2022 RA-L John McConnell Overhead Image Factors for Underwater Sonar-based SLAM.pdf]]
3DOF

所谓 Overhead 指的是卫星图片的分割。Factors 指的是卫星图片参与的步骤成为一个因子加入到因子图中。实操中，通过 U-Net 把当前声呐图像和 Overhead 图像合成一个当前声呐图所表示的 Overhead 图，然后与原 Overhead 图做配准获得估计。

（感觉此方法非常 brittle，没有发展空间）


[[2022 Bredan Englot Virtual Maps for Autonomous Exploration of Cluttered Underwater Enviroments.pdf]]
注重后端，包含路径规划，建立在很多之前的工作基础上，因不太相关，留之后需要再看。
***previous work*
- Wang, Jinkun, and Brendan Englot. "Autonomous exploration with expectation-maximization." _Robotics Research_. Springer, Cham, 2020. 759-774.
- Wang, Jinkun, Tixiao Shan, and Brendan Englot. "Virtual maps for autonomous exploration with pose slam." _2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)_. IEEE, 2019.