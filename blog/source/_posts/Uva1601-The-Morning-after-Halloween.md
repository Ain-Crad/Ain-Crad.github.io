---
title: Uva1601-The Morning after Halloween-建图+BFS状态搜索/双向BFS
date: 2019-01-02 10:02:57
categories:
- ACM
- BFS
tags:
- 建图
- 双向BFS
---
卡了两天，中间曾一度自闭...(●°u°●)​ 」。不过理解了之后发现确实是好题。
$w \times h，（w, h <= 16）$的网格上有$n（n <= 3）$个小写字母（代表ghost）。要求把它们分别移动到对应的大写字母里。每步可以有多个ghost同时移动（均为上下左右4个方向之一移动），但每步结束之后任何两个ghost不能占用同一个位置，也不能在一步之内交换位置。
<!--more-->

# 链接
题目链接：[Uva1601-The Morning after Halloween](https://vjudge.net/problem/UVA-1601)
参考链接：[crazysillynerd's blog](https://blog.csdn.net/crazysillynerd/article/details/42562959)

# 题解
总的状态数为$256^3$。另外每次转移时需要$5^3$枚举每一个ghost的下一个位置（上下左右+不动）。如果这样直接BFS的话，肯定会超时。于是要进行优化，第一次接触这类问题的优化方法，感谢博主[crazysillynerd](https://blog.csdn.net/crazysillynerd/article/details/42562959)，讲解的很清晰。
首先是尽量避免搜索不合法的走法。题目条件“任何一个2\*2子网格中至少有一个障碍格”说明了有很多格子都是障碍，可转移的状态很多都是不合法的。每次都压到队列里然后不合法再排除就会浪费很多时间。以这里为优化点，可以**把迷宫图转换成图，用邻接表保存起来**，这样搜索的时候就只走可以走的点，而不是临时判断5种方案是否合法。
其次在查重上。因为任何一个2\*2的子网格中至少有一个障碍格，所有最多有256 \* 75%的格子即不超过200个格子可以走。**首先把坐标(x，y)，映射到一个整型数上，（这个整型数相当于把16*16的网格拉伸成一维之后原坐标点此时的数组下标，但是排除了'#'的格子）。然后建立一个数组vis[200][200][200]用来查重。**
最后考虑ghost的数量不同的问题。在建图的时候把多余的不存在的ghost**放到一个孤立的点中，然后使其起始位置和目标位置相同**，这样就避免了需要根据情况动态调整的麻烦。
总体思路：**输入->建图->BFS**

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Wed  2 Jan 07:17:12 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 20;
int w, h, n;
char d[maxn][maxn];
int s[3], t[3];
int cnt;
int x[200], y[200], id[maxn][maxn];;
bool vis[200][200][200];
vector<int> G[200];
int dx[] = {1, 0, -1, 0, 0};
int dy[] = {0, 1, 0, -1, 0};
int ans;
struct Pos{
    Pos(int a = 0, int b = 0, int c = 0, int dis = 0):a(a), b(b), c(c), dis(dis){}
    int a, b, c;
    int dis;
};

void bfs(){
    queue<Pos> que;
    que.push(Pos(s[0], s[1], s[2], 0));
    vis[s[0]][s[1]][s[2]] = 1;

    while(!que.empty()){
        Pos u = que.front();
        que.pop();
        if(u.a == t[0] && u.b == t[1] && u.c == t[2]){
            ans = u.dis;
            break;
        }
        for(int i = 0; i < (int)G[u.a].size(); i++){
            for(int j = 0; j < (int)G[u.b].size(); j++){
                for(int k = 0; k < (int)G[u.c].size(); k++){
                    int p1 = G[u.a][i], p2 = G[u.b][j], p3 = G[u.c][k];

                    //check conflict
                    if(vis[p1][p2][p3]) continue;
                    if(p1 == p2 || p1 == p3 || p2 == p3) continue;
                    if(p1 == u.b && p2 == u.a) continue;
                    if(p1 == u.c && p3 == u.a) continue;
                    if(p2 == u.c && p3 == u.b) continue;

                    que.push(Pos(p1, p2, p3, u.dis + 1));
                    vis[p1][p2][p3] = 1; 
                }
            }
        }
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> w >> h >> n){
        if(w == 0 && h == 0 && n == 0) break;
        getchar();
        cnt = 0;
        memset(id, -1, sizeof(id));
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < 200; i++){
            G[i].clear();
        }

        for(int i = 0; i < h; i++){
            fgets(d[i], maxn, stdin);
        }

        for(int i = 0; i < h; i++){
            for(int j = 0; j < w; j++){
                if(d[i][j] != '#'){
                    x[cnt] = i, y[cnt] = j, id[i][j] = cnt;
                    if(islower(d[i][j])) s[d[i][j] - 'a'] = cnt;
                    if(isupper(d[i][j])) t[d[i][j] - 'A'] = cnt;
                    cnt++;
                }
            }
        }

        for(int i = 0; i < cnt; i++){
            for(int dir = 0; dir < 5; dir++){
                int nx = x[i] + dx[dir], ny = y[i] + dy[dir];
                //"Outermost cells of a map are walls" means we don't need to check out-of-bound
                if(d[nx][ny] != '#') G[i].push_back(id[nx][ny]);
            }
        }

        //add fake nodes
        if(n <= 2){
            G[cnt].push_back(cnt);
            s[2] = t[2] = cnt;
            cnt++;
        } 
        if(n <= 1){
            G[cnt].push_back(cnt);
            s[1] = t[1] = cnt;
            cnt++;
        }

        ans = -1;
        bfs();
        cout << ans << endl;
    }
    return 0;
}
```
# 双向BFS解法
参考链接：[phlsheji's blog](https://www.cnblogs.com/bhlsheji/p/4804022.html)
参考连接：[crazysillynerd](https://blog.csdn.net/crazysillynerd/article/details/42681579)

双向BFS是利用两个队列。一个队列保存从起点开始向后搜索的状态，另一个保存从终点开始向前搜索的状态，**双向BFS主要是区分每一个格子是从起点开始搜索到的还是从终点开始搜索到的**，每一个经过的格子结点保存到达该格子经过的步数，这样两边出现相交，两个方向的步数相加就是最终结果。
*双向BFS的效率会高一些。*

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Wed  2 Jan 07:17:12 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 20;
int w, h, n;
char d[maxn][maxn];
int s[3], t[3];
int cnt;
int x[200], y[200], id[maxn][maxn];;
int vis[200][200][200];
int dis_s[200][200][200], dis_t[200][200][200];
vector<int> G[200];
int dx[] = {1, 0, -1, 0, 0};
int dy[] = {0, 1, 0, -1, 0};
int ans;
struct Pos{
    Pos(int a = 0, int b = 0, int c = 0):a(a), b(b), c(c){}
    int a, b, c;
};

void adv_bfs(){
    queue<Pos> que_s;
    que_s.push(Pos(s[0], s[1], s[2]));
    vis[s[0]][s[1]][s[2]] = 1;
    dis_s[s[0]][s[1]][s[2]] = 0;
    queue<Pos> que_t;
    que_t.push(Pos(t[0], t[1], t[2]));
    vis[t[0]][t[1]][t[2]] = 2;
    dis_t[t[0]][t[1]][t[2]] = 0;

    while(!que_s.empty() || !que_t.empty()){
        if(!que_s.empty()){
            Pos u = que_s.front();
            que_s.pop();
            if(vis[u.a][u.b][u.c] == 2){
                ans = dis_s[u.a][u.b][u.c] + dis_t[u.a][u.b][u.c];
                return;
            }

            for(int i = 0; i < (int)G[u.a].size(); i++){
                for(int j = 0; j < (int)G[u.b].size(); j++){
                    for(int k = 0; k < (int)G[u.c].size(); k++){
                        int p1 = G[u.a][i], p2 = G[u.b][j], p3 = G[u.c][k];
                        //check conflict
                        if(vis[p1][p2][p3] == 1) continue;
                        if(p1 == p2 || p1 == p3 || p2 == p3) continue;
                        if(p1 == u.b && p2 == u.a) continue;
                        if(p1 == u.c && p3 == u.a) continue;
                        if(p2 == u.c && p3 == u.b) continue;

                        dis_s[p1][p2][p3] = dis_s[u.a][u.b][u.c] + 1;

                        if(vis[p1][p2][p3] == 2){
                            ans = dis_s[p1][p2][p3] + dis_t[p1][p2][p3];
                            return;
                        }
                        else if(vis[p1][p2][p3] == 0){
                            vis[p1][p2][p3] = 1;
                            que_s.push(Pos(p1, p2, p3));
                        }
                    }
                }
            }
        }

        if(!que_t.empty()){
            Pos v = que_t.front();
            que_t.pop();
            if(vis[v.a][v.b][v.c] == 1){
                ans = dis_t[v.a][v.b][v.c] + dis_s[v.a][v.b][v.c];
                return;
            }

            for(int i = 0; i < (int)G[v.a].size(); i++){
                for(int j = 0; j < (int)G[v.b].size(); j++){
                    for(int k = 0; k < (int)G[v.c].size(); k++){
                        int p1 = G[v.a][i], p2 = G[v.b][j], p3 = G[v.c][k];
                        //check conflict
                        if(vis[p1][p2][p3] == 2) continue;
                        if(p1 == p2 || p1 == p3 || p2 == p3) continue;
                        if(p1 == v.b && p2 == v.a) continue;
                        if(p1 == v.c && p3 == v.a) continue;
                        if(p2 == v.c && p3 == v.b) continue;

                        dis_t[p1][p2][p3] = dis_t[v.a][v.b][v.c] + 1;

                        if(vis[p1][p2][p3] == 1){
                            ans = dis_t[p1][p2][p3] + dis_s[p1][p2][p3];
                            return;
                        }
                        else if(vis[p1][p2][p3] == 0){
                            vis[p1][p2][p3] = 2;
                            que_t.push(Pos(p1, p2, p3));
                        }
                    }
                }
            }
        }
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> w >> h >> n){
        if(w == 0 && h == 0 && n == 0) break;
        getchar();
        cnt = 0;
        memset(id, -1, sizeof(id));
        memset(vis, 0, sizeof(vis));
        memset(dis_s, 0, sizeof(dis_s));
        memset(dis_t, 0, sizeof(dis_t));
        for(int i = 0; i < 200; i++){
            G[i].clear();
        }

        for(int i = 0; i < h; i++){
            fgets(d[i], maxn, stdin);
        }

        for(int i = 0; i < h; i++){
            for(int j = 0; j < w; j++){
                if(d[i][j] != '#'){
                    x[cnt] = i, y[cnt] = j, id[i][j] = cnt;
                    if(islower(d[i][j])) s[d[i][j] - 'a'] = cnt;
                    if(isupper(d[i][j])) t[d[i][j] - 'A'] = cnt;
                    cnt++;
                }
            }
        }

        for(int i = 0; i < cnt; i++){
            for(int dir = 0; dir < 5; dir++){
                int nx = x[i] + dx[dir], ny = y[i] + dy[dir];
                //"Outermost cells of a map are walls" means we don't need to check out-of-bound
                if(d[nx][ny] != '#') G[i].push_back(id[nx][ny]);
            }
        }

        //add fake nodes
        if(n <= 2){
            G[cnt].push_back(cnt);
            s[2] = t[2] = cnt;
            cnt++;
        } 
        if(n <= 1){
            G[cnt].push_back(cnt);
            s[1] = t[1] = cnt;
            cnt++;
        }
        
        ans = -1;
        adv_bfs();
        cout << ans << endl;
    }
    return 0;
}
```
