---
title: Fundamentals of Computer Graphics 笔记
date: 2019-08-25 16:58:56
tags:
- Computer Graphics
categories:
- Computer Graphics
mathjax: true
---

第四版教材个人笔记。

图形学主要包括Modeling（建模），Rendering（渲染）、Animation（动画），还包括User interaction（用户交互)、Virtual reality（虚拟现实）、Visualization（可视化）、Image processing（图像处理）、3D scanning（3D扫描）、Compoutational photography（计算摄影）。

使代码处理更快的方法：
1. 尽量以直接的方式写代码。中间结果直接在运行需要时计算，而不存储它们。
2. 在最优化的模式下编译。
3. 使用任何的分析工具来寻找重要的瓶颈问题。
4. 检查数据结构来改善局部性。尽量使得数据单元尺寸与目标架构的cache/page大小一致。
5. 如果分析工具展露出来了数值计算上的瓶颈问题，检查编译器生成的汇编代码（assembly code）。根据问题重写源代码。

## Inverse Mappings
&emsp;&emsp;如果有函数 $f:\boldsymbol A \rightarrow \boldsymbol B$，那么可能存在反函数（inverse function）$f^{-1}:\boldsymbol B \rightarrow \boldsymbol A$，其中定义如$f^{-1}(b)=a$则$b=f(a)$。这一定义只生效于：如果每个$b \in \boldsymbol B$是$f$下的某些点的镜像（image），并且这样的点是只有一个点是满足的（即只有一个$a$满足了$f(a)=b$）。这种映射称为双向映射（*bijections*）。双向映射的情况为每一个$a \in \boldsymbol A$对应于一个唯一的$b \in \boldsymbol B$，并且对于每个$b \in \boldsymbol B$，有一个明确的$a \in \boldsymbol A$使得满足$f(a)=b$。函数不是双映射的不存在反函数。

## Trigonometry
atan和atan2的区别：atan的自变量所在域为实数域，映射的因变量的区间为$[-\pi /2,\pi /2]$；atan2的自变量的所在域为在2D实数平面一对有序实数域，映射的因变量区间为$[- \pi ,\pi]$。

### Pythagorean identities（勾股性质）
$sin^2A+cos^2A=1$
$sec^2A-tan^2A=1$
$csc^2A-cot^2A=1$

### Addition and subtraction identities（加法与减法性质）
$sin(A+B)=sinAcosB+sinBcosA$
$sin(A-B)=sinAcosB-sinBcosA$
$sin(2A)=2sinAcosA$
$cos(A+B)=cosAcosB-sinAsinB$
$cos(A-B)=cosAcosB+sinAsinB$
$cos(2A)=cos^2A-sin^2A$

$tan(A+B)=\displaystyle\frac{tanA+tanB}{1-tanAtanB}$

$tan(A-B)=\displaystyle\frac{tanA-tanB}{1+tanAtanB}$

$tan(2A)=\displaystyle\frac{2tanA}{1-tan^2A}$

### Half-angle indentities（半角性质）
$sin^2(A/2)=(1-cosA)/2$
$cos^2(A/2)=(1+cosA)/2$

### Product identities（乘积性质）
$sinAsinB=-(cos(A+B)-cos(A-B))/2$
$sinAcosB=\quad (sin(A+B)+sin(A-B))/2$
$cosAcosB=\quad(cos(A+B)+cos(A-B))/2$

接下来的性质对于有边长$a$,$b$,和$c$，对应的对角为$A$，$B$，$C$的任意三角形都满足，
$\displaystyle\frac{sinA}{a}=\frac{sinB}{b}=\frac{sinC}{c}$ (Law of sines, 正弦定理)
$c^2=a^2+b^2-2abcosC$ (Law of cosines, 余弦定理)
$\displaystyle\frac{a+b}{a-b}=\frac{tan(\frac{A+B}{2})}{tan(\frac{A-B}{2})}$ (Law of tangents，正切定理)

三角形面积计算方法：
$triangle \ area = \frac{1}{4} \sqrt{(a+b+c)(-a+b+c)(a-b+c)(a+b-c)}$