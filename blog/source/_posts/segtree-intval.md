---
title: 线段树区间更新模板
date: 2018-05-08 00:08:11
categories:
- ACM
tags:
- 线段树区间更新
---
线段树区间更新中比较重要的一个概念是延迟标记，即lazy思想，当要对某一个区间中的所有节点进行更新时，先找到包含该区间所有节点的那一个（或多个）节点，只对它（们）进行更新，同时保存更新的值lazy。当进行区间查询时，若查询的区间的大小在lazy标记区间之内则直接返回，否则将lazy值向下进行传递，直到包含查询的区间。通过这种方式，可以用更新整个区间的值来代替更新区间中的每一个节点，从而避免了很多不必要的操作，提高了效率。说起来可能比较抽象，详见代码。
[参考链接](https://www.zybuluo.com/lychee123/note/731591)
<!-- more -->
## 基于区间和的线段树区间更新模板
```C++
struct SegTree{
    int vl, vr;
    long long sum;
    long long lazy;//延迟标记
}tree[maxn << 2];

void build(int k, int l, int r){
    tree[k].vl = l;
    tree[k].vr = r;
    tree[k].lazy = 0;//延迟标记初始化为0
    if(l == r){
        tree[k].sum = d[l];
        return;
    }
    int mid = (l + r) / 2;
    build(k << 1, l, mid);
    build(k << 1 | 1, mid + 1, r);
    tree[k].sum = tree[k << 1].sum + tree[k << 1 | 1].sum;
}
//更新下层区间的值，即将lazy向下进行传递，这里是区别于单点更新的最主要的地方
void pushdown(int k){
    tree[k << 1].lazy += tree[k].lazy;
    tree[k << 1 | 1].lazy += tree[k].lazy;
    tree[k << 1].sum += tree[k].lazy * (tree[k << 1].vr - tree[k << 1].vl + 1);//区间长度乘以当前标记
    tree[k << 1 | 1].sum += tree[k].lazy * (tree[k << 1 | 1].vr - tree[k << 1 | 1].vl + 1);
    tree[k].lazy = 0;//标记传递后清空为0
}

void update(int k, int l, int r, int val){
    if(l <= tree[k].vl && r >= tree[k].vr){
        tree[k].lazy += val;
        tree[k].sum += val * (tree[k].vr - tree[k].vl + 1);
        return;
    }
    if(tree[k].lazy != 0)
        pushdown(k);
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        update(k << 1, l, r, val);
    else if(l > mid)
        update(k << 1 | 1, l, r, val);
    else{
        update(k << 1, l, mid, val);
        update(k << 1 | 1, mid + 1, r, val);
    }
    tree[k].sum = tree[k << 1].sum + tree[k << 1 | 1].sum;
}

long long query(int k, int l, int r){
    if(l <= tree[k].vl && r >= tree[k].vr){
        return tree[k].sum;
    }
    if(tree[k].lazy)
        pushdown(k);
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        return query(k << 1, l, r);
    if(l > mid)
        return query(k << 1 | 1, l, r);
    return query(k << 1, l, mid) + query(k << 1 | 1, mid + 1, r);
}

```
## 例题实现
[POJ-3468--A Simple Problem with Integers](http://acm.hdu.edu.cn/showproblem.php?pid=1754)
AC代码
```C++
//#include <bits/stdc++.h>
#include <cstdio>
#include <cmath>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <queue>
#include <set>
#include <map>

using namespace std;

const int maxn = 1e5 + 7;
int n, q;

struct SegTree{
    int vl, vr;
    long long sum;
    long long lazy;
}tree[maxn << 2];
int d[maxn];

void build(int k, int l, int r){
    tree[k].vl = l;
    tree[k].vr = r;
    tree[k].lazy = 0;
    if(l == r){
        tree[k].sum = d[l];
        return;
    }
    int mid = (l + r) / 2;
    build(k << 1, l, mid);
    build(k << 1 | 1, mid + 1, r);
    tree[k].sum = tree[k << 1].sum + tree[k << 1 | 1].sum;
}

void pushdown(int k){
    tree[k << 1].lazy += tree[k].lazy;
    tree[k << 1 | 1].lazy += tree[k].lazy;
    tree[k << 1].sum += tree[k].lazy * (tree[k << 1].vr - tree[k << 1].vl + 1);
    tree[k << 1 | 1].sum += tree[k].lazy * (tree[k << 1 | 1].vr - tree[k << 1 | 1].vl + 1);
    tree[k].lazy = 0;
}

void update(int k, int l, int r, int val){
    if(l <= tree[k].vl && r >= tree[k].vr){
        tree[k].lazy += val;
        tree[k].sum += val * (tree[k].vr - tree[k].vl + 1);
        return;
    }
    if(tree[k].lazy != 0)
        pushdown(k);
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        update(k << 1, l, r, val);
    else if(l > mid)
        update(k << 1 | 1, l, r, val);
    else{
        update(k << 1, l, mid, val);
        update(k << 1 | 1, mid + 1, r, val);
    }
    tree[k].sum = tree[k << 1].sum + tree[k << 1 | 1].sum;
}

long long query(int k, int l, int r){
    if(l <= tree[k].vl && r >= tree[k].vr){
        return tree[k].sum;
    }
    if(tree[k].lazy)
    pushdown(k);
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        return query(k << 1, l, r);
    if(l > mid)
        return query(k << 1 | 1, l, r);
        return query(k << 1, l, mid) + query(k << 1 | 1, mid + 1, r);
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    char s[10];
    int a, b, c;
    scanf("%d%d", &n, &q);
    for(int i = 1; i <= n; i++)
        scanf("%d", d + i);
    build(1, 1, n);
    while(q--){
        scanf("%s", s);
        if(s[0] == 'Q'){
            scanf("%d%d", &a, &b);
            printf("%lld\n", query(1, a, b));
        }
        else if(s[0] == 'C'){
            scanf("%d%d%d", &a, &b, &c);
            update(1, a, b, c);
        }
    }
	
    return 0;
}

```