---
title: Nabla算子——与梯度、散度、旋度和拉普拉斯算子
date: 2019-08-08 17:03:33
tags:
- Math
categories:
- Math
mathjax: true
---

&emsp;&emsp;**Del算子** 或称 **Nabla算子**。符号为$\nabla$，是一个向量微分算子，但本身并非一个向量。
&emsp;&emsp;其形式化定义为：$$\nabla=\frac{d}{dr}$$
&emsp;&emsp;在n维空间中，分母dr为含n个分量的向量，因而$\nabla$本身就是个n维向量算子。
&emsp;&emsp;三维情况下，$\nabla=\frac{\partial}{\partial x}\boldsymbol i + \frac{\partial}{\partial y}\boldsymbol j + \frac{\partial}{\partial z}\boldsymbol k$ 或 $\nabla=(\frac{\partial}
{\partial x},\frac{\partial}{\partial y},\frac{\partial}{\partial z})$
&emsp;&emsp;二维情况下，$\nabla=\frac{\partial}{\partial x}\boldsymbol i + \frac{\partial}{\partial y}\boldsymbol j$ 或 $\nabla=(\frac{\partial}
{\partial x},\frac{\partial}{\partial y})$

&emsp;&emsp;$\nabla$作用于不同类型的量，得到的就是不同类型的新量：

&emsp;&emsp;$\nabla$直接作用于函数$F(r)$（不论F是标量还是向量），则求$F(r)$的梯度，表示为：$\nabla F(r)$（标量函数的梯度为向量，向量的梯度为二阶张量，因此$\nabla$的直接作用，造成结果的升阶）；
&emsp;&emsp;$\nabla$与非标量函数$F(r)$由点积符号$\cdot$连接，则求$F(r)$的散度，表示为：$\nabla \cdot F(r)$；
&emsp;&emsp;$\nabla$与非标量（三维）函数$F(r)$由叉积符号$\times$连接，则求$F(r)$的旋度，表示为：$\nabla \times F(r)$
&emsp;&emsp;$\nabla \cdot \nabla$为 **拉普拉斯算子** 或 **拉普拉斯算符**（Laplace operator, Laplacian），是由欧几里得空间中的一个函数的梯度的散度给出的微分算子，通常写成$\Delta$、${\nabla}^2$或$\nabla \cdot \nabla$。


以上部分摘自维基百科。