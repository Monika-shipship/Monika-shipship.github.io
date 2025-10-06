---
tags: zhihu
zhihu-title: 2025年兰州按到学引力波数据分析暑期学校Lecture 1 Part 1翻译
zhihu-topics: 广义相对论
zhihu-link: https://zhuanlan.zhihu.com/p/1950710843642708253
zhihu-created-at: 2025-09-15 00:02
zhihu-updated-at: 2025-09-19 23:46
---

# 目录

- 连续（模拟 Analog time），离散时间（discrete time）和数字信号
- 连续傅立叶变换和信号频谱图
	- 离散傅立叶变换 DFT（快速傅立叶变换 FFT）
- 连续卷积和傅立叶变换的关系
	- 离散时间卷积和傅立叶变换的关系
	- 窗口化
- 滤波：用卷积将信号频谱图调整到我们想要的形状
	- 离散时间滤波
- Nyquist 采样定理（频率至少到多少才不会失真）

# 基本信号过程

## 什么是信号？

![[signal_time.jpg| $S(t)$ 含时函数]]

![[4073095a0fa3553eb3e6369e6d920b79.jpg|$S(x,y)$ 空间的函数，如一张图片]]

- 信号就是随时间或空间变换的函数所携带的信息
	- 比如 $S(t)$ 含时函数
	- $S(x,y)$ 空间的函数，如一张图片
- 信号处理：对信号进行数学运算以转换其中包含的信息
- 例子
	- 去噪：从信号中去除噪声
	- 滤波：强调或减弱某些信息

## Analog vs. Digital 连续（模拟）和离散（数字）

![[analogVSdigital.png]]

- 模拟信号 Analog signal：时间的连续函数 $r(t)$
	- 例：如传到你耳朵里的声音信号
- 离散时间信号 Discrete time signal：一系列从模拟信号中采样得到的值
	- $s[k]=s(t_{k})\to\text{表示为:}{s[k],k=\dots,-1,-2,0,1,2,\dots}$
- 数字信号 Digital signal：$s[k]$ 由于二进制系统中不能精确地表示信号而被*量子化*（有人翻译成量化，*quantized*，连续的信号被拆成离散的）
	- $5\to 101=1 \times 2^{2}+0\times 2^{1} +1\times 2^{0}$
	- $7\to 111 \text{但} 8\to ?,6.3\to ?$
	- 例子：数字音乐文件
	- 由于现代计算机中常用双精度（=64 位）数字表示，量化影响在很大程度上可以忽略
	- 量子化在需要快速数据采集的应用中非常重要
- 离散时间信号（Discrete time signal）
	- 定义：离散时间信号也称为时间序列
	- 表示（Notation）: 
		$\mathbf{x} \quad or\quad  \tilde{x}=(x[0],x[1], \ldots,x[N-1]) \quad or \quad \bar{x}=(x_{0},x_{1}, \ldots,x_{N-1})$ 
		$x[n]=x(t_{n})$
- 均匀采样时间序列: 引力波数据在大多数情况下（PTA 除外）由等时间间隔的样本组成
- 每秒的样本数：采样频率（Hz）
	- 激光干涉引力波天文台（LIGO）的数据采样频率通常为 16384Hz（= $2^{14}$ 样本/s）
	- 激光干涉空间天线（LISA）的采样频率将约为 2 赫兹
	- 脉冲星计时阵列（PTA）的数据采样频率平均约为 $7\times 10^{-7}$ Hz

# Analog Fourier transform 连续傅立叶变换

- 定义：一种用较简单的信号来表示复杂信号的方法
- 对于一个连续信号（analog signal）有 $\tilde{s}(f)=F[s(t)]=\int_{-\infty}^{\infty} s(t)e^{-2\pi i f t} \mathrm{d}t$
- 傅立叶变换是*可逆的*（invertible），其逆傅立叶变换是 $s(t)=F^{-1}[\tilde{s}(f)]=\int_{-\infty}^{\infty} \tilde{s}(f)e^{2\pi i ft} \mathrm{d}f$
- 线性（Linearity）：$f[s(t)+g(t)]=F[s(t)]+F[g(t)]=\tilde{s}(f)+\tilde{g}(f)$
- 厄米性 (Hermiticity property): 对于一个实函数 $s(t),i.e.,s^{*}(t)=s(t)$ 可以推导
$$
\begin{align}
s^{*}(t)=\int_{-\infty}^{\infty} \tilde{s^{*}}(f)e^{-2 \pi ift} \mathrm{d}f &= \int_{-\infty}^{\infty} \tilde{s^{*}}(-f)e^{2 \pi ift} \mathrm{d}f \\
 s(t)&=\int_{-\infty}^{\infty} \tilde{s}(f)e^{2 \pi ift} \mathrm{d}f
\end{align}
$$
$$
\tilde{s}(-f)=\tilde{s^{*}}(f)\implies \begin{cases}
Re[\tilde{s}(f)]\to \text{偶函数 Even}  \\
Im[\tilde{s}(f)]\to \text{奇函数 Odd}
\end{cases}
$$

- 下面来改写这个式子
$$
\begin{align*}
s(t)&=\int_{-\infty}^{\infty}\tilde{s}(f)e^{2\pi ift}df=\int_{-\infty}^{\infty}\left(\underset{\text{even}}{a(f)}+i\underset{\text{odd}}{b(f)}\right)e^{2\pi ift}df\\
&=\int_{-\infty}^{\infty}\left[\underset{\text{even}\times\text{even}=\text{even}}{(a(f)\cos(2\pi ft))}-\underset{\text{odd}\times\text{odd}=\text{even}}{(b(f)\sin(2\pi ft))}+i\left(\underset{\text{even}\times\text{odd}=\text{odd}}{(a(f)\sin(2\pi ft))}+\underset{\text{odd}\times\text{even}=\text{odd}}{(b(f)\cos(2\pi ft))}\right)\right]df\\
&=2\int_{0}^{\infty}\sqrt{a^{2}(f)+b^{2}(f)}\cos\left(2\pi ft+\tan^{-1}\frac{b(f)}{a(f)}\right)df
\end{align*}
$$

 虚部是因为奇函数，从负无穷积到正无穷抵消掉了

- 从这个式子可以看出，傅里叶变换的意义在于：*任何函数* $s(t)$ 都可以表示为一系列频率范围内的正弦曲线（即形如 $A(f)\cos(2\pi ft+\Phi (f))$ 的正弦函数）的叠加（"superposition"）。
- 任何函数！即使是非周期性的也可以做傅立叶变换，只要认为他的周期是无穷大就可以了
- 信号的傅里叶变换称为其**频谱 Spectrum**，将信号分解为正弦波的叠加则称为**频谱分解 Spectral decomposition**。
如果你是较真的数学人，会说这样的结果是收敛的，那好吧，换种说法，为了使这个积分有一个合理的结果，物理人发明了一种叫柯西主值的东西，是这样定义的
$$
P[\int_{-\infty}^{+\infty} f(x) \mathrm{d}x]=\lim_{ A \to \infty }  \int_{-A}^{A} f(x) \mathrm{d}x
$$

这样，如果 $f(x)$ 是奇函数，这个积分就有收敛的主值 0

## 正弦函数的傅立叶变换

- 如果 $s(t) = A \sin(2\pi f_0 t+\phi)$, 它的傅立叶变换是
$$
\int_{-\infty}^{\infty}A \sin(2\pi f_0 t+\phi)e^{-2\pi i f t}dt=\frac{1}{2i\cdot(2\pi i)}\left[A\delta(f - f_0)e^{i\phi}+A\delta(f + f_0)e^{-i\phi}\right]
$$

上式中利用了

$$
\sin(x)=\frac{1}{2i}(e^{ix}-e^{-ix})
$$

可以很容易地看出，正弦函数进行傅立叶变换后，结果是一个纯虚数

和 $\delta(x)$ 是狄拉克 delta 函数 (Dirac-delta function) 

$$
\delta(x)=\int_{-\infty}^{\infty}1 \cdot dy\ e^{-2\pi i y x}=\begin{cases}\infty; & x = 0\\0; & \text{otherwise}\end{cases}
$$
$$
\int_{-\infty}^{\infty}dx\ f(x)\delta(x - x_0)=f(x_0)
$$
- 正弦波函数的频率分布是 -- 只有他自身的频率，所以傅立叶变换后是 delta 函数，他自身的频率占据了所有频率分布

狄拉克 $\delta$ 函数的示意图是一个箭头. 箭头的高度代表其积分值（即面积）. 有的示意图直接将面积值标在箭头旁边

## 信号频谱图 Signal Spectrum

信号做连续傅立叶变换后，取其绝对值，其和频率的关系就称为频谱图（Spectrum），记作 $|fft|-f$

有的书里的 Spectrum 是指 $|fft|^2-f$

但是这里有一个很操蛋的点，直接对一组数据做 fft （快速傅立叶变换，对于离散数据才有），我们得到的 fft 所对应的频率是正频率 -0 频率（直流）- 负频率的，这个留到离散傅立叶变换时讲，也会讲 matlab 中的 fftshift 函数是干什么的

## 傅立叶变换的例子

- 信号是两个正弦函数之和
- $|fft|-t$ 傅立叶变换的绝对值 - 频率图显示出四个峰（对应于两个正弦函数的频率的正负值），是偶对称的
- $Re(fft)-t$ 图应该恒为零，因为正弦函数的傅立叶变换是纯虚数
- $Im(fft)-t$ 图应该是奇对称的，因为我们之前分析过 $$
\begin{cases}
Re[\tilde{s}(f)]\to \text{偶函数 Even}  \\
Im[\tilde{s}(f)]\to \text{奇函数 Odd}
\end{cases}
$$
- 解决几个正弦函数的和时傅立叶变换很方便
- （如果你使用 matlab 想画出这个图像，请了解 fftshift 函数，他会自动将零频率即直流分量平移到中间去）
![[信号时域图f1f2.png|f1＝2hz,f2=5hz]]
![[fftDraw.png]]
# 离散傅立叶变换 Discrete Fourier Transform (Fast Fourier Transform)
## 序列 Sequences
![[signal_time 1.jpg]]
在计算机上，一次只能处理有限数量的样本
记做 $\bar{x}=(x_{0},x_{1},x_{2}, \ldots,x_{N-1})$ 
我们考虑一个均匀采样的信号，所以 $\implies t_{k}=k \Delta,\Delta:\text{采样频率}$
对于 $N$ 个样本，当 $N$ 很大时， 总时间间隔 $T=(N-1)\Delta \approx N \Delta$ 
#  卷积 Convolution (“Faltung”)
调整信号的频率成分
## 离散傅立叶变换(DFT) Discrete Fourier Transform
DFT：
$$
\tilde{x}_k = \sum_{n=0}^{N - 1} x_n e^{-2\pi i k / N}
$$  $k = 0, 1, \ldots, N - 1$ 
- DFT 是一个序列: $(\tilde{x}_0, \tilde{x}_1, \ldots, \tilde{x}_{N - 1})$ 
   $\tilde{x}(f) = F[x(t)] = \int_{-\infty}^{\infty} x(t) e^{-2\pi i} dt$ 
逆 DFT： 
$$
x_n = \frac{1}{N} \sum_{k=0}^{N - 1} \tilde{x}_k e^{2\pi i n k / N}
$$  $n = 0, 1, \ldots, N - 1$ 

 $x(t) = F^{-1}[\tilde{x}(f)] = \int_{-\infty}^{\infty} \tilde{x}(f) e^{2\pi i f t} df$

傅立叶变换是处理连续信号的，对于离散的信号应该怎么处理呢？

将傅立叶变换离散化就行了

对于一个有限长的离散时间信号，他对应的变换就是傅立叶变换的离散化版本

下面我们试试推导其具体形式：

DFT 可以看作是连续傅立叶变换的一种近似

- $x(t) = 0$ for $t \notin [0,T] \Rightarrow$
$$

\tilde{x}(f)=\int_{-\infty}^{\infty} x(t) e^{-2 \pi i f t} d t=\int_{0}^{T} x(t) e^{-2 \pi i f t} d t$$ - 令 $\Delta$ 为采样间隔 (即相邻两信号点的时间差)，且 $T \approx N \Delta$ 是信号的总时长

- 采样时间: $t_{n}=n \Delta$
- 将时间 $t$ 代换成 $n \Delta$ ，$\mathrm{d}t=t_{n}-t_{n-1}=\Delta$，积分化为求和
$$

\tilde{x}(f)=\int_{0}^{T} x(t) e^{-2 \pi i f t} d t \approx \Delta \sum_{n = 0}^{N - 1} x\left(t_{n}\right) e^{-2 \pi i f t_{n}}=\Delta \sum_{n = 0}^{N - 1} x_{n} e^{-2 \pi i f(n \Delta)}

$$
 - 令 $f_{k}=\frac{k}{N \Delta}=\frac{k}{T}$
$$

\frac{\tilde{x}\left(f_{k}\right)}{\Delta} \approx \sum_{n = 0}^{N - 1} x_{n} e^{-\frac{2 \pi i k n}{N}}=\tilde{x}_{k}

$$ - $\Rightarrow$ DFT 是 FT 在 $f_{k}$ 上的的近似 $k = 0,1, \ldots, N - 1$
