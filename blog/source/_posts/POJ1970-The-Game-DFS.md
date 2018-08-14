---
title: POJ1970-The Game-DFS
date: 2018-08-14 19:39:48
categories:
- ACM
tags:
- POJ
- DFS
- 迷宫问题
---
在一个$19×19$的棋盘上有一局五子棋的棋局，要求判断是白棋获胜还是黑棋获胜，并输出获胜一方获胜棋子部分的最左上一个棋子的位置．
相比其它迷宫问题稍微变化了一点点，多了一个方向因素，通过这道题对迷宫问题和DFS有了更进一步的理解．
<!-- more -->
## 链接
[POJ1970-The Game](http://poj.org/problem?id=1970)
## 原题描述
A game of Renju is played on a 19*19 board by two players. One player uses black stones and the other uses white stones. The game begins in an empty board and two players alternate in placing black stones and white stones. Black always goes first. There are 19 horizontal lines and 19 vertical lines in the board and the stones are placed on the intersections of the lines. 
Horizontal lines are marked 1, 2, ..., 19 from up to down and vertical lines are marked 1, 2, ..., 19 from left to right. 
![example](/Renju.png)
The objective of this game is to put five stones of the same color consecutively along a horizontal, vertical, or diagonal line. So, black wins in the above figure. But, a player does not win the game if more than five stones of the same color were put consecutively. 
Given a configuration of the game, write a program to determine whether white has won or black has won or nobody has won yet. There will be no input data where the black and the white both win at the same time. Also there will be no input data where the white or the black wins in more than one place. 
##　题解
相比于其它迷宫问题的搜索，这个问题要求每一次DFS始终沿一个方向进行搜索判断，判断出一方的某一个方向上的棋子个数等于5时该方获胜，所以在DFS函数上加上一个控制方向的参数，并把方向的选择放在DFS外部（main函数中）．还有一个需要注意的地方是连续棋子个数大于5的情况不算获胜．
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 14 Aug 17:36:47 CST 2018
 *
 */
 
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

int d[25][25];
int t;
/**/
//搜索方向为向下，向右，右斜上，右斜下（方便获得最左上的棋子坐标）
int dx[4] = {1, 0, -1, 1};
int dy[4] = {0, 1, 1, 1};
/**/
int cnt = 0;

//参数k为方向参数
void dfs(int x, int y, int k){
    cnt++;
    int nx = x + dx[k], ny = y + dy[k];
    if(nx >= 1 && nx <= 19 && ny >= 1 && ny <= 19 && d[nx][ny] == d[x][y])
        dfs(nx, ny, k);
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    
    cin >> t;
    while(t--){
        int leftx = 0, lefty = 0;
        int win = 0;
        for(int i = 1; i <= 19; i++){
            for(int j = 1; j <= 19; j++){
                cin >> d[i][j];
                //cout << d[i][j] << " ";
            }
            //cout << endl;
        }
        for(int i = 1; i <= 19; i++){
            for(int j = 1; j <= 19; j++){
                if(!d[i][j]) continue;
                for(int k = 0; k < 4; k++){
                    cnt = 0;
                    dfs(i, j, k);
                    if(cnt == 5){
                        int xx = i - dx[k], yy = j - dy[k];
                        //判断是否连续棋子个数大于5
                        if(xx >= 1 && xx <= 19 && yy >= 1 && yy <= 19){
                            if(d[xx][yy] != d[i][j]){
                                win = d[i][j], leftx = i, lefty = j;
                                break;
                            }
                        }
                        else{
                            win = d[i][j], leftx = i, lefty = j;
                            break;
                        }
                    }
                }
                if(win) break;
            }
            if(win) break;
        }
        cout << win << endl;
        if(win) cout << leftx << " " << lefty << endl;
    }
    
    return 0;
}
```