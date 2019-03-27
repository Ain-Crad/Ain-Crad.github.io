---
title: Covariance-协方差
date: 2019-03-27 10:26:57
categories:
- Math
tags:
- Covariance
---
*填坑数学系列*
总结协方差的意义，它可以表示物体间的什么属性，它和相关系数之间的关系，以及协方差矩阵的作用。
<!--more-->
## 概念
协方差用在概率论和统计学中于衡量两个变量的总体误差([Wikiwand](https://www.wikiwand.com/zh-hans/%E5%8D%8F%E6%96%B9%E5%B7%AE))。
简单来说协方差用来描述两个随机变量之间具有什么样的关系。

## 数学定义
和方差的数学定义类似：$$Var(X)=\frac{\sum_{i=1}^n(x_i-\overline{x})(x_i-\overline{x})}{n-1}$$
协方差的数学定义：$$Cov(X,Y)=\frac{\sum_{i=1}^n(x_i-\overline{x})(y_i-\overline{y})}{n-1}$$
通过向量表示为：$$Cov(X,Y)=\frac{\vec{x}\cdot\vec{y}}{n-1}$$

可以发现协方差看起来很像**点积**。

## 相关系数
协方差可以理解为通过**点积**来计算关系。举一个实际应用的例子。
假设有A，B，C，D，E，F六本书，甲、乙两个人对它们的评分(满分5分)如下：
```
　    A  B  C  D  E  F
甲    4  3  0  0  5  0
乙    4  0  4  0  4  0
```
甲的信息可以用向量$\vec{x} = (4,3,3,0,5,3)$来表示。
乙的信息可以用向量$\vec{y} = (4,2,4,0,4,4)$来表示。
甲和乙的协方差经过计算可得(有偏计算)：$$Cov(X,Y)=2.4$$
这个值可以表示甲、乙之间的关系，但是由于点积结果受到向量长度的影响，所以我们最好把**协方差进行归一化**，具体的做法是将得到的协方差再除以变量各自的标准差。
$$r=\frac{\sum_{i=1}^n(x_i-\overline{x})(y_i-\overline{y})}{\sqrt{\sum_{i=1}^n(x_i-\overline{x})^2\sqrt{\sum_{i=1}^n(y_i-\overline{y})^2}}}$$
这被称作两个变量样本的**相关系数**，其实就是两向量夹角的**余弦**：$$\cos\theta=\frac{\vec{x}\cdot\vec{y}}{\|\|\vec{x}\|\|\|\|\vec{y}\|\|}$$
上面的例子通过计算相关系数得到：$$r=\cos\theta\approx0.86$$
余弦范围为[-1，1]，所以结果表明两者之间应该挺有正向关系的，可以考虑把甲喜欢的书推荐给乙。

## 协方差矩阵
协方差反映的是两个变量之间的相关性，协方差矩阵反映的是所有变量两两之间的关系。具体来说，包含两个特征变量的矩阵：
$$
Z=
 \begin{bmatrix}
   1 & 2 \\\\
   3 & 6 \\\\
   4 & 5 \\\\
   5 & 2 \\\\
  \end{bmatrix}
$$
其中两个特征X，Y分别为：
$$
X=
 \begin{bmatrix}
   1 \\\\
   3 \\\\
   4 \\\\
   5 \\\\
  \end{bmatrix}
，
Y=
 \begin{bmatrix}
   2 \\\\
   6 \\\\
   2 \\\\
   2 \\\\
  \end{bmatrix}
$$
此时协方差矩阵为：
$$
Cov(Z)=
 \begin{bmatrix}
   Cov(X,X) & Cov(X,Y) \\\\
   Cov(Y,X) & Cov(Y,Y) \\\\
 \end{bmatrix}
$$

*注：自己和自己的协方差等于自己的方差*

## 协方差矩阵的作用
协方差矩阵经常被用来计算特征之间的某种联系，机器学习中的用于降维的主成分分析(PCA)就用到了协方差矩阵。

## 参考
[如何通俗易懂地解释协方差和相关系数的概念](https://www.zhihu.com/question/20852004/answer/287792087)
[协方差矩阵-Jermmy's Lazy Blog](http://jermmy.xyz/2017/03/19/2017-3-19-covariance-matrix/)