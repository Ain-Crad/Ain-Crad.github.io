---
title: Uva10305-Ordering Tasks-拓扑排序
date: 2018-11-04 19:47:51
categories:
- ACM
tags:
- DFS
- 拓扑排序
---
有n个变量，m个二元组(u, v), 分别表示u小于v。要求给出一种所有变量从小到大排列的可能。
<!--more-->
## 链接
[Uva10305-Ordering Tasks](https://vjudge.net/problem/UVA-10305)
## 题解
建图，以变量为顶点，“小于”关系为有向边。问题变为求这个图的拓扑排序。借助DFS来完成排序。
```
在图论中，由一个有向无环图的顶点组成的序列，当且仅当满足下列条件时，称为该图的一个拓扑排序：
1. 每个顶点出现且只出现一次；
2. 若A在序列中排在B的前面，则在图中不存在从B到A的路径。
```
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun  4 Nov 18:47:13 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxn = 107;
int n, m;
int c[maxn];
int res[maxn];
int G[maxn][maxn];
int t;

bool dfs(int u){
    c[u] = -1;
    for(int i = 1; i <= n; i++){
        if(G[u][i]){
            if(c[i] == -1) return false;
            else if(c[i] == 0 && !dfs(i)) return false;
        }
        //c[u] = 1, res[--t] = u;
    }
    c[u] = 1, res[--t] = u;
    return true;
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    
    while(cin >> n >> m){
        if(n == 0 && m == 0) break;
        t = n;
        memset(c, 0, sizeof(c));
        memset(res, 0, sizeof(res));
        memset(G, 0, sizeof(G));
        int x, y;
        for(int i = 0; i < m; i++){
            cin >> x >> y;
            G[x][y] = 1;
        }
        for(int i = 1; i <= n; i++){
            if(!c[i]){
                //cout << "i: " << i << endl;
                dfs(i);
            }
        }
        for(int i = 0; i < n; i++){
            if(!i) cout << res[i];
            else cout << " " << res[i];
        }
        cout << endl;
    }
    
    return 0;
}
```
## 拓展
拓扑等价：
有多个解释，其中一个几何上的解释是，一个曲面经过扭转、弯曲、拉长或收缩得到另一个曲面，期间没有出现任何点的重叠与断开，那么可以说这两个曲面是拓扑等价的。
```
//知乎上看到的一个笑话
“美女和丑女在数学上是不是拓扑等价的？”
“这要看她有没有打耳洞”
（逃
```