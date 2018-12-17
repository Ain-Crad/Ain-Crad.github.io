---
title: Uva12118-Inspetor's Dilemma-DFS求连通+欧拉路
date: 2018-12-17 18:36:53
categories:
- ACM
- 欧拉路
tags:
- 欧拉路
---
V个城市之间两两相连，给定E条需要走的边，走过一条边需要时间T。求走完所有的E条边需要的最少时间。E条边不一定在一个连通图中。
竟然卡在``ifndef ONLINE_JUDGE``上好久，单词``ONLINE``手滑打成了``ONLIEN``，= =。吐血。

*紫书第六章结束*

## 链接
题目链接[Uva12118-Inspetor's Dilemma](https://vjudge.net/problem/UVA-12118)
参考[SingleK's Blog](https://blog.csdn.net/xiao_k666/article/details/79209697)精简了自己的代码。

## 题解
因为E条边不一定在一个连通图中，所以首先DFS遍历连通块，统计每一个连通块中度数为奇数的结点个数。若能满足构成欧拉路的条件，则一次“一笔画”就可以走完当前连通块中的边。若不能构成欧拉路（奇度数结点个数大于2），则通过加边的方式，使其可以构成欧拉路。n个连通块之间还需要n - 1条边连接。
所以最后的所需要走过的边是E，加上添加边的个数，再加上连通块个数减1。
*最后不要忘记乘以T得到总耗时 = =，以及当给定E为0时，结果会出现负数，要特判。*

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sat 15 Dec 17:21:07 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 1e3 + 7;
int V, E, T;
bool vis[maxn];
vector<int> d[maxn];
int ans;
int odd;

void dfs(int u){
    vis[u] = 1;
    if(d[u].size() % 2) odd++;
    for(size_t i = 0; i < d[u].size(); i++){
        int v = d[u][i];
        if(!vis[v]){
            dfs(v);
        }
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    int kase = 0;
    while(cin >> V >> E >> T){
        if(V == 0 && E == 0 && T == 0) break;
        kase++;
        memset(vis, 0, sizeof(vis));
        for(int i = 1; i <= V; i++){
            d[i].clear();
        }
        ans = 0;

        int u, v;
        for(int i = 0; i < E; i++){
            cin >> u >> v;
            d[u].push_back(v);
            d[v].push_back(u);
        }

        int cnt = 0;
        for(int i = 1; i <= V; i++){
            odd = 0;
            if(!d[i].empty() && !vis[i]){
                dfs(i);
                cnt++;
                if(odd > 2) ans += (odd - 2) / 2;
            }
        }
        ans += E;
        ans += cnt - 1;
        ans *= T;
        if(ans < 0) cout << "Case " << kase << ": " << 0 << endl;
        else cout << "Case " << kase << ": " << ans << endl;
    }
    return 0;
}
```