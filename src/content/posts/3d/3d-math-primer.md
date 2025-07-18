---
title: 3D-Math-Primer
published: 2025-04-30
description: 3D-Math-Primer纯英文阅读笔记
tags:
  - 3d
  - math

category: math
draft: false
image: '/violet.png'
---

# 矩阵
## 矩阵变换的几何本质解析

### 核心发现：矩阵行向量与基变换的关系

当标准基向量 **i**=[1,0,0], **j**=[0,1,0], **k**=[0,0,1] 右乘任意矩阵 **M** 时：

$$
\begin{aligned}
iM &= \begin{bmatrix}1&0&0\end{bmatrix}M = \begin{bmatrix}m_{11}&m_{12}&m_{13}\end{bmatrix} \\
jM &= \begin{bmatrix}0&1&0\end{bmatrix}M = \begin{bmatrix}m_{21}&m_{22}&m_{23}\end{bmatrix} \\
kM &= \begin{bmatrix}0&0&1\end{bmatrix}M = \begin{bmatrix}m_{31}&m_{32}&m_{33}\end{bmatrix}
\end{aligned}
$$
这个计算揭示了一个关键结论：**矩阵的每一行就是标准基向量经过变换后的新坐标**。这意味着：

1. 第一行 = 变换后的 **i** 向量
2. 第二行 = 变换后的 **j** 向量
3. 第三行 = 变换后的 **k** 向量

### 任意向量的变换推导

对于任意向量 **v**=vₓ**i**+vᵧ**j**+v_z**k**，其变换结果为：
$$
vM = v_x(iM) + v_y(jM) + v_z(kM) = v_x\begin{bmatrix}m_{11}&m_{12}&m_{13}\end{bmatrix} + v_y\begin{bmatrix}m_{21}&m_{22}&m_{23}\end{bmatrix} + v_z\begin{bmatrix}m_{31}&m_{32}&m_{33}\end{bmatrix}
$$
这验证了4.1.7节的观察：**向量×矩阵的运算结果就是矩阵行向量的线性组合**。

### 与坐标空间变换理论的联系


1. **符号统一**：若用 **p**, **q**, **r** 表示新基向量，则矩阵 **M** 可表示为：
$$
M = \begin{bmatrix} \leftarrow & p & \rightarrow \\ \leftarrow & q & \rightarrow \\ \leftarrow & r & \rightarrow \end{bmatrix}
$$
2. **几何意义**：矩阵乘法本质是执行坐标空间变换的紧凑编码方式。例如：
	- 旋转矩阵的行就是旋转后的新基向量
	- 缩放矩阵的行是缩放后的基向量

### 关键知识点图解

| 概念 | 数学表示 | 几何解释 |
|------|----------|----------|
| 标准基 | **i**,**j**,**k** | 原始坐标系的x,y,z轴 |
| 变换矩阵 | **M** | 新坐标系的基向量排列 |
| 向量变换 | **vM** | 向量在新基下的线性组合 |

### 进阶理解

1. **逆向思维**：要构造特定变换的矩阵，只需计算标准基经过该变换后的新坐标，按行排列即可
2. **非方阵情况**：当处理不同维度的变换（如3D→2D投影）时，矩阵的行数会减少，但仍保持"行向量=变换基"的规律
3. **计算机图形学应用**：现代渲染管线中，模型变换矩阵本质上就是定义模型局部坐标系到世界坐标系的基变换

（注：本段内容为保持与原文一致的右乘约定，实际OpenGL/DirectX等API多采用左乘，使用时需注意坐标系的左右手规则）

这个理论框架为理解后续章节的具体变换（旋转/缩放/投影矩阵）提供了根本性的数学基础。

您提出了一个非常关键的问题！旋转确实需要旋转轴的信息，而矩阵能够完整编码这些信息。下面我将通过**几何构造过程**和**矩阵运算原理**两个层面详细解释：

---

## 旋转矩阵

### 一、旋转矩阵的构造原理（含旋转轴）
#### 1. 罗德里格斯旋转公式（Rodrigues' Formula）
任何3D旋转都可以表示为：
$$
R = I + \sinθ \cdot K + (1-\cosθ) \cdot K^2
$$
其中：
- **K** 是由旋转轴 **n**=(nₓ,nᵧ,n_z) 构造的叉积矩阵：
$$
  K = \begin{bmatrix}
  0 & -n_z & n_y \\
  n_z & 0 & -n_x \\
  -n_y & n_x & 0 \\
  \end{bmatrix}
$$
- **θ** 是旋转角度

#### 2. 矩阵元素解析

展开后得到的旋转矩阵：
$$
R = \begin{bmatrix}
n_x^2(1-c)+c & n_xn_y(1-c)-n_zs & n_xn_z(1-c)+n_ys \\
n_yn_x(1-c)+n_zs & n_y^2(1-c)+c & n_yn_z(1-c)-n_xs \\
n_zn_x(1-c)-n_ys & n_zn_y(1-c)+n_xs & n_z^2(1-c)+c \\
\end{bmatrix}
$$
（其中 c=cosθ, s=sinθ）

这个矩阵的**特征向量就是旋转轴 n**，对应的特征值为1（因为旋转轴方向不变）。

---

### 二、矩阵如何旋转模型（几何过程）
#### 1. 模型旋转的本质
旋转一个3D模型，实际上是**将所有顶点坐标从原坐标系变换到旋转后的新坐标系**。具体步骤：

1. **建立局部坐标系**：
   - 假设模型初始顶点坐标基于标准基 **i,j,k**

2. **构造旋转矩阵**：
   - 根据旋转轴 **n** 和角度 **θ** 生成矩阵 **R**

3. **坐标变换**：
   - 对每个顶点 **v**，计算 **v' = Rv**
   - 新坐标 **v'** 就是旋转后的位置

#### 2. 实例演示（绕z轴旋转90度）
旋转矩阵：
$$
R_z(90°) = \begin{bmatrix}
0 & -1 & 0 \\
1 & 0 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
$$
作用效果：
- 原x轴[1,0,0] → 新x轴[0,1,0]（旋转到原y轴位置）
- 原y轴[0,1,0] → 新y轴[-1,0,0]（旋转到负x轴位置）
- z轴保持不变

顶点变换示例：
$$
\begin{bmatrix}2\\3\\4\end{bmatrix} \rightarrow 
\begin{bmatrix}
0×2 + (-1)×3 + 0×4 \\
1×2 + 0×3 + 0×4 \\
0×2 + 0×3 + 1×4 \\
\end{bmatrix} = 
\begin{bmatrix}-3\\2\\4\end{bmatrix}
$$
---

### 三、旋转轴在矩阵中的体现
#### 1. 特征值分析
- 旋转矩阵 **R** 必有**一个实数特征值1**
- 对应的特征向量就是**旋转轴方向**
  
计算示例：
解方程 **(R - I)n = 0** 即可求得旋转轴。

#### 2. 几何解释
- 旋转轴上的向量满足 **Rn = n**（变换后不变）
- 其他向量绕该轴做圆周运动

---

### 四、与欧拉角的对比
| 表示方法 | 优点 | 缺点 |
|----------|------|------|
| 旋转矩阵 | 无歧义、易组合变换 | 需要9个参数 |
| 欧拉角 | 直观（yaw/pitch/roll） | 存在万向节锁 |
| 四元数 | 紧凑、插值平滑 | 数学抽象 |

旋转矩阵可以转换为欧拉角（需考虑旋转顺序）：
$$
\text{绕ZYX顺序的欧拉角} = 
\begin{cases}
θ_x = \arctan2(R_{32}, R_{33}) \\
θ_y = -\arcsin(R_{31}) \\
θ_z = \arctan2(R_{21}, R_{11})
\end{cases}
$$

---

### 五、现代图形管线中的实现
在Shader中的典型应用：
```glsl
// 顶点着色器中的旋转变换
uniform mat4 u_rotationMatrix;
void main() {
    gl_Position = u_rotationMatrix * vec4(aPosition, 1.0);
}
```
其底层硬件会并行计算：
$$
\begin{bmatrix}
R_{11}x + R_{12}y + R_{13}z \\
R_{21}x + R_{22}y + R_{23}z \\
R_{31}x + R_{32}y + R_{33}z \\
\end{bmatrix}
$$

---

### 关键结论
1. **旋转轴编码在矩阵中**：通过特征向量可提取
2. **矩阵旋转的本质**：基向量的重新定向
3. **实际应用**：GPU通过矩阵乘法高效实现顶点变换

这种表示方法既满足了数学严谨性，又为计算机图形学提供了高效的运算基础。理解这一点对3D编程、机器人运动学等领域至关重要。

