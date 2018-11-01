---
title: Uva816-Abbott's Revenge-BFS
date: 2018-11-01 16:18:17
categories:
- ACM
tags:
- BFS
---
一道比较别致的BFS搜索题，题目规定了在迷宫里沿特定方向到达某点后只能再沿特定一个或几个方向继续前进，而不是平常的任意四个方向都可以前进。
在输出格式上WA了很久，，什么时候我才能“Bug Free”啊，哭QAQ
<!--more-->
## 链接
[Uva816-Abbott's Revenge](https://vjudge.net/problem/UVA-816)
## 题目描述
迷宫找最短路径问题，不同于以往的障碍物形式，题目规定沿某一方向进入节点（东，南，西，北）之后只能按照给定的方向离开该节点（前进，左转，右转）。给定起点和终点，要求输出最短路径。
## 题解
带方向的BFS，在构建地图的时候添加上方向和转向两个维度。在更新节点距离和记录路径的时候加上方向维度。
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Wed 31 Oct 07:40:43 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxn = 10, dir = 4, turn = 3;
char dirs[] = "NESW", turns[] = "FLR";
int mp[maxn][maxn][dir][turn];
int dx[] = {-1, 0, 1, 0};
int dy[] = {0, 1, 0, -1};
int dis[maxn][maxn][dir];
struct Node{
    int x;
    int y;
    int dir;
    Node(){}
    Node(int xx, int yy, int ddir): x(xx), y(yy), dir(ddir){}
};
Node path[maxn][maxn][dir];
int stx, sty, edx, edy;
char stdir;

int finddir(char c){
    return strchr(dirs, c) - dirs;
}

int findturn(char c){
    return strchr(turns, c) - turns;
}


void read(){
    int x, y, dr, tr;
    string s;
    while(1){
        cin >> x;
        if(x == 0) break;
        cin >> y;
        for(;;){
            cin >> s;
            //cout << s << endl;
            if(s == "*") break;
            dr = finddir(s[0]);
            //cout << dr << endl;
            for(size_t i = 1; i < s.length(); i++){
                tr = findturn(s[i]);
                //cout << tr << endl;
                mp[x][y][dr][tr] = 1;
            }
        }
    }
}

bool inside(int x, int y){
    if(x >= 1 && x <= 9 && y >= 1 && y <= 9) return true;
    else return false;
}

int trans(int turn, int dir){
    if(turn == 0) return dir;
    else if(turn == 1) return (dir + 3) % 4;
    else return (dir + 1) % 4;
}

void print(Node node){
    //cout << "GodBlessMe!" << endl;
    vector<Node> vec;
    vec.push_back(node);
    Node nd;
    while(1){
        nd = path[node.x][node.y][node.dir];
        if(nd.x == -1) break;
        vec.push_back(nd);
        node = nd;
    }
    vec.push_back(Node(stx, sty, finddir(stdir)));
    reverse(vec.begin(), vec.end());
    
    int cnt = 0;
    for(auto x : vec){
        if(cnt % 10 == 0) cout << " ";
        cout << " (" << x.x << "," << x.y << ")";
        if(++cnt % 10 == 0) cout << "\n";
    }
    if(vec.size() % 10 != 0) cout << endl;
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    
    string s;
    bool ok;
    while(cin >> s){
        if(s == "END") break;
        memset(mp, 0, sizeof(mp));
        memset(dis, -1, sizeof(dis));
        memset(path, -1, sizeof(path));
        ok = false;
        
        cin >> stx >> sty >> stdir >> edx >> edy;
        read();
        queue<Node> que;
        Node node;
        int num = finddir(stdir);
        node.x = stx + dx[num], node.y = sty + dy[num], node.dir = num;
        que.push(node);
        dis[node.x][node.y][node.dir] = 0;
        while(!que.empty()){
            Node nd = que.front();
            if(nd.x == edx && nd.y == edy){
                ok = true;
                break;
            }
            que.pop();
            for(int i = 0; i < 3; i++){
                int k = trans(i, nd.dir);
                int nx = nd.x + dx[k], ny = nd.y + dy[k];
                if(mp[nd.x][nd.y][nd.dir][i] && inside(nx, ny) && dis[nx][ny][k] == -1){
                    dis[nx][ny][k] = dis[nd.x][nd.y][nd.dir] + 1;
                    que.push(Node(nx, ny, k));
                    path[nx][ny][k] = nd;
                }
            }
        }
        cout << s << endl;
        if(ok) print(que.front());
        else cout << "  No Solution Possible" << endl;
    }
    
    return 0;
}
```