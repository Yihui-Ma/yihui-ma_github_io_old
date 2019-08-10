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

以上都引用自：
> Kinetically Consistent Thermal Lattice Boltzmann Models  -by Parthib R. Rao, B.Tech, National Institute of Technology, Calicut, India, 2004