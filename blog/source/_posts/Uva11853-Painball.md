---
title: Uva11853-Painball-全局+DFS
date: 2018-11-20 10:53:30
categories:
- ACM
- DFS
tags:
- DFS
- 全局思想
---
好题哇。有一个 $1000 \times 1000$ 的方形场地，有n个敌人分别在 $(x_i，y_i)$ 处，且每个人有一个攻击范围 $r_i$ ,求解是否可以在不受攻击的情况下从场地的最西侧走到场地的最东侧，若可以，输出起始和终点坐标。如果有多个位置满足条件，输出最靠北的位置。
<!--more-->
# 链接
[Uva-Painball](https://vjudge.net/problem/UVA-11853)

# 题解
一开始的思路是建一个 $1000 \times 1000$ 的图，将每一个敌人攻击范围内的所有点做标记，做为图中的障碍物，然后从上边界开始跑一遍DFS/BFS，看是否可以到达下边界。如果可以的话说明图被分成了左右两部分，东西两侧不可达。参考了刘汝佳老师的代码后发现完全没有这个必要 = =。
不以坐标为基本单位，而直接以各个圆为基本单位，从一个与上边界有交的圆出发，不断递归寻找和它自身相交的圆，判断是否可以到达一个与下边界相交的圆。同时沿途检查这一系列中和左右边界有交的圆，这些圆和左边界的所有交点中最南边的点就是最北起始点，和右边界的所有交点中最南边的点就是最北终止点。膜Orz。

# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 20 Nov 09:54:58 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 1000 + 7;
const double brd = 1000.0;
int n;
double x[maxn], y[maxn], r[maxn];
bool vis[maxn];
bool ok;
double lb, rb;

bool intersect(int u, int v){
    return (x[u] - x[v]) * (x[u] - x[v]) + (y[u] - y[v]) * (y[u] - y[v]) <= (r[u] + r[v]) * (r[u] + r[v]);
}

void checkcircle(int c){
    if(x[c] - r[c] <= 0){
        lb = min(lb, y[c] - sqrt(r[c] * r[c] - x[c] * x[c]));
    }
    if(x[c] + r[c] >= brd){
        rb = min(rb, y[c] - sqrt(r[c] * r[c] - (brd - x[c]) * (brd - x[c])));
    }
}

void dfs(int c){
    if(y[c] - r[c] <= 0){
        ok = false;
        return;
    }
    for(int i = 0; i < n; i++){
        if(!vis[i] && intersect(c, i)){
            vis[i] = 1;
            dfs(i);
        }
    }
    checkcircle(c);
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> n){
        ok = true;
        lb = rb = brd;
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < n; i++){
            cin >> x[i] >> y[i] >> r[i];
        }
        for(int i = 0; i < n; i++){
            if(y[i] + r[i] >= brd){
                vis[i] = 1;
                dfs(i);
                if(!ok) break;
            }
        }
        if(!ok) cout << "IMPOSSIBLE" << endl;
        else printf("0.00 %.2f 1000.00 %.2f\n", lb, rb);
    }
    return 0;
}
```