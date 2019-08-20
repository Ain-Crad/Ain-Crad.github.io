---
title: Maze
date: 2019-03-26 13:23:44
categories:
- ACM
tags:
- BFS
---
第十届蓝桥杯省赛D题，迷宫求从(0，0)－>(n，m)的最短路径。
本来如果只是求最短路的话倒没什么，一遍BFS就可以解决。但是要求如果存在多条最短路，取路径字典序最小的那条。其中路径由``U D L R``四种字符组成，分别表示``上 下 左 右``。
譬如，下面这个迷宫的最短路为``DRRURRDDDR``。
![maze](/maze.png)
<!--more-->
# 题解
*这道题和之前整理过的[Uva1599](https://ain-crad.github.io/2018/11/18/Uva1599-Ideal-Path/)有点类似。*
一种策略是和Uva1599的做法一样，先从终点开始反向求一遍BFS，再根据距离信息从起点出发选择方向字典序小的走直到终点。
和Uva1599不同的是，这道题中每个位置相邻的四个可走方向优先级固定，且不存在优先级相同的情况，所以另一种策略是直接从起点BFS每次优先把方向字典序小的位置加入队列中，并记录路径。

# 代码
*题目中给定了一个maze.txt，代码是根据文件中的迷宫来写的。*
*[maze.txt以及原题pdf链接](https://github.com/Ain-Crad/ACM/tree/master/%E7%AC%AC%E5%8D%81%E5%B1%8A%E8%93%9D%E6%A1%A5%E6%9D%AF/Problems)*
## 策略1
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 26 Mar 13:55:33 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 100;
char maze[maxn][maxn];
int dis[maxn][maxn];
bool vis[maxn][maxn];
int n = 30, m = 50;
int dx[] = {1, 0, 0, -1};
int dy[] = {0, -1, 1, 0};
char dir[] = {'D', 'L', 'R', 'U'};
struct Node{
    int x, y;
    Node(int x = 0, int y = 0):x(x), y(y){}
};

void back_bfs(){
    queue<Node> que;
    que.push(Node(n - 1, m - 1));
    dis[n - 1][m - 1] = 0;
    vis[n - 1][m - 1] = 1;

    while(!que.empty()){
        Node node = que.front();
        que.pop();
        for(int i = 0; i < 4; i++){
            int nx = node.x + dx[i], ny = node.y + dy[i];
            if(nx >= 0 && nx < n && ny >= 0 && ny < m && !vis[nx][ny] && maze[nx][ny] == '0'){
                que.push(Node(nx, ny));
                dis[nx][ny] = dis[node.x][node.y] + 1;
                vis[nx][ny] = 1;
            }
        }
    }
}

void bfs(){
    vector<char> vec;
    queue<Node> que;
    que.push(Node(0, 0));
    int len = dis[0][0];
    for(int k = 0; k < len; k++){
        Node node = que.front();
        que.pop();
        if(node.x == n - 1 && node.y == m - 1) break;
        for(int i = 0; i < 4; i++){
            int nx = node.x + dx[i], ny = node.y + dy[i];
            if(nx >= 0 && nx < n && ny >= 0 && ny < m && dis[nx][ny] == dis[node.x][node.y] - 1){
                vec.push_back(dir[i]);
                que.push(Node(nx, ny));
                break;
            }
        }
    }

    for(auto x : vec){
        cout << x;
    }
    cout << endl;
}

int main(){
    freopen("maze.txt", "r", stdin);
    memset(dis, -1, sizeof(dis));
    memset(vis, 0, sizeof(vis));
    for(int i = 0; i < n; i++){
        gets(maze[i]);
    }
    back_bfs();
    bfs();

    return 0;
}
```
## 策略2
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 26 Mar 20:02:33 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 100;
int n = 30, m = 50;
char maze[maxn][maxn];
bool vis[maxn][maxn];
int dis[maxn][maxn];
int dx[] = {1, 0, 0, -1};
int dy[] = {0, -1, 1, 0};

struct Node{
    int x, y;
    Node(int x = 0, int y = 0):x(x), y(y){}
};
Node pre[maxn][maxn];

void bfs(){
    queue<Node> que;
    que.push(Node(0, 0));
    vis[0][0] = 1;
    dis[0][0] = 0;

    while(!que.empty()){
        Node node = que.front();
        que.pop();
        if(node.x == n - 1 && node.y == m - 1) break;
        for(int i = 0; i < 4; i++){
            int nx = node.x + dx[i], ny = node.y + dy[i];
            if(nx >= 0 && nx < n && ny >= 0 && ny < m && !vis[nx][ny] && maze[nx][ny] == '0'){
                dis[nx][ny] = dis[node.x][node.y] + 1;
                vis[nx][ny] = 1;
                pre[nx][ny] = node;
                que.push(Node(nx, ny));
            }
        }
    }
}

void printPath(){
    vector<char> vec;
    int len = dis[n - 1][m - 1];
    int x = n - 1, y = m - 1;
    for(int i = 0; i < len; i++){
        if(pre[x][y].x - x == -1 && pre[x][y].y - y == 0)
            vec.push_back('D');
        else if(pre[x][y].x - x == 0 && pre[x][y].y - y == 1)
            vec.push_back('L');
        else if(pre[x][y].x - x == 0 && pre[x][y].y - y == -1)
            vec.push_back('R');
        else if(pre[x][y].x - x == 1 && pre[x][y].y - y == 0)
            vec.push_back('U');

        //坑点，卡了好久，一开始直接另x = pre[x][y].x, y = pre[x][y].y, 这样就在赋值过程中改变了原始值。
        int x_tmp = pre[x][y].x, y_tmp = pre[x][y].y; 
        x = x_tmp, y = y_tmp;
    }

    reverse(vec.begin(), vec.end());

    for(auto x : vec){
        cout << x;
    }
    cout << endl;
}

int main(){
    freopen("maze.txt", "r", stdin);
    freopen("c1out.txt", "w", stdout);
    memset(vis, 0, sizeof(vis));
    memset(dis, -1, sizeof(dis));
    for(int i = 0; i < n; i++){
        fgets(maze[i], sizeof(maze[i]), stdin);
    }

    bfs();
    printPath();

    return 0;
}
```
