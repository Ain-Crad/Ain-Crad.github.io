---
title: hdu 4856 Tunnels-BFS+状态压缩DP
date: 2018-05-16 14:03:19
categories:
- ACM
tags:
- 状态压缩DP
- BFS
---
最近的几场训练赛做题的时候发现自己对搜索类、动态规划类的题有些恐惧，导致这些类型的题比赛时基本做不出来。仔细想了一下，是自己的递归部分理解的就不够透彻，导致搜索写不出来，动态规划就更无从下手。之前也找了很多递归理解的文章去看，也做了一些递归的题，但是效果不佳。意识到网上的资料对于递归的理解一般会拿经典的斐波那契数列或汉诺塔来举例，这对于递归的理解确实是有帮助的，但是对于锻炼搜索思维的效果不是太有效，于是决定先恶补一部分搜索类的题。
这道题是准备西安邀请赛时打的模拟赛做到的题，知识点主要用到了BFS和状态压缩DP，可以当做旅行商问题来做，但是最后不返回起点。由于自己DP比较差，所以搞懂这道题还是花了挺长时间。状态压缩DP部分的实现代码参考大白。

Ps：在最后会贴几个自己觉得看到的对递归和动态规划理解比较有帮助的文章链接。

## 题目链接
[hdu-4856-Tunnels](http://acm.hdu.edu.cn/showproblem.php?pid=4856)
## 题目描述
一张城市网格图，城市中有m条快速通道，可以从图中任意位置出发，求访问完所有快速通道所用的最少时间，忽略在快速通道内的时间。
## 题解
因为在快速通道内的时间是不计的所以只要到达了一个快速通道的入口就相当于访问完了这一条通道，所以可以构造一张$m×m$的邻接表，其中$d[i][j]$表示从第$i$个通道的出口到第$j$个通道的入口的距离，这一部分直接用BFS就能实现，接下来就可以用旅行商问题的解法求解不重复的访问完所有通道所用的时间。
有两个与旅行商问题不同需要注意的地方：
**最后不返回起点**
**起点位置任意，也就是可以将任意一个通道的入口作为起点**
解决方法详见代码。
##代码
```C++
#include <bits/stdc++.h>

using namespace std;

const int maxn = 18;
const int maxm = 18;
const int inf = 0x3f3f3f3f;
char mp[maxn][maxn];
typedef pair<int, int> P;
P in[maxm];
P out[maxm];
int n, m;
int d[maxn][maxn];
int sx, sy;
int dx[4] = {1, 0, -1, 0};
int dy[4] = {0, 1, 0, -1};
int dis[maxm][maxm];
int dp[1 <<  maxm][maxm];

//状态压缩DP
void dpsolve(){
    for(int S = 0; S < 1 << m; S++){
        fill(dp[S], dp[S] + m, inf);
    }
    for(int i = 0; i < m; i++){
        dp[(1 << m) - 1][i] = 0;//因为起点未定，所以终点在任意一个通道都可以
    }
    for(int S = (1 << m) - 2; S >= 0; S--){
        for(int v = 0; v < m; v++){
            for(int u = 0; u < m; u++){
                if(!(S >> u & 1)){
                    dp[S][v] = min(dp[S][v], dp[S | 1 << u][u] + dis[v][u]);
                }
            }
        }
    }
    int res = inf;
    for(int i = 0; i < m; i++){
        res = min(res, dp[1 << i][i]);//判断以不同通道为起点的情况下，哪种用时最短
    }
    if(res == inf) printf("%d\n", -1);
    else printf("%d\n", res);
}

//bfs搜索确定两两通道之间最小距离
void bfs(int sx, int sy){
    queue<P> que;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            d[i][j] = inf;
        }
    }
    d[sx][sy] = 0;
    que.push(P(sx, sy));

    while(!que.empty()){
        P p = que.front();
        que.pop();
        for(int i = 0; i < 4; i++){
            int nx = p.first + dx[i];
            int ny = p.second + dy[i];
            if(nx >= 0 && nx < n && ny >= 0 && ny < n && mp[nx][ny] != '#' && d[nx][ny] == inf){

                d[nx][ny] = d[p.first][p.second] + 1;
                que.push(P(nx, ny));
            }
        }

    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif // ONLINE_JUDGE
    while(~scanf("%d%d", &n, &m)){
        for(int i = 0; i < n; i++){
            scanf("%s", mp[i]);
        }
        for(int i = 0; i < m; i++){
            scanf("%d%d%d%d", &in[i].first, &in[i].second, &out[i].first, &out[i].second);
        }
        //构建邻接表
        for(int i = 0; i < m; i++){
            bfs(out[i].first - 1, out[i].second - 1);
            for(int j = 0; j < m; j++){
                if(i == j) dis[i][j] = 0;
                else dis[i][j] = d[in[j].first - 1][in[j].second - 1];
            }
        }

        dpsolve();
    }
    return 0;
}
```
## 关于递归、动态规划理解的文章链接
[写递归函数的正确思维方法](https://blog.csdn.net/vagrxie/article/details/8470798)
[递归算法详解](https://chenqx.github.io/2014/09/29/Algorithm-Recursive-Programming/)
[怎么更好地终极理解递归算法](https://blog.csdn.net/StruggleShu/article/details/51051140)
[如何有效地理解程序中的递归](https://blog.csdn.net/qq_34773981/article/details/79225588)

[一道题看清动态规划的前世今生（一）](http://www.importnew.com/27323.html)
[一道题看清动态规划的前世今生（二）](http://www.importnew.com/27444.html)

最后对这些文章的博主表示感谢！