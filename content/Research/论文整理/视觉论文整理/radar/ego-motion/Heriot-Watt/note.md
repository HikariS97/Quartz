[[2020 IROS RadarSLAM_Radar_based_Large-Scale_SLAM_in_All_Weathers.pdf]]
1. 特征提取：利用 SURF 提取点，加入 Motion Prior 进行限制，利用描述子配准，用边误差与一个预设的 $\delta_c$ 比较抵消外点；
2. 局部优化：局部 BA
3. 回环检测：
	词袋模型在视觉中有很好的效果，但在 Radar 数据下并非如此，原因如下：
	1. 描述子重复
	2. 多径进一步加剧描述子的模糊
	3. 视觉转换导致场景剧烈变化
	图像转换成点云，再一次提取点，形成旋转不变描述子
5. 优化：g2o
文章构建基于 ORB-SLAM
单从里程计的角度来说，并不一定[[2018 ICRA Precise Ego-Motion Estimation with Millimeter-Wave Radar under Diverse and Challenging Conditions.pdf]]好。