Sarah Huiyi Cen 关于雷达运动估计的文章只有两篇，目前（2022）在 MIT 做博后已转深度学习研究。

采用的 FMCW 角度采样点 399，距离采样点 2000，距离分辨率 0.25米。水平方向波束宽度 2°，竖直方向波束宽度 25°，采样速率 4Hz。

[[2018 ICRA Precise Ego-Motion Estimation with Millimeter-Wave Radar under Diverse and Challenging Conditions.pdf]]
[[2019 ICRA Radar-only ego-motion estimation in difficult settings via graph matching.pdf]]

两项工作分别提出、改进了利用 FMCW（frequency-modulated continuous-wave）进行自我运动估计的算法中，特征提取与特征联合的步骤。

**特征提取**
前视声呐的特征提取计划使用简单的 Harris 角点。因前视声呐分辨率相较雷达低，径向上没有明显的多径效应，故暂时先不详细了解对应的特征提取算法。

**特征联合**
2018
> it seeks to find the largest subsets of two pointclouds that share a similar shape

2019
>our current method is more efficient and robust due to changes to the ***unary keypoint descriptor and termination condition***.

没有关于图匹配核心算法上的区别
