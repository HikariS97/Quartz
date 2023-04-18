[[2018 IROS Scan Context --- Egocentric Spatial Descriptor for Place Recognition within 3D Point Cloud Map.pdf]]
核心思想：
1. 获取***scan context***。在声呐、雷达的情况下，本身获得的就是 ***angle-range*** 的图片，只需要 ***max-downsample*** 即可。文章对点云做了一些扰动，获得更多的 ***scan context*** 增强鲁棒性；
2. ***colunm-wise*** 比对然后相加。认为只有 colunm-shift，而 row-order 基本不变。（不太站得住脚。）利用 colunm-shifted-scan-context 暴力得到最相近的分数，来判断是否回环。（不如用 FFT？）
3. two-phase detection
	1. 提取 ring-key，方法是每一个 ring 的占空比。其是 rotational-invariant。
	2. 再暴力搜索