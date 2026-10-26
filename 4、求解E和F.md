# 从特征匹配到本质矩阵：求解E和F

学习目标：我们的核心任务就是：**利用已知的点对 $(\tilde{x_1}, \tilde{x_2})$，把F和E求解出来。**

## 1. 八点法核心思想求F

- 图1中的点：$x_1 = \begin{bmatrix} u_1 & v_1 & 1 \end{bmatrix}^T$

- 图2中的点：$x_2 = \begin{bmatrix} u_2 & v_2 & 1 \end{bmatrix}^T$

- $F$ 是一个 $3 \times 3$ 的矩阵，里面包含 9 个未知数：

$$F = \begin{bmatrix} f_1 & f_2 & f_3 \\ f_4 & f_5 & f_6 \\ f_7 & f_8 & f_9 \end{bmatrix}$$

根据对极约束公式：$$x_2^T F x_1 = 0$$

把具体的矩阵和向量代进去：

$$\begin{bmatrix} u_2 & v_2 & 1 \end{bmatrix} \begin{bmatrix} f_1 & f_2 & f_3 \\ f_4 & f_5 & f_6 \\ f_7 & f_8 & f_9 \end{bmatrix} \begin{bmatrix} u_1 \\ v_1 \\ 1 \end{bmatrix} = 0$$

$$u_2(f_1 u_1 + f_2 v_1 + f_3) + v_2(f_4 u_1 + f_5 v_1 + f_6) + 1(f_7 u_1 + f_8 v_1 + f_9) = 0$$

**得到关于 $f$ 的线性组合**：$u_2 u_1 f_1 + u_2 v_1 f_2 + u_2 f_3 + v_2 u_1 f_4 + v_2 v_1 f_5 + v_2 f_6 + u_1 f_7 + v_1 f_8 + f_9 = 0$

这就像是$$ax+by+c=0$$的直线方程，我们需要两个点才能求出来abc的值，对于这个f的线性组合，我们需要8个匹配点才能求解出来f1-f9的数值。

$$\underbrace{\begin{bmatrix} u_2u_1 & u_2v_1 & u_2 & v_2u_1 & v_2v_1 & v_2 & u_1 & v_1 & 1 \end{bmatrix}}_{\text{由已知像素点 } (x_1, x_2) \text{ 组成的行向量}} \underbrace{\begin{bmatrix} f_1 \\ f_2 \\ \vdots \\ f_9 \end{bmatrix}}_{f} = 0$$

所以我们要找的是：

$$\boxed{Af=0\text{ 的非零解}}$$

在现实世界中，图像提取的特征点坐标总会带有**噪声**（比如像素有误差）。这意味着完美的 $Af = 0$ 往往**无解**（找不到一个非零的 $f$ 让等式绝对等于 0）。

### 1.1引入使用svd分解求解最小二乘解

于是目标改成：

$$\boxed{
\min_{\|f\|=1}\|Af\|^2
}$$

它的意思是：

> 在所有长度为 1 (限制向量长度 $\Vert{}f\Vert{} = 1$)的候选中，求一个**最小二乘解**，因此我们引入了奇异值分解。

将矩阵$$A$$进行SVD分解

数学原理**：在线性代数中，对于齐次线性方程组 $Af = 0$（在最小二乘意义下），使 $\Vert{}Af\Vert{}$ 最小且 $\Vert{}f\Vert{}=1$ 的最优解 $f$，正好对应着矩阵 $V$ 中**最小奇异值（即 $\Sigma$ 对角线最后一个元素）所对应的那一列向量。

### 1.2 对这个 $$A$$ 做完整 SVD，$$f$$是多大？

当我们有N对匹配点，把 $$N$$ 对匹配点产生的行向量叠起来：

$$A=
\begin{bmatrix}
a_1^T\\
a_2^T\\
\vdots\\
a_N^T
\end{bmatrix}$$

因此：

$$\boxed{A\text{ 的大小为 }N\times9}$$

点对增加，只增加方程数量，不增加未知数数量。未知数始终是 $$F$$的九个元素。

尺寸对应为：

$$\underbrace{A}_{N\times9}
\underbrace{f}_{9\times1}\underbrace{Af}_{N\times1}$$

其中，$$Af$$的每个分量都是一对匹配点的约束残差：

$$Af=
\begin{bmatrix}
x_2^{(1)T}Fx_1^{(1)}\\
x_2^{(2)T}Fx_1^{(2)}\\
\vdots\\
x_2^{(N)T}Fx_1^{(N)}
\end{bmatrix}$$

已知矩阵 $A$ 的奇异值分解形式为：  

$$A = U \Sigma V^T$$

对于一个具有 $N$ 行和 9 列的矩阵 $A$（即大小为 $N \times 9$），其完整 SVD 的尺寸展开为：

$$\underbrace{A}_{N\times9} = \underbrace{U}_{N\times N} \underbrace{\Sigma}_{N\times9} \underbrace{V^T}_{9\times9}$$

因此：

$$\boxed{V \text{ 的大小为 } 9 \times 9}$$

若将矩阵 $V$ 按列向量展开：  

$$V = \begin{bmatrix} \vert{} & \vert{} & & \vert{} \\ v_1 & v_2 & \cdots & v_9 \\ \vert{} & \vert{} & & \vert{} \end{bmatrix}$$

每一列向量 $v_i$ 都有 9 个元素：  

$$\boxed{v_i \text{ 的大小为 } 9 \times 1}$$

特别是矩阵 $V$ 的最后一列 $v_9$：

$$v_9 = \begin{bmatrix} V_{1,9} \\ V_{2,9} \\ V_{3,9} \\ V_{4,9} \\ V_{5,9} \\ V_{6,9} \\ V_{7,9} \\ V_{8,9} \\ V_{9,9} \end{bmatrix}$$

通过上述尺寸推导可以看出，它的大小正好与我们要寻找的未知向量 $f$ 相同（均为 $9 \times 1$）。

### 1.3 为什么现在求得的$$f$$不是最终的$$F$$

先按照之前的3x3的顺序重新排列形状：

$$\boxed{
F_0=
\begin{bmatrix}
V_{1,9}&V_{2,9}&V_{3,9}\\
V_{4,9}&V_{5,9}&V_{6,9}\\
V_{7,9}&V_{8,9}&V_{9,9}
\end{bmatrix}
}$$

目前的求解只考虑了匹配点产生的线性约束，还没有保证：

$$\operatorname{rank}(F)=2$$

因此需要对于$$F_0$$做第二次SVD分解：

因此，再对 $$F_0$$ 做一次 SVD：

$$F_0=U_F
\begin{bmatrix}
s_1&0&0\\
0&s_2&0\\
0&0&s_3
\end{bmatrix}
V_F^T$$

将最小奇异值$$s_3$$ 设成零：

$$\boxed{
F=U_F
\begin{bmatrix}
s_1&0&0\\
0&s_2&0\\
0&0&0
\end{bmatrix}
V_F^T
}$$

这样得到的F就是最终的F。

## 2. 把基础矩阵 $$F $$ 转换成本质矩阵 $$E$$

我们从第三章极几何中可以得到公式：

$$\boxed{
F=K_2^{-T}EK_1^{-1}
}$$

那么

$$\boxed{E=K_2^TFK_1}$$

如果两张图片具有相同的内参 \(K\)，公式简化为：

$$\boxed{E_0=K^TFK}$$

现在得到的E已经满足秩2，但是E还要满足有两个非奇异值相等，因此还要对$$E_0$$做SVD：

$$E_0=U_E\operatorname{diag}(s_1,s_2,s_3)V_E^T$$

取前两个奇异值的平均：

$$s=\frac{s_1+s_2}{2}$$

重构这样就得到了E：

$$\boxed{
E=U_E
\begin{bmatrix}
s&0&0\\
0&s&0\\
0&0&0
\end{bmatrix}
V_E^T
}$$
