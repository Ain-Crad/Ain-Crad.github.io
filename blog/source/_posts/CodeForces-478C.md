---
title: CodeForces-478C-Table Decorations
date: 2018-08-28 14:21:27
categories:
- ACM
tags:
- 思维
---
水题，然而自己WA了orz，看到了一种很有意思的解法。
[参考连接](http://codeforces.com/blog/entry/18619)
<!--more-->
## 链接
[CodeForces-478C-Table Decorations](http://codeforces.com/problemset/problem/478/C)
## 题目描述
给定红黄蓝三种气球的数量，要求每个桌子上放三个气球，且三个气球的颜色不能完全相同，求这些气球最多可以装饰几张桌子。
## 题解
假定三种气球的数量分别为$a[0]，a[1]，a[2]$，且已按大小排好序，$a[0] <= a[1] <= a[2]$。
有两种情况:
* $2*(a[0] + a[1]) <=  a[2]$。这种情况下可以每取一个$a[0]
$取两个$a[2]$组成三气球或者每取一个$a[1]$取两个$a[2]$组成三气球，即取球集合为$(1，0，2)$和$(0，1，2)$。答案为$a[0] + a[1]$。
* $2*(a[0] + a[1]) > a[2]$。这种情况下我们从$a[2]$中取两个气球同时从$max(a[0]，a[1])$中取一个气球，直到$a[2] <= max(a[0]，a[1])$。这时满足$max(a[0]，a[1]) - a[2] <= 1$，$max(a[0]，a[1]) - min(a[0]，a[1]) <= 1$。接下来的按集合$(1，1，1)$继续取球。**可以发现这种取法最后剩余球的数量一定是** $(a[0]+a[1]+a[2])$ mod $3$，**除去剩余的这些球(数量小于3)，其它球每三个装饰一张桌子，所以针对这种情况我们直接可以用** $(a[0]+a[1]+a[2])$ div $3$ **来得到最终结果。**

真是神解orz。
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 28 Aug 14:12:13 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

long long d[5];

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        //freopen("in.txt", "r", stdin);
    #endif
    
    for(int i = 0; i < 3; i++)
        cin >> d[i];
    sort(d, d + 3);
    long long ans;
    if(2 * (d[0] + d[1]) <= d[2])
        ans = d[0] + d[1];
    else 
        ans = (d[0] + d[1] + d[2]) / 3;
    cout << ans << endl;
    
    return 0;
}
```
