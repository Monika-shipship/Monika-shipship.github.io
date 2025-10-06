### 问题一：两种 SNR 定义的一致性与合理性分析


#### 1. 两种定义的形式回顾
- **通用检测统计量的 SNR**：对于任意检测统计量 $\Gamma(\bar{y})$，其 SNR 定义为信号存在时统计量的均值与信号不存在时统计量的标准差之比：
  $$\text{SNR} = \frac{E\left[\Gamma \mid H_1\right]}{\sqrt{\text{var}\left(\Gamma \mid H_0\right)}}$$
- **LR 检验的 SNR（振幅归一化中的 $A$）**：对于高斯噪声下的似然比（LR）检验，信号向量 $\vec{s}(\theta)$ 的**Mahalanobis 范数**（由噪声协方差矩阵 $\mathbf{C}$ 定义）即为其SNR：
  $$A = \|\vec{s}(\theta)\| = \sqrt{\langle \vec{s}(\theta), \vec{s}(\theta) \rangle} = \sqrt{\vec{s}(\theta)^T \mathbf{C}^{-1} \vec{s}(\theta)}$$


#### 2. 两种定义的一致性证明
两种定义的一致性**仅针对 LR 检验这一特定情况**，核心逻辑是：**LR 统计量的 SNR（按通用定义计算）恰好等于信号向量的 Mahalanobis 范数**。以下是严格推导：

对于高斯噪声下的 LR 检验，数据模型为：
$$\bar{y} = 
\begin{cases} 
\bar{n} & (H_0, \text{纯噪声}) \\
\vec{s} + \bar{n} & (H_1, \text{信号+噪声})
\end{cases}$$
其中 $\bar{n}$ 是零均值高斯噪声，协方差矩阵为 $\mathbf{C}$。

LR 统计量（对数形式，忽略常数项）为：
$$\Gamma_{\text{LR}}(\bar{y}) = \langle \bar{y}, \vec{s} \rangle = \bar{y}^T \mathbf{C}^{-1} \vec{s}$$

**步骤 1：计算 $E\left[\Gamma_{\text{LR}} \mid H_1\right]$**
当 $H_1$ 为真时，$\bar{y} = \vec{s} + \bar{n}$，因此：
$$E\left[\Gamma_{\text{LR}} \mid H_1\right] = E\left[(\vec{s} + \bar{n})^T \mathbf{C}^{-1} \vec{s}\right] = \vec{s}^T \mathbf{C}^{-1} \vec{s} + E\left[\bar{n}^T \mathbf{C}^{-1} \vec{s}\right]$$
由于 $\bar{n}$ 零均值，第二项为 0，故：
$$E\left[\Gamma_{\text{LR}} \mid H_1\right] = \langle \vec{s}, \vec{s} \rangle = \|\vec{s}\|^2$$

**步骤 2：计算 $\text{var}\left(\Gamma_{\text{LR}} \mid H_0\right)$**
当 $H_0$ 为真时，$\bar{y} = \bar{n}$，因此：
$$\text{var}\left(\Gamma_{\text{LR}} \mid H_0\right) = \text{var}\left(\bar{n}^T \mathbf{C}^{-1} \vec{s}\right) = E\left[(\bar{n}^T \mathbf{C}^{-1} \vec{s})^2\right] - \left(E\left[\bar{n}^T \mathbf{C}^{-1} \vec{s}\right]\right)^2$$
由于 $\bar{n}$ 零均值，第二项为 0；展开第一项：
$$E\left[(\bar{n}^T \mathbf{C}^{-1} \vec{s})^2\right] = E\left[\vec{s}^T \mathbf{C}^{-1} \bar{n} \bar{n}^T \mathbf{C}^{-1} \vec{s}\right] = \vec{s}^T \mathbf{C}^{-1} E\left[\bar{n} \bar{n}^T\right] \mathbf{C}^{-1} \vec{s}$$
而 $E\left[\bar{n} \bar{n}^T\right] = \mathbf{C}$（噪声协方差矩阵定义），因此：
$$\text{var}\left(\Gamma_{\text{LR}} \mid H_0\right) = \vec{s}^T \mathbf{C}^{-1} \mathbf{C} \mathbf{C}^{-1} \vec{s} = \vec{s}^T \mathbf{C}^{-1} \vec{s} = \|\vec{s}\|^2$$

**步骤 3：计算 LR 统计量的 SNR**
将上述结果代入通用 SNR 定义：
$$\text{SNR}_{\text{LR}} = \frac{E\left[\Gamma_{\text{LR}} \mid H_1\right]}{\sqrt{\text{var}\left(\Gamma_{\text{LR}} \mid H_0\right)}} = \frac{\|\vec{s}\|^2}{\sqrt{\|\vec{s}\|^2}} = \|\vec{s}\| = A$$

这直接证明：**LR 检验的 SNR（按通用定义）等于信号向量的 Mahalanobis 范数 $A$**。


#### 3. 为何都称为“信噪比”？
两种定义的核心一致：**衡量“信号对统计量的贡献”与“噪声对统计量的干扰”的相对强度**。
- 通用定义中，$E\left[\Gamma \mid H_1\right]$ 是信号存在时统计量的“均值偏移”（信号的贡献），$\sqrt{\text{var}\left(\Gamma \mid H_0\right)}$ 是信号不存在时统计量的“波动幅度”（噪声的干扰），比值越大，信号越易从噪声中区分。
- LR 检验的定义中，$A = \|\vec{s}\|$ 本质是“信号能量与噪声能量的比值的平方根”（Mahalanobis 范数已融入噪声协方差的加权），与通用定义的物理意义完全一致——均反映信号相对于噪声的“可检测性”。


### 问题二：平稳高斯噪声下内积的频域形式解析


#### 1. 公式的作用
该公式的核心是**将时域中带噪声协方差加权的内积转换为频域运算**，解决了平稳噪声下大规模数据的高效计算问题。具体来说，时域内积 $\langle \bar{x}, \bar{y} \rangle = \bar{x}^T \mathbf{C}^{-1} \bar{y}$（$\mathbf{C}$ 为噪声协方差矩阵），在频域可表示为：
$$\langle \bar{x}, \bar{y} \rangle = \frac{\Delta}{N} \sum_{k=0}^{N-1} \tilde{x}_k \cdot \frac{\tilde{y}_k^\dagger}{S_n(f_k)}$$
其中 $\tilde{x} = F\bar{x}$（$F$ 为 DFT 矩阵），$\tilde{y}^\dagger$ 是 $\tilde{y}$ 的共轭转置，$S_n(f)$ 是噪声的功率谱密度（PSD），$\Delta$ 为采样间隔，$N$ 为样本数。


#### 2. 为何内积形式发生变化？
变化的本质是**平稳噪声的协方差矩阵可通过 DFT 对角化**，从而将时域的矩阵乘法转换为频域的逐元素运算（复杂度从 $O(N^2)$ 降至 $O(N\log N)$，依赖 FFT）。具体逻辑如下：

- **平稳噪声的协方差矩阵特性**：平稳噪声的自相关函数仅与时间差有关，即 $R_n(m) = E[n_k n_{k+m}]$，因此协方差矩阵 $\mathbf{C}$ 是**Toeplitz 矩阵**（对角线元素相同）。
- **循环近似与 DFT 对角化**：当样本数 $N$ 很大时，Toeplitz 矩阵可近似为**循环矩阵**（通过“循环扩展”消除边界效应）。循环矩阵的 DFT 是对角矩阵，即：
  $$\mathbf{C} \approx F^\dagger \Lambda F$$
  其中 $\Lambda$ 是对角矩阵，对角元素 $\Lambda_{kk} = N\Delta S_n(f_k)$（$f_k = k/(N\Delta)$ 为频率点），$F$ 是 DFT 矩阵，$F^\dagger$ 是其共轭转置。
- **协方差矩阵的逆**：循环矩阵的逆仍为循环矩阵，且：
  $$\mathbf{C}^{-1} \approx F^\dagger \Lambda^{-1} F = F^\dagger \cdot \text{diag}\left(\frac{1}{N\Delta S_n(f_k)}\right) \cdot F$$

将 $\mathbf{C}^{-1}$ 代入时域内积：
$$\langle \bar{x}, \bar{y} \rangle = \bar{x}^T \mathbf{C}^{-1} \bar{y} \approx \bar{x}^T F^\dagger \cdot \text{diag}\left(\frac{1}{N\Delta S_n(f_k)}\right) \cdot F \bar{y}$$

利用 DFT 的酉性质（$F^\dagger F = I$，且 $\bar{x}^T F^\dagger = (F\bar{x})^\dagger = \tilde{x}^\dagger$），上式可简化为：
$$\langle \bar{x}, \bar{y} \rangle \approx \frac{1}{N\Delta} \tilde{x}^\dagger \cdot \text{diag}\left(\frac{1}{S_n(f_k)}\right) \cdot \tilde{y}$$

若 $\bar{x}$ 和 $\bar{y}$ 为实向量，则 $\tilde{x}^\dagger = \tilde{x}^*$（共轭），逐元素相乘后求和即得题目中的形式：
$$\langle \bar{x}, \bar{y} \rangle = \frac{\Delta}{N} \sum_{k=0}^{N-1} \tilde{x}_k \cdot \frac{\tilde{y}_k^*}{S_n(f_k)}$$


#### 3. 符号含义解析
- **$S_n(f)$**：噪声的功率谱密度（PSD），描述噪声在不同频率上的能量分布。由维纳-辛钦定理，$S_n(f)$ 是噪声自相关函数 $R_n(m)$ 的傅里叶变换，即 $R_n(m) = \int_{-1/(2\Delta)}^{1/(2\Delta)} S_n(f) e^{i2\pi f m\Delta} df$。
- **$N$**：时域样本数，总时长 $T = N\Delta$。
- **$\Delta$**：采样间隔（相邻两个样本的时间差），决定频率分辨率 $\Delta f = 1/(N\Delta)$。
- **$\tilde{x} = F\bar{x}$**：$\bar{x}$ 的离散傅里叶变换（DFT），将时域信号转换为频域表示。
- **$\tilde{y}^\dagger$**：$\tilde{y}$ 的共轭转置，对应时域内积中的共轭操作（实向量下等价于 $\tilde{y}^*$）。
- **“./”**：逐元素除法，即频域中每个频率点的 $\tilde{y}$ 分量除以对应的 $S_n(f_k)$（实现噪声协方差的逆加权）。


### 总结
1. **SNR 定义的一致性**：通用定义是检测统计量的“信号均值-噪声标准差”比，LR 检验的定义是信号向量的 Mahalanobis 范数，两者对 LR 检验完全等价，均反映信号相对于噪声的可检测性。
2. **频域内积的意义**：通过 DFT 将平稳噪声的协方差矩阵对角化，将时域的高复杂度矩阵运算转换为频域的低复杂度逐元素运算，是引力波检测等大规模平稳数据处理的核心工具。