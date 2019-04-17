---
title: Kalman filter-卡尔曼滤波器
date: 2019-04-17 15:15:33
categories:
- Summary
tags:
- Kalman filter
---
之前的一段时间一直在看卡尔曼滤波器，用作对运动轨迹的滤波处理(减少扰动)，效果感人。

## 什么是卡尔曼滤波
&ensp &ensp[维基百科上的解释](https://www.wikiwand.com/zh-hans/%E5%8D%A1%E5%B0%94%E6%9B%BC%E6%BB%A4%E6%B3%A2)：卡尔曼滤波是一种高效率的递归滤波器，它能从一系列的不完全及包含杂讯的测量中，估计动态系统的状态。
&ensp &ensp简单来说就是它可以有效利用多个粗糙数据之间的关系