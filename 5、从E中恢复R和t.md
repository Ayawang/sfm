# 从特征匹配到本质矩阵：从E中恢复R和t

汇总：已知本征矩阵（Essential Matrix）$E$，要求解对应的旋转矩阵 $R$ 和平移向量 $t$（注意：平移向量 $t$ 只能恢复到绝对尺度之外的方向，即 $t$ 的模长无法确定，通常将其归一化为单位向量），标准的方法是使用**奇异值分解（SVD）**。

## 1. 对 $$E$$做 SVD，并统一尺度。

### 1.1 $\Sigma$归一化

我们现在已经得到了前两个奇异值相同，最后一个为0的本质矩阵E：

$$E=U
\begin{bmatrix}
s&0&0\\
0&s&0\\
0&0&0
\end{bmatrix}
V^T$$

因为整体尺度不重要，可以除以 \(s>0\)，使用：

$$\boxed{
\bar E=U
\underbrace{
\begin{bmatrix}
1&0&0\\
0&1&0\\
0&0&0
\end{bmatrix}
}_{D}
V^T
}$$

这里 $$\bar E$$ 与原来的 $$E$$ 表示相同的几何约束。

现在的公式是：

$$E = U \Sigma_Z V^T$$

### 1.2 U和V行列式取正

旋转矩阵 $R$ 的行列式必须为正（即 $\det(R) = +1$），所以在分解后，我们需要检查并确保：

$$\det(U) = +1 \quad \text{和} \quad \det(V) = +1$$

如果 $\det(U) = -1$，则将 $U$ 的所有列取反（同时将 $E$ 视为 $-E$）；

同理，如果 $\det(V) = -1$，则将 $V$ 的所有列取反。

这样可以保证后续算出的 $R$ 是一个真正的旋转矩阵，$R$ 是旋转矩阵，因此$R$ 的行列式为1

为什么不影响本质矩阵：

对 $U$ 或 $V$ 动手脚，等价于把整个本质矩阵 $E$ 变成了 $-E$（或者说，平移向量 $t$ 偷偷变号了）。由于对极约束方程 $\hat{x}_2^T E \hat{x}_1 = 0$ 对 $E$ 和 $-E$ 是一视同仁的（乘个负号结果依然是 0），

## 2. 推导出来$R$ 和 $t$ 的 2 个独立的解

###### 引入辅助矩阵 $W$ 和 $Z$

在三维空间中，我们定义两个固定的特殊矩阵：

$$W = \begin{bmatrix} 0 & -1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}, \quad Z = \begin{bmatrix} 0 & 1 & 0 \\ -1 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix}$$

其中：

- $W$ 是一个绕 $z$ 轴旋转 $90^\circ$ 的正交矩阵（满足 $W^T W = I$ 且 $\det(W) = +1$）。

- $Z$ 是向量 $e_3 = [0, 0, 1]^T$ 的反对称矩阵，即 $Z = [e_3]_\times$，即对任意向量 \(a\)：

  $$Za=e_3\times a$$

本质矩阵为：

$$E = U \Sigma_Z V^T$$

我们知道，本质矩阵的物理定义是：

$$E = [t]_\times R$$

现在，我们的目标就是找到满足 $U \Sigma_Z V^T = [t]_\times R$ 的所有解 $(t, R)$。

$$ZW^T=
\begin{bmatrix}
0&-1&0\\
1&0&0\\
0&0&0
\end{bmatrix}
\begin{bmatrix}
0&1&0\\
-1&0&0\\
0&0&1
\end{bmatrix}\begin{bmatrix}
1&0&0\\
0&1&0\\
0&0&0
\end{bmatrix}
=D$$

所以：

$$\boxed{D=ZW^T}$$

另外：

$$ZW=
\begin{bmatrix}
-1&0&0\\
0&-1&0\\
0&0&0
\end{bmatrix}
=-D$$

因此还有：

$$\boxed{D=(-Z)W}$$

#### 2.1第一组解推导：

$D=ZW^T$  $=>$  $\bar E=UDV^T$  $=>$ $\bar E=UZW^TV^T$ $=>$ $\bar E=UZ(U^TU)W^TV^T$

重新分组：

$$\boxed{
\bar E=(UZU^T)(UW^TV^T)
}$$

取任意向量 $$q$$，因为$$Z=[e_3]_\times$$，所以：

$$UZU^Tq=U(e_3\times U^Tq)$$

利用旋转矩阵叉乘的性质：

$$U(e_3\times U^Tq)=(Ue_3)\times(UU^Tq)$$

因此：

$$\boxed{UZU^T=[Ue_3]_\times}$$

而：

$$U=
\begin{bmatrix}
|&|&|\\
u_1&u_2&u_3\\
|&|&|
\end{bmatrix}$$

乘上 $$e_3$$就是在取第三列：

$$Ue_3=u_3$$

所以：

$$\boxed{UZU^T=[u_3]_\times}$$

代回前面的分解：

$$\boxed{
\bar E=[u_3]_\times(UW^TV^T)
}$$

<span style="background-color:#fff3c4">于是得到第一组解：</span>

$$\boxed{
t=u_3,\qquad R_1=UW^TV^T
}$$

$$u_3$$ 是单位向量，因此这里自动满足 $$|t\|=1$$。

#### 2.2 第二组解推导：

$D=(-Z)W$ $=>$ $\bar E=UDV^T$  $=>$ $\bar E=U(-Z)WV^T$ $=>$ $\bigl(U(-Z)U^T\bigr)(UWV^T)$

代回前面的分解：

$$\boxed{
\bar E=[-u_3]_\times(UWV^T)
}$$

<span style="background-color:#fff3c4">于是得到另一组解：</span>

$$\boxed{
t=-u_3,\qquad R_2=UWV^T
}$$

最终有四组候选：

| **编号** | **旋转矩阵 (R)**  | **平移向量 (t)** |
| -------- | ----------------- | ---------------- |
| **1**    | $R_1 = U W^T V^T$ | $+u_3$           |
| **2**    | $R_1 = U W^T V^T$ | $-u_3$           |
| **3**    | $R_2 = U W V^T$   | $+u_3$           |
| **4**    | $R_2 = U W V^T$   | $-u_3$           |

## 3. 从四组中选出真实的相机运动

增加一个实际成像条件：

> **被两台相机正常观测到的空间点，应当同时位于两台相机前方。**



取一对正确匹配的归一化坐标：

$$\hat{x}_1=
\begin{bmatrix}x_1\\y_1\\1\end{bmatrix},
\qquad
\hat{x}_2=
\begin{bmatrix}x_2\\y_2\\1\end{bmatrix}$$

因为图像点确定的是一条射线，三维点可以写成：

$$P_1=z_1\hat{x}_1,\qquad
P_2=z_2\hat{x}_2$$

代入：

$$P_2=RP_1+t$$

得到：

$$z_2\hat{x}_2=z_1R\hat{x}_1+t$$

移项：

$$-z_1R\hat{x}_1+z_2\hat{x}_2=t$$

写成线性方程：

$$\boxed{
\underbrace{
\begin{bmatrix}
-R\hat{x}_1&\hat{x}_2
\end{bmatrix}
}_{3\times2}
\underbrace{
\begin{bmatrix}
z_1\\z_2
\end{bmatrix}
}_{2\times1}\underbrace{t}_{3\times1}
}$$

对于每组候选 $$R,t$$，都可以解出这一对匹配点的深度；

然后检查：

$$\boxed{z_1>0,\qquad z_2>0}$$

如果某个深度为负，说明这组候选把空间点放到了某台相机后面，应当排除。

对 RANSAC 保留的<span style="background-color:#fde2e2">全部内点匹配</span>，分别在四组候选 \(R,t\) 下进行三角化，统计同时满足两台相机正深度条件的点数。选择支持数明显占优、且通过其他质量检查的候选。若没有可靠候选，则认为这次两视图初始化失败。

在理想和绝大多数实际情况下，通过正深度检验（Cheirality Check）后，**最终筛选出的正确解只有唯一的一对 $(R, t)$**。

## 4.引入三角化和pnp

1、把相机间距取为 1，指的是**初始化的那一对相机**，不是整个 SfM 流程中每一对相机都这样处理。

2、**增量式 SfM 的做法：只在初始化时选一次尺度。**

先用相机 1、2：

1. 从 $$E_{12}$$ 恢复 $$R_{12}$$ 和平移方向。
2. 令 $$|t_{12}\|=1$$，选定整个重建的长度单位。
3. 三角化，得到初始三维点。

此时得到的三维点和两台相机，已经处于**同一个坐标系、同一个尺度**中。

3、**加入相机 3 时，不再把它与相机 2 的距离强行设为 1。**

我们去找：

> 已经重建出来的三维点，在第 3 张图中对应哪些像素？

这样就获得一组 **三维点—二维像素对应关系**：

$$P_j\longleftrightarrow x_{3j}$$

利用这些对应关系，通过 **PnP** 求相机 3 的位姿：

$$P_{c_3}=R_3P_j+t_3$$

这里的三维点$$P_j$$已经具有初始化时选定的尺度。因此，求出的相机 3 的位置也必须沿用这个尺度，**平移长度不再被规定为 1**。

4、<span style="background-color:#fde2e2">现在选定的是所有相机和三维点共同参考的世界坐标系。</span>
