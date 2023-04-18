# 一些资源汇总
- https://github.com/SoundMetrics/aris-file-sdk/tree/master/beam-width-metrics 中有官方给出的 ARIS 声呐的 beam pattern 的数据。 
- https://github.com/pvazteixeira/multibeam 给出了处理 DIDSON mapping 的方法。给出的 DIDSON 的 fov 是 *0.5026548245743669/28.8*，根据表格计算出的是 *28.812*。fov 在更新 azimuth 的过程中**还是赋值成了 *28.812***。每次更新 azimuth 后，需要用 5 阶的曲线拟合 azimuth table。在计算 lookUpTable 时需注意， res 应该是一个合理的值。在 pivazteixeira 的计算中很有可能是一个极小的值，甚至可能是 0（当 rmin = 0 时）。其中，coefficients 的计算与 Matlab 相同。
- http://www.soundmetrics.com/My-Account/Downloads/DIDSON/Products/DIDSON-300m/DIDSON-300m-Product-Specs-English.pdf 官方给出的 DIDSON fov 为 29°
- 官方的邮件回复中告知 ARIS 的 beam width 是由多个 lens 的平均组成，标定是 27.8°。官方的 GitHub 文件中若按 beam's center 计算则是 27.625°，按照最左侧至最右侧计算为 27.839°。
- 官方提到 lens 的硬件上，DIDSON 与 ARIS 是一样的，只是官方的软件在处理 DIDSON 时用的是 curve fit，在处理 ARIS 时为 table。
- 2005 OCEANS Calibration 中给出的官方数值为：
$$
s(a_3\theta ^3+a_2\theta ^2 + a_1\theta + a_0)
$$
其中，$s=1.012$, $a_3 = 0.0030$, $a_2=-0.0056$, $a_1=2.7151$, $a_0=48.62$.
- 2020 年更新的 DIDSON Matlab 脚本中数值为：
$s=1.012$, $a_3 = 0.0030$, $a_2=-0.0055$, $a_1=2.6829$, $a_0=48.04$.
- 
# 如何写一个好的 mapping function？
## 一些注意事项
1. 对于 ARIS 声呐，拿到的是 `BeamLookUpTable`，对于 DIDSON，pvazteixeira 既给出了 table，也可从 Matlab 一样的系数进行计算。
2. 图的分辨率应可任意选择，默认高度与 polar 图像的 bin 的个数一致。
3. 图上的每一个像素点会产生一个角度，当为系数时，角度可以方便地转换成 BeamNum；当为 `BeamLookUpTable` 时，既可以通过拟合曲线再计算 BeamNum，也可以通过 BeamWidth 直接归到某一个 BeamNum 里。不过曲线拟合的方式可以更简单地进行线性插值。
## 具体步骤
```
meters_per_bin = (maxr - minr) / num_bins
meters_per_pixel = 0.01
res = meters_per_pixel

width_in_meters = maxr*(sin(maxa) - sin(mina))
width_in_pixels = ceil(width_in_meters / meters_per_pixel)
xres = width_in_meters / width_in_pixels
x0 = 

height_in_meters = maxr - minr * cos(min(abs(maxa) abs(mina)))
height_in_pixels = ceil(height_in_meters / meters_per_pixel)
yres = height_in_meters / height_in_pixels
y0 = 

# res / xres / yres 有微小的差别。res 越小，差别也应越小。

# 创建
(mag, angle) = cv2.cartToPolar(x.flatten(), y.flatten())

row_polar = mag
col_polar = angle
```


---

[[2005 OCEANS Calibration of DIDSON Forward-Scan Acoustic.pdf]]
做一个方框
![[Pasted image 20221028103203.png]]
假设完上面每一个点的坐标，则可通过外参（至相机坐标系）、映射（包含内参的 3D -> 2D 过程）得到两个测量。目前主要是对 Beams 进行矫正，一个点为一次测量，但有 4 个未知量。假设一次观测中有 K 个点，则有 K 个测量，未知量为 6 + 4，则需要 $K \ge 10$ 才能解出三次多项式的 4 个未知数。在这个过程中，图像中的点（提取、位置确定）与方框实物上的点的匹配关系是已知的。

**可能存在的问题**
1. 由于量化误差的存在，可能点越近越集中，数值计算越不稳定（可从矩阵条件数推断）。
2. 三次多项式不一定好。
3. 交替推断的方式的收敛不一定是最优。

**提出一个新的方法可以具有的优点**
1. 不需要标定物，只需要海底一些特征等
2. 不依赖于三次多项式
3. jointly optimization

# 采用直线信息进行校准
## 相关工作
1. 上海大学 [[2012 Detection and Tracking of Underwater Object Based on Forward-Scan Sonar.pdf]]
2. 上海大学，类似的共两篇，都发在 AOR 上[[上海大学/2021 Applied Ocean Research A submarine pipeline segmentation method for noisy forward-looking sonar.pdf]]
3. [[2021 JOE 泰国 A_Pipeline_Extraction_Algorithm_for_Forward-Looking_Sonar_Images_Using_the_Self-Organizing_Map.pdf]]
	1. CFAR 
	2. 先扩张后消除小片区域以提取连接的管道（hyperparameters多得离谱）
	3. 利用 Self-Organizing Map 用一维分段直线去拟合二维管道（应有一定的必要性）
4. 2022 OCEANS Hampton Roads --- Real-time Wall Identification for Underwater Mapping using Imaging Sonar。用了 MSIS 和 Hough Transform 检测墙等平面（所形成的直线）
5. 

## 相关公式推导
## 基础的变换公式
### 直角坐标系至极坐标系
$$
\mathbf{p}=\left[\begin{array}{l}
x \\
y \\
z
\end{array}\right]=\left[\begin{array}{c}
r \cos \theta \cos \phi \\
r \sin \theta \cos \phi \\
r \sin \phi
\end{array}\right]
$$
### 极坐标系至直角坐标系
$$
\mathbf{s}=\left[\begin{array}{l}
r \\
\theta \\
\phi
\end{array}\right]=
\left[\begin{array}{c}
\sqrt{x^2+y^2+z^2} \\
\arctan 2(y, x) \\
\arctan 2\left(z, \sqrt{x^2+y^2}\right)
\end{array}\right]
$$
### 波束的表示
假设共有波束$N$个，第$n$个波束的开角为：
$$\theta_n, n\in \{1, 2, 3,...,N\}$$
同时，用：
$$
\theta_{n, n+1}, n\in\{1, 2, 3,...,N-1\}
$$
表示$\theta_n$与$\theta_{n+1}$之间的距离（弧度）。

## 假设直线处于投影平面
则直线上处处 $\phi=0$。

进一步假设直线贯穿声呐视角，我们会得到这样的一组数据：
$$
\mathbf{p}_l = \{\theta_{n},r_{n}\},n\in \{1, 2, 3,...,N\},
$$
其中，$r$来自于**真实的**$\theta$的采样，$\mathbf{p}_l$为表达直线的一组点。这组点在直角坐标系下呈现**非均匀采样**。假设一条直线为：
$$
ax+by+cz=d,
$$
通过转换公式可以得：
$$
a(r \cos \theta \cos \phi)+b(r \cos\phi \sin\theta)+c(r\sin\phi)=d.
$$
由于$\phi=0$，则简化为：
$$
a(r \cos \theta)+b(r\sin\theta)=d.
$$
根据$N$个测量点，得到：
$$
\left[
\begin{array}{c}
r_1\cos\theta_{1} & r_1\sin\theta_{1} & -1 \\
r_2\cos\theta_{2} & r_2\sin\theta_{2} & -1 \\
... & ... & ...\\
r_N\cos\theta_{N} & r_N\sin\theta_{N} & -1 \\
\end{array}
\right]
\left[
\begin{array}{c}
a \\
b \\
d
\end{array}
\right]

=

\mathbf{0}^{N \times 1}
,
$$
形式为：
$$
\mathbf{Ax } = \mathbf{0}
$$
$\mathbf{A}^T\mathbf{A}$最小特征值对应的特征向量即为解，即求得直线方程。求得直线方程后可求得每一个$\theta$所映射的点与直线的距离（沿弧线）：
$$
\mathcal{L}_i = \mathop{\arg\min}_{\theta_{\text{new}\in\{\theta_{\text{new1}}, \theta_{\text{new2}}\}}} (\theta_{\text{new}} - \theta_{\text{old}}),
$$
$$
\{\theta_{\text{new1}}, \theta_{\text{new2}}\} = \arcsin(\frac{2bp\pm\sqrt{4a^2(a^2+b^2-p^2)}}{2(a^2+b^2)}),
$$
$$
p = \frac{d-cr\sin(\phi)}{r\cos(\phi)}.
$$
$\mathcal{L}$的下标表示第几条直线。最终
$$
\mathcal{L} = \frac{1}{N_{\text{L}}}\sum^{N_{\text{L}}}_{i=1}\mathcal{L}_i
$$
实操发现一轮更新即陷入局点。故改为优化直线方程参数。

设直线$L_i, i\in\{1, 2, ..., N_{\text{L}} \}$的参数为$a_i, b_i, c_i, d_i$，其对应的距离采样为 $\mathbf{r}_i$，则对应的角度为：
$$
\sin{\mathbf{\theta}_i} = \min(\frac{2b_ip_i\pm\sqrt{4a_i^2(a_i^2+b_i^2-p_i^2)}}{2(a_i^2+b_i^2)}).
$$
由于通常$-\frac{\pi}{2} < \theta < \frac{\pi}{2}$，故用$\sin{\theta}$代替$\theta$可行。因为同一款声呐的$\theta$一致，则有：
$$
\mathcal{L} =\mathop{\arg\min}_{a_1, b_1,c_1,d_1,...,a_{N_\text{L}}, b_{N_\text{L}},c_{N_\text{L}},d_{N_\text{L}}}
\sum_{i=1}^{N_\text{L}} (\sin\theta_i - \mu)^2.
$$
其中，$\mu = \frac{1}{N_\text{L}}\sum_{i=1}^{N_\text{L}}\sin\theta_i$ 。




