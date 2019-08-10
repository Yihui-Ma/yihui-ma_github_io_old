---
title: 使用介观方法（Mesoscale Methods）的原因
date: 2019-08-10 16:30:01
tags:
- Fluid
categories:
- Fluid
---
# 为什么使用介观方法？（Why Mesoscale Methods?）
&emsp;&emsp;基于连续性假设的模型对于大部分流体问题来说已经足够了；但是，许多宏观的流体现象仍然受到其潜在的微观世界的重要影响。表面张力就是一个重要的例子：分子之间的凝聚力在宏观上起到了相位分离的作用，同时还伴随着表面的动力学影响。但是，使用连续性方法来模拟仿真这一现象充满了困难，因为控制方程都是在宏观尺度下的，其中由于基本机制而产生的表面动力学都是在微观尺度下的。在微通道下的气体流动，尤其是在具有墙边界（wall boundaries）（大Kn数流动）的情况下是另一种情况，在这里基于连续性方法是不够的。介观模型技术在这些情况是有用处的，如我们需要模拟宏观流体，但是也需要拓展这之下重要的微观现象。介观方法通过统计力学和动力学理论，提供了微观世界和宏观世界的连接。

&emsp;&emsp;目前已经发展了许多种介观尺度方法，其中的一些直接地求解了玻尔兹曼方程（Boltzmann equation），如格子玻尔兹曼方法（lattice Boltzmann (LB) method），离散速度方法（discrete velocity method (DVM)），气体动力学方案（gas-kinetic scheme (GKS)）等；并且一些直接模拟了伪粒子，如格子气细胞自动机（Lattice Gas Cellular Automata (LGCA)），耗散粒子动力学（Dissipative Particle Dynamics (DPD)），以及直接模拟蒙特卡罗方法（Direct Simulation Monte Carlo (DSMC) method）。所有的这些方法不像在分子动力学（Molecular Dynamics (MD)）里一样追踪单独的粒子，但是跟踪一组伪粒子（pseudo-particles），统计上地由它们的概率分布函数（Probability Distribution Function (PDF)）表示。

以上都引用自：
> Kinetically Consistent Thermal Lattice Boltzmann Models  -by Parthib R. Rao, B.Tech, National Institute of Technology, Calicut, India, 2004