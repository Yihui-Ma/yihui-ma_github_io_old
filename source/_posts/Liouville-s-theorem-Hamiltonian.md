---
title: Liouville's theorem(Hamiltonian)
date: 2019-08-09 14:06:36
tags:
- Fluid
categories:
- Fluid
---
<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: ["tex2jax.js"],
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      <!--$表示行内元素，$$表示块状元素 -->
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    },
    "HTML-CSS": { availableFonts: ["TeX"] }
  });
</script>
<!--加载MathJax的最新文件， async表示异步加载进来 -->
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js">
</script>

# Liouville equations
&emsp;&emsp;**Liouville's theorem**，根据法国物理学家Joesph Liouville命名，是经典统计力学（statistical mechanics）和哈密顿力学（Hamiltonian mechanics）的关键理论，它认为沿着系统的路径下，相空间（phase-space）分布函数是常量，即在给定的系统点集的邻近地带，经过相空间（phase-space）的系统点集的密度分布随着时间不变。这一独立于时间的密度在统计力学中被认为是具有一种经典的先验可能性。

# Hamiltonian mechanics
&emsp;&emsp;**Hamiltonian machanics（哈密顿力学）** 是一种由经典力学的重组发展而来的，并且它预估了与非哈密顿经典力学（non-Hamiltonian classical mechanics）所推导出的同样结果。它使用了一种不同的数学形式，提供了一个对经典力学的更抽象的理解。从历史上看，它是经典力学的一次重要重构，为后续的统计力学和量子力学的构成作出了重要的贡献。

&emsp;&emsp;在哈密顿力学中，一种经典的物理系统由规范坐标系\\(\boldsymbol{r}=(\boldsymbol{q},\boldsymbol{p})\\)，其中坐标系的每个元素$q_i$，\\(p_i\\)都是索引到系统的参考坐标系。


<center>
<img src="https://raw.githubusercontent.com/Yihui-Ma/MarkdownImages/master/Liouville's_theorem(Hamiltonian)/Hamilton's_equations.png" style="zoom:30%">
</center>