---
title: 直观理解线性代数-点积
date: 2018-10-24 21:56:50
categories:
- Messy article
tags:
- 总结
- Liner algebra
---
我们都知道两个向量 $\vec{a}$，$\vec{b}$ 的点积可以想象为 $\vec{a}$ 朝 $\vec{b}$ 方向上的投影长度乘以 $\vec{b}$ 的长度。其中结果的正负代表方向，两个向量成锐角则大于$0$，成钝角则小于$0$。
可是为什么可以这样做呢，这要从线性代数的本质说起。致敬[3Blue1Brown](http://www.3blue1brown.com/)，一个热爱数学，并教你如何从直观上去理解数学的团队，将只停留在数值运算和公式的线性代数推进到可视化几何直观的领悟上，在B站上可以找到搬运的系列视频[线性代数的本质](https://www.bilibili.com/video/av5977466/?spm_id_from=333.788.videocard.8)。
<!-- more -->
```
卡尔文：你知道吗，我觉得数学不是一门科学，而是一种宗教
霍布斯：一种宗教？
卡尔文：是啊。这些公式就像奇迹一般。你取出两个数，把它们相加时，它们神奇地成为了一个全新
的数！没人能说清这到底是怎么发生的。你要么完全相信，要么完全不信。
```
## 点积与投影
视频里关于点积和投影对应的那一部分个人觉得不太好理解，自己也花了挺久才理解，在这里记录一下，并提供一种理解方式。
***以下图片均来自3Blue1Brown发布的线性代数的本质系列视频，直接来源为[Go Further的博客](https://charlesliuyx.github.io/2017/10/06/%E3%80%90%E7%9B%B4%E8%A7%82%E8%AF%A6%E8%A7%A3%E3%80%91%E7%BA%BF%E6%80%A7%E4%BB%A3%E6%95%B0%E7%9A%84%E6%9C%AC%E8%B4%A8/#%E7%82%B9%E7%A7%AF%E4%B8%8E%E5%AF%B9%E5%81%B6%E6%80%A7)***

**首先来看点积的运算：**
两个维数相同的向量做点积，就是把他们对应坐标相乘再相加。
<div style="width: 300px; margin: auto">
    ![1.gif](/1.gif)
</div>
我们假设图中的两个向量分别为 $\vec{a}$ 和 $\vec{b}$， 我们可以发现点积的运算和将向量 $\vec{a}$ 转置为 $1*4$ 的矩阵再和 $\vec{b}$ 做相乘的结果是一样的。从这里入手我们来分析一下矩阵相乘和投影的关系。
**投影：**
任取坐标系中的一条直线作为投影直线，同时在这条直线上取一个单位向量 $\vec{u}$，设它的坐标为$(u_x，u_y)$。
<div style="width: 500px; margin: auto">
    ![2.gif](/2.gif)
</div>
投影是一种线性变换，矩阵的意义就是表示线性变换，所以这个二维空间中的任意一个向量在这条直线上的投影变换都可以用一个矩阵表示出来，至于这个矩阵是什么，我们只需要考虑基向量的变换。(***至于为什么矩阵的意义是线性变换以及为什么用矩阵描述一个变换只需要考虑基向量的变换请移步[线性代数的本质](https://www.bilibili.com/video/av5977466/?spm_id_from=333.788.videocard.8)***)
<div style="width: 500px; margin: auto">
    ![3.gif](/3.gif)
</div>
上图运用对称性得到基向量 $\vec{i}$ 和 $\vec{j}$ 在直线上的投影分别为$u_x$ 和 $u_y$，所以这个投影变换的矩阵也就是
$$
 \begin{bmatrix}
   u_x & u_y
  \end{bmatrix}
$$
所以二维空间中任一向量 $\begin{bmatrix} x \\\\ y \end{bmatrix}$ 在单位向量 $\vec{u}$ 所在直线上的投影值为
$$
 \left[
 \begin{matrix}
   u_x & u_y
  \end{matrix}
  \right]
  \left[
  \begin{matrix}
   x \\\\
   y
  \end{matrix}
  \right]
$$
**把矩阵$\begin{bmatrix} u_x & u_y \end{bmatrix}$转置一下，就变成了点积的定义，这也就是为什么向量的点积可以和几何投影对应起来。**

上面是以单位向量 $\vec{u}$ 为例，如果 $\vec{u}$ 不是单位向量怎么理解呢？
其实也很简单，把 $\vec{u}$ 归一化成单位向量 $\vec{u_0}$，前面提取出一个系数 $c$，这个系数也就是 $\vec{u}$ 的长度。那么空间中任意向量 $\begin{bmatrix} x \\\\ y \end{bmatrix}$ 和 $\vec{u}$ 的点积可以先看做 $\begin{bmatrix} x \\\\ y \end{bmatrix}$ 和 $\vec{u_0}$ 的点积，也就是上面的投影过程，最后再乘以 $c$，也就是 $\vec{u}$ 的长度。
完美对应。