---
title: 工科数学分析的复习
published: 2025-05-13
description: 工数的二次复习，重点为试卷，支援期中考试和期末
tags:
  - math
  - review
category: course
draft: true
image: /violet.png
---


# 加急复习结节奏

## 现状以及计划

周四周五周六周日四天

暂时只有四天时间复习，然后当天能够延迟交卷，可以搜题啥的，不过估计也够呛，需要大量时间复习，正好map-gen已经结束了，可以好好复习一下，平时作业后来就在抄，基础也不太好。

暂时资料就只有作业题目和以往的期中试卷，可以先做一套期中试卷，然后手写知识点框架为主，然后笔记速速录入md，因为数学复习感觉刷题还是占用大多时间，手写知识点可以简陋一些。

刷题资料只有试卷和平时的作业题，加上笔记完善，就是全部的复习计划了！

# 叙述定义

## 小概念

### 聚点（Limit Point）

设 $D$ 是平面或空间中的一个点集，$P_0$ 是某一点。如果 **任意邻域** 内都包含 $D$ 中 **无限多个** 不同于 $P_0$ 的点，则称 $P_0$ 为 $D$ 的聚点。

- **例子**：区间 $(0,1)$ 的聚点包括所有 $x \in [0,1]$，因为任意接近 $0$ 或 $1$ 的点都有无限多个其他点在其邻域内。  

### 孤立点（Isolated Point）

若 $P_0 \in D$，但存在某个邻域 **仅包含 $P_0$ 本身**，则称 $P_0$ 为 $D$ 的孤立点。 

- **例子**：点集 $\{0\} \cup \{\frac{1}{n} \mid n \in \mathbb{N}\}$ 中，$0$ 是聚点，而每个 $\frac{1}{n}$ 是孤立点。  

### 级数（Series）的定义与基本概念

#### 级数的定义

在数学中，**级数**是指将数列的各项依次相加所构成的**无限求和表达式**。一般形式为：  
$$
S = \sum_{n=1}^{\infty} a_n = a_1 + a_2 + a_3 + \cdots + a_n + \cdots
$$
其中：
- $a_n$ 称为级数的**通项**（或**一般项**），
- $S_n = a_1 + a_2 + \cdots + a_n$ 称为级数的**前 $n$ 项部分和**。

#### 级数的收敛与发散

- **收敛（Convergent）**：如果部分和数列 $\{S_n\}$ 的极限存在，即  
  $$
  \lim_{n \to \infty} S_n = S \quad (\text{有限值}),
  $$  
  则称级数 $\sum a_n$ **收敛**，$S$ 称为级数的**和**。  
- **发散（Divergent）**：如果 $\lim_{n \to \infty} S_n$ 不存在（如趋于无穷或振荡），则称级数**发散**。

#### 常见级数类型

| 级数类型 | 定义 | 例子 |
|----------|------|------|
| **常数项级数** | 通项 $a_n$ 为常数 | $\sum_{n=1}^{\infty} \frac{1}{n}$（调和级数） |
| **交错级数** | 通项符号交替变化 | $\sum_{n=1}^{\infty} (-1)^{n-1} \frac{1}{n}$（莱布尼茨级数） |
| **幂级数** | 形如 $\sum_{n=0}^{\infty} c_n x^n$ | $\sum_{n=0}^{\infty} \frac{x^n}{n!} = e^x$ |
| **傅里叶级数** | 用三角函数展开 | $f(x) = \frac{a_0}{2} + \sum_{n=1}^{\infty} (a_n \cos nx + b_n \sin nx)$ |

#### 判断级数收敛的方法

- **比较判别法**：与已知收敛/发散的级数对比（如 $p$-级数 $\sum \frac{1}{n^p}$）。  
- **比值判别法**：计算 $\lim_{n \to \infty} \left| \frac{a_{n+1}}{a_n} \right|$。  
- **积分判别法**：将级数转化为积分判断（适用于单调递减正项级数）。  
- **莱布尼茨判别法**：用于交错级数（见前文）。

#### **例子**  
- **收敛级数**：$\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}$（巴塞尔问题）。  
- **发散级数**：$\sum_{n=1}^{\infty} \frac{1}{n}$（调和级数）。  



#### 交错级数（Alternating Series）

形如 $\sum_{n=1}^{\infty} (-1)^{n-1} a_n$（$a_n > 0$）的级数，其项正负交替出现。  

- **判别法（莱布尼茨）**：若 $a_n$ **单调递减** 且 $\lim_{n \to \infty} a_n = 0$，则级数收敛。  
- **例子**：$\sum_{n=1}^{\infty} (-1)^{n-1} \frac{1}{n}$ 收敛（但非绝对收敛）。  

#### 余项（Remainder）

级数 $\sum_{n=1}^{\infty} (-1)^{n-1} a_n$ 前 $n$ 项部分和为 $S_n$，则余项 $r_n = S - S_n$ 表示截断误差。 

- **估计**：若级数满足莱布尼茨条件，则 $|r_n| \leq a_{n+1}$（即下一项的绝对值）。  

## 二元函数的连续性

定义7.2.2 设 $z=f(x,y)$ 的定义域为 $D$，$P_0$ 是 $D$ 的聚点且 $P_0 \in D$。如果  

$$
\lim_{P \to P_0} f(P) = f(P_0),
$$
则称 $f(x,y)$ 在 $P_0 (x_0, y_0)$ 处连续，$(x_0, y_0)$ 为 $f(x,y)$ 的连续点。  用“$\epsilon$”的语言叙述，就是：$\forall \epsilon > 0$，$\exists \delta > 0$。当 $|| P - P_0 || < \delta$ 时，有

$$
|| f(P) - f(P_0) || < \epsilon.
$$
由上述定义可见，$P_0$ 是 $D$ 的孤立点时，上述条件也满足。因此我们约定：定义域 $D$ 中的孤立点都是函数的连续点。若 $f(x,y)$ 在 $D$ 的每一点都是连续的，则称 $f(x,y)$ 在 $D$ 连续，记作 $f \in C(D)$。

## 方向导数

定义7.5.1(方向导数) 设函数 $z=f(x,y)$ 在点 $P_0 (x_0, y_0)$ 处的邻域内有若干极限  

$$
\lim_{t \to 0} \frac{f(P_0 + t)}{t} = f(P_0)
$$

存在，则称它为 $f(x,y)$ 在点 $P_0$ 沿方向 $l$ 的方向导数，记作  

$$
\left. \frac{\partial f(P_0)}{\partial l} \right|_{P_0}
$$

$f(x,y)$ 在任意点 $P$ 沿方向 $l$ 的方向导数记作  

$$
   \left. \frac{\partial f(P)}{\partial l} \right|_{P_0}, \quad \text{或} \quad \left. \frac{\partial f}{\partial l} \right|_{P_0}.
$$

## Green公式

定理9.5.1 设 $D$ 是以分段光滑的曲线 $L$ 为边界的平面闭区域，函数 $P(x,y)$ 和 $Q(x,y)$ 在 $D$ 上具有一阶连续偏导数，则有公式  

$$
\iint\limits_{D} \left( \frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y} \right) dxdy = \oint\limits_{L} Pdx + Qdy,
$$

其中 $L$ 是 $D$ 的取正向的边界曲线。  

## 莱布尼茨判别法

定理10.3.1（莱布尼茨判别法） 设交错级数（10.3.1）满足条件

1. $a_n \geq a_{n+1}, n=1,2,\cdots;$  
2. $\lim_{n \to \infty} a_n = 0.$  

则级数收敛且对于余项 $r_n = \sum_{k=n+1}^{\infty} (-1)^{k-1}a_k$，有估计式
   
$$
|r_n| = \left| \sum_{k=n+1}^{\infty} (-1)^{k-1}a_k \right| \leq a_{n+1}.
$$






### 问题重述

计算曲面积分：

$$
I = \iint_{\Sigma} \frac{x \, dy dz + (z+1)^2 \, dx dy}{\sqrt{x^2 + y^2 + z^2}},
$$
其中 $\Sigma$ 是下半球面 $z = -\sqrt{1 - x^2 - y^2}$ 的上侧。

### 简化思路

题目提示这是一个简单的二重积分问题，因此我们尝试将曲面积分直接转化为 \(xy\)-平面上的二重积分。

#### 1. 曲面方程

下半球面的方程为：

$$
z = -\sqrt{1 - x^2 - y^2}, \quad x^2 + y^2 \leq 1.
$$

#### 2. 曲面积分转化为二重积分

曲面积分的一般形式为：
$$
\iint_{\Sigma} P \, dy dz + Q \, dz dx + R \, dx dy.
$$
对于曲面 \(z = z(x, y)\)，可以表示为：
$$
\iint_{\Sigma} R \, dx dy = \iint_{D} R(x, y, z(x, y)) \, dx dy,
$$
$$
\iint_{\Sigma} P \, dy dz = \iint_{D} P(x, y, z(x, y)) \left(-\frac{\partial z}{\partial x}\right) dx dy,
$$
其中 \(D\) 是曲面在 \(xy\)-平面的投影（这里是单位圆盘）。

#### 3. 计算各部分

- **\(R \, dx dy\) 部分**：

$$
  R = \frac{(z+1)^2}{\sqrt{x^2 + y^2 + z^2}} = \frac{\left(-\sqrt{1 - x^2 - y^2} + 1\right)^2}{1} = \left(1 - \sqrt{1 - x^2 - y^2}\right)^2.
$$
  因此：
$$
  \iint_{\Sigma} R \, dx dy = \iint_{D} \left(1 - \sqrt{1 - x^2 - y^2}\right)^2 dx dy.
$$

- **\(P \, dy dz\) 部分**：
$$
  P = \frac{x}{\sqrt{x^2 + y^2 + z^2}} = x.
$$
  计算 
  $$\frac{\partial z}{\partial x}$$
$$
  \frac{\partial z}{\partial x} = \frac{x}{\sqrt{1 - x^2 - y^2}}.
$$
  因此：
$$
  \iint_{\Sigma} P \, dy dz = \iint_{D} x \left(-\frac{x}{\sqrt{1 - x^2 - y^2}}\right) dx dy = -\iint_{D} \frac{x^2}{\sqrt{1 - x^2 - y^2}} dx dy.
$$

#### 4. 合并积分

总积分：
$$
I = -\iint_{D} \frac{x^2}{\sqrt{1 - x^2 - y^2}} dx dy + \iint_{D} \left(1 - \sqrt{1 - x^2 - y^2}\right)^2 dx dy.
$$

#### 5. 极坐标变换

设 
$$
x = r \cos \theta，y = r \sin \theta，dx dy = r dr d\theta，D 为 0 \leq r \leq 1，0 \leq \theta \leq 2\pi
$$

- **第一项**：
$$
  -\int_0^{2\pi} \int_0^1 \frac{r^2 \cos^2 \theta}{\sqrt{1 - r^2}} r dr d\theta = -\int_0^{2\pi} \cos^2 \theta d\theta \int_0^1 \frac{r^3}{\sqrt{1 - r^2}} dr.
 $$
  计算角度部分：
$$
  \int_0^{2\pi} \cos^2 \theta d\theta = \pi.
$$
  计算径向部分（设 \(u = 1 - r^2\)）：
$$
  \int_0^1 \frac{r^3}{\sqrt{1 - r^2}} dr = \frac{2}{3}.
$$
  因此第一项为：
$$
  -\pi \cdot \frac{2}{3} = -\frac{2\pi}{3}.
$$
- **第二项**：
$$
  \int_0^{2\pi} \int_0^1 \left(1 - \sqrt{1 - r^2}\right)^2 r dr d\theta = 2\pi \int_0^1 \left(1 - 2\sqrt{1 - r^2} + (1 - r^2)\right) r dr.
$$
  计算径向部分：
 $$
  \int_0^1 \left(2 - 2\sqrt{1 - r^2} - r^2\right) r dr = \frac{1}{12}.
$$
  因此第二项为：
$$
  2\pi \cdot \frac{1}{12} = \frac{\pi}{6}.
$$

#### 6. 最终结果
$$
I = -\frac{2\pi}{3} + \frac{\pi}{6} = -\frac{\pi}{2}.
$$

### 最终答案
$$
\boxed{ -\frac{\pi}{2} }
$$