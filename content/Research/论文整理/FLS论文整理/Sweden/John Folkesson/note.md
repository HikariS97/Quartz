[[2022 Submit JOE Neural Shape-from-Shading for Survey-Scale Self-Consistent Bathymetry from Sidescan.pdf]]
依赖于高精度定位信息（NOTE: iNeRF，同时估计？）
Loss Function:
$$
\mathcal{L}=\mathcal{L}_{\nabla}+\alpha \mathcal{L}_H
$$
其中，$\mathcal{L}_H$ 为高度计数据与预测的差值，$\mathcal{L}_{\Delta}$ 则复杂一些，也重要一些：
$$
\mathcal{L}_{\nabla}=\frac{1}{\left|\left\{I_{i, n}\right\}\right|} \sum \left\| \tilde{I}_{i, n}-I_{i, n} \right\| 
$$
将 $\text{ping}_i$ 的 $\text{bin}_n$ 的点 $p_{i, n}$ 用 $\phi_{\text{俯仰角}}$ 参数化，得 $p_{i, n} (\phi)$。并假设表示地面的函数在该表示下***单调***。

如何求 $\tilde{I}_{i, n}$ ？根据 Lambertion Scattering Model，需求出入射平面法向量和入射。先求曲面法向量：
$$
N^\theta(p)=\left[-\nabla_x \theta\left(p_x, p_y\right),-\nabla_y \theta\left(p_x, p_y\right), 1\right]^T
$$
然后求入射角：(这一段脱裤子放屁的推导属实给我整笑了)
$$
\begin{aligned}
r_i(\phi) &= R_i R_x\left(\frac{\pi}{2}\right) R_i^T \frac{d}{d \phi} p_{i, n}(\phi) \\ &= d_n R_i R_x\left(\frac{\pi}{2}\right) \frac{d}{d \phi}[0, \sin (\phi),-\cos (\phi)]^T \\
&=d_n R_i R_x\left(\frac{\pi}{2}\right)[0, \cos (\phi), \sin (\phi)]^T \\
&=d_n R_i[0,-\sin (\phi), \cos (\phi)]^T
\end{aligned}
$$
直接声呐坐标减去交叉点就好了：
$$
\begin{aligned}
r_i(\phi) &= t_i - p_{i, n}(\phi) \\
&=t_i - (t_i+d_n R_i[0, \sin (\phi),-\cos (\phi)]^T) \\
&= -d_n R_i[0, \sin (\phi),-\cos (\phi)]^T \\
&= d_n R_i[0, -\sin (\phi),\cos (\phi)]^T
\end{aligned}
$$
最后结合 Beam Pattern、Gain、Albedo，得到：
$$
\tilde{I}_{i, n}=K A_i M_{i, n}^\theta\left(\phi_{i, n}^*\right) \Phi\left(\phi_{i, n}^*\right) R\left(p_{i, n}\left(\phi_{i, n}^*\right)\right)
$$
**Experiments**
将小于 0.3 的值，以及靠近 AUV 的一半的值舍去，以避免阴影。因为该模型只适用于有反射的情况。

---
[[2022 JOE Neural Network Normal Estimation and Bathymetry Reconstruction From Sidescan Sonar.pdf]]
将 [[2022 Submit JOE Neural Shape-from-Shading for Survey-Scale Self-Consistent Bathymetry from Sidescan.pdf]] 中基于勃朗特反射模型的侧扫声呐回波反射预测改成又 CNN 生成。同时，SIREN 中的基于强度差的代价函数改为基于表面法向量的差的代价函数。

---


[[2022 sensors Sidescan Only Neural Bathymetry from Large-Scale Survey.pdf]]
是
利用 SSS 和 INR 显示测深图。
***输入：***
1. 反射信号的位姿（姿来自声呐、载具）和强度
***输出：***
1. 测深高度图
2. 反照率图
3. 波束形式
4. 增益
