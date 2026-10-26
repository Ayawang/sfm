1. 矩阵：通过线性变换，把一个向量变成另一个向量

2. 用 $x = \begin{bmatrix} x & y & 1 \end{bmatrix}^T$来表示点

3. 用 $l = \begin{bmatrix} a & b & c \end{bmatrix}^T$来表示直线

4. 当$x^T l = 0$ 时，就说明**这个点在这条直线上**。

5. 正交矩阵$A^T=A^{-1}$，正交矩阵一定是方阵。

6. 对于任意向量 $x$，其欧氏范数的平方可以写成自身与自身的内积，即：$$\Vert{}x\Vert{}^2 = x^T x$$

7. 意一个实数正交矩阵 $R$，其行列式的值必然只能是 $+1$ 或 $-1$

8. 按照右手坐标系，绕x轴、y轴和z轴逆时针旋转角度$\theta$的标准旋转矩阵，旋转矩阵一定是正交矩阵

   ①绕 $x$ 轴旋转：

$$R_x(\theta) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{bmatrix}$$

②绕 $y$ 轴旋转

$$R_y(\theta) = \begin{bmatrix} \cos\theta & 0 & \sin\theta \\ 0 & 1 & 0 \\ -\sin\theta & 0 & \cos\theta \end{bmatrix}$$

③绕 $z$ 轴旋转

$$R_z(\theta) = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

9.对于旋转矩阵 $$U$$(主要是行列式为1的正交矩阵)，叉乘满足：

$$U(a\times b)=(Ua)\times(Ub)$$

也就是说：先叉乘再整体旋转，与先旋转两个向量再叉乘，结果相同。

一般的矩阵：

$$(Ua) \times (Ub) = (\det U) U^{-T} (a \times b)$$

①当 **$\det U = +1$** 时，它代表**纯旋转**。

②当 **$\det U = -1$** 时，它代表**旋转加镜像反射**。

10.矩阵乘以标准基向量，本质上就是在提取矩阵的某一个列。

11.对于旋转矩阵 $U$，叉乘满足：

$$U(a\times b)=(Ua)\times(Ub)$$

也就是说：先叉乘再整体旋转，与先旋转两个向量再叉乘，结果相同。

12. 点 $(x_0, y_0)$ 到直线 $Ax + By + C = 0$ 的距离 $d$ 的计算公式为：

    $$d = \frac{\vert{}Ax_0 + By_0 + C\vert{}}{\sqrt{A^2 + B^2}}$$

13. 最小二乘法：

    ① 齐次方程 ($Az = 0$)**：用 SVD 找**最小奇异值对应的右奇异向量（如八点法）。

    ② 非齐次方程 ($Bz = t$)**：用 SVD 计算矩阵的**伪逆来得到最小二乘解，而不是直接取最小奇异向量。
