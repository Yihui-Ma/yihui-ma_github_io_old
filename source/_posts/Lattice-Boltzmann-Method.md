---
title: Lattice Boltzmann Method
date: 2019-08-10 19:32:47
tags:
- Fluid
categories:
- Fluid
---
# 综述
&emsp;&emsp;在介观方法中，LB方法逐渐被采用为模拟大范围的非单相流动的一种替代的方法。采用简单的动力学方法来模拟水动力系统的理论基础是基于自然宏观现象的观测，如水动力，对于其潜在的微观动力细节方面是相当不敏感的。在水动力系统下，微观动力的细节可以影响运输系数的数值，但是不影响Navier-Stokes equations 的整体形式。

&emsp;&emsp;LB方法，因此，是一种离散玻尔兹曼方程（discrete Boltzmann equation）的极小的形式，在极小的Kn数下主要地重构了Navier-Stokes级别的水动力。它是极小的，因为只有一小部分的离散值$v$→$v_i$从速度空间中保留，这些都要求满足守恒定律并且能够复原正确的空间-时间对称性（space-time symmetries）（伽利略，移动和旋转不变性(Galilean, translational and rotational invariance)）。这使得LB方法成为一种间接的N-S求解器。此外，LB方法也可以容纳描述了许多复杂流动现象的粒子间力(inter-particle force)，并且不用必须求解复杂的动力方程或者它们的分子动力。因此，LB方法适合求解包含了表面动力的问题，微粒的/悬浮的流动（particulate/suspension flows），多相流动（multiphase flows），Kn-过渡流动（Kn-transition flows），多孔介质流动（porous media flows）和二元复杂流动（binary complex flows）及三元复杂流动（ternary complex flows）等。值得讨论的是LB方法的多种天性是一种优点，但是同时，关于它的适用性也是一个潜在的困惑来源。

## 基于LGCA的方程（LGCA-based formulation）
&emsp;&emsp;LB方法来自于它的前身——格子气细胞自动机（LGCA），尤其是FHP模型，相当独立于动力学理论。LGCA背后的基本思想是在一个微观格子上的伪粒子的微观相互作用可以导致平均宏观方程组。对于相互作用，由粒子伴随着格子速度的碰撞（streaming）和传播（propagation）（或流动 (streaming)）过程组成，格子（离散的）对称性在质量守恒方面起到了一个关键作用，并且保证了角动量守恒。但是，FHP模型具有一些缺点，如统计噪声，缺乏伽利略不变性，低雷诺数要求，三维空间难以拓展等。LB方法便是为了解决LGCA的这些问题而出现的。通过一系列的里程碑式的进步，如布尔变量被实值变量替换，碰撞操作符的线性化等，LBM-BGK（LBGK）模型领域的发展，都成功地解决了LGCA的缺点。值得注意的是原始的LBM方程（由Qian和Chen等人提出）受限于等温低马赫数（近似不可压缩(near-incompressible)）流动，并且不提供一种得到非等温LBM模型的系统性的方法（也需要满足能量守恒）。早期的LB模型，由LGCA推导得出，有时候被认为是LB模型的第一代。

<center>
<img src="https://raw.githubusercontent.com/Yihui-Ma/MarkdownImages/master/Lattice%20Boltzmann%20Method/FHP%20model.png" style="zoom:30%">
<div>
FHP模型图
</div>
</center>

## 连续性基于玻尔兹曼-BGK的构想 （Continuous Boltzmann-BGK based formulation）
&emsp;&emsp;基于LGCA的方程并没有一个与玻尔兹曼方程或者气体动力学理论相关的正式联系。与LGCA之间的联系不存在被开创性论文（由Abe，He和Luo）证明了，其中LB方法由（连续）Boltzmann-BGK方程的直接相位-空间离散化所导出。即，LB方法被证明是连续的Boltzmann-BGK方程的一种特殊的有限差分形式。即使生成的方法保持了之前基于LGCA构想（formulation）的结构形式，它在数学形式上是严谨的，（部分地）与动力学理论一致，并且具有更好的稳定性和准确性。更重要的是，这一构想使得LB方法能够通过自适应网格和非笛卡尔坐标网格等，大规模拓展到实际的工程流动问题中。但是，正如基于LGCA的构想，由于在实现推导过程中的小马赫数假设（近似不可压缩），这一构想主要受限于等温（isothermal），低速(low-speed)流动。基于He和Luo的构想的LB模型被称为第二代LB模型。





以上都引用自：
> Kinetically Consistent Thermal Lattice Boltzmann Models  -by Parthib R. Rao, B.Tech, National Institute of Technology, Calicut, India, 2004