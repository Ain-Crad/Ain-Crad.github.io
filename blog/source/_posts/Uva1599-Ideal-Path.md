---
title: Uva1599-Ideal Path-双向BFS
date: 2018-11-18 10:49:44
categories:
- ACM
tags:
- BFS
---
给定一个n个顶点和m条边的无向图，每条边都有颜色，求从结点1到结点n的一条最短路，若有多条则取路径颜色序列字典序最小的那条。可能会有重边和自环。

## 链接
[Uva1599-Ideal Path](https://vjudge.net/problem/UVA-1599)

## 题解
这里每条边的权值都是一样的，可以求最短路的方式有 $BFS$，$SPFA$，$Dijkstra$ 等。但是这些方法都不能保证求得的最短路的字典序最小。可以先从节点n出发跑一次 $BFS$。这样再从节点1出发时可以按照各个结点已经标记好的距离，每次到达一个新结点时保证d值恰好减1,直到到达终点。
按照上述规则从起点出发，优先选择颜色字典序最小的走，若多条边的颜色字典序都最小则记录所有这些边的终点，下一步时考虑从所有这些点出发的边。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sat 17 Nov 23:09:12 CST 2018
 *
 */
#include <bits/stdc++.h>

using namespace std;

const int maxn = 1e5 + 7;
const int inf = 1e9 + 7;
struct Edge{
    int to;
    int color;
};
vector<Edge> G[maxn];
int n, m;
int d[maxn];
bool vis[maxn];

void back_bfs(){
    d[n - 1] = 0;
    vis[n - 1] = true;
    queue<int> que;
    que.push(n - 1);
    while(!que.empty()){
        int v = que.front();
        que.pop();
        for(size_t i = 0; i < G[v].size(); i++){
            Edge e = G[v][i];
            int u = e.to;
            if(!vis[u]){
                d[u] = d[v] + 1;
                que.push(u);
                vis[u] = true;
            }
        }

    }
}

vector<int> ans;

void bfs(){
    memset(vis, 0, sizeof(vis));
    vis[0] = true;

    vector<int> next;
    next.push_back(0);
    for(int i = 0; i < d[0]; i++){
        int minColor = inf;
        for(size_t j = 0; j < next.size(); j++){
            int v = next[j];
            for(size_t k = 0; k < G[v].size(); k++){
                Edge e = G[v][k];
                int u = e.to;
                if(d[u] == d[v] - 1){
                    minColor = min(minColor, e.color);
                }
            }
        }
        ans.push_back(minColor);

        vector<int> next2;
        for(size_t j = 0; j < next.size(); j++){
            int v = next[j];
            for(size_t k = 0; k < G[v].size(); k++){
                Edge e = G[v][k];
                if(e.color == minColor && !vis[e.to] && d[e.to] == d[v] - 1){
                    next2.push_back(e.to);
                    vis[e.to] = true;
                }
            }
        }
        next = next2;
    }

    int res = ans.size();
    cout << res << endl;
    for(int i = 0; i < res; i++){
        if(i == 0) cout << ans[i];
        else cout << " " << ans[i];
    }
    cout << endl;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> n >> m){
        memset(d, 0, sizeof(d));
        memset(vis, 0, sizeof(vis));
        ans.clear();
        for(int i = 0; i < n; i++){
            G[i].clear();
        }

        int a, b, c;
        Edge e;
        for(int i = 0; i < m; i++){
            cin >> a >> b >> c;
            e.to = b - 1;
            e.color = c;
            G[a - 1].push_back(e);
            e.to = a - 1;
            G[b - 1].push_back(e);
        }

        back_bfs();
        bfs();
    }
    return 0;
}
```
