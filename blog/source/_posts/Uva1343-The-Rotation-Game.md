---
title: Uva1343-The Rotation Game-IDA*
date: 2019-01-13 11:13:57
categories:
- ACM
- IDA*
tags:
---
如下图所以，#字型方格中填充有数字1，2，3。每种数字的个数为8，每行每列都可以“滚动”，实际上是指每行每列的数组可以左右移位。求使得中间8个小格子变为同一种数字的“滚动”方式，要求步数最少，步数相同的情况下滚动序列的字典序最小。

_我发现IDA\*好像因为深度限制以及估价函数的存在而不需要进行判重。_
![game](/game.png)
<!--more-->

# 链接
题目链接：[Uva1343-The Rotation Game](https://vjudge.net/problem/UVA-1343)
参考链接：[Rujia Liu's repository](https://github.com/aoapc-book/aoapc-bac2nd/blob/master/ch7/UVa1343.cpp)

# 题解
一开始想用BFS进行状态空间搜索，但是状态数总共有``24!/(8!*8!*8!) = 9465511770``种情况，而且我也不知道怎样构造哈希比较合适=　=。刘汝佳老师的IDA\*实现方法代码简单清晰，而且很自然的满足了题目中的步数最少，字典序最小的要求。__而且我发现IDA\*好像因为深度的限制和估价函数的存在而不需要考虑判重的问题。__

# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun 13 Jan 10:18:02 CST 2019
 *
 */

/* 
      00    01
      02    03
04 05 06 07 08 09 10
      11    12
13 14 15 16 17 18 19
      20    21
      22    23
*/

#include <bits/stdc++.h>

using namespace std;

int d[24];
int id[8][7] = {
    {0, 2, 6, 11, 15, 20, 22},
    {1, 3, 8, 12, 17, 21, 23},
    {10, 9, 8, 7, 6, 5, 4},
    {19, 18, 17, 16, 15, 14, 13},
    {23, 21, 17, 12, 8, 3, 1},
    {22, 20, 15, 11, 6, 2, 0},
    {13, 14, 15, 16, 17, 18, 19},
    {4, 5, 6, 7, 8, 9, 10}
};
int reff[8] = {5, 4, 7, 6, 1, 0, 3, 2}; // reverse lines of each line
int center[8] = {6, 7, 8, 11, 12, 15, 16, 17}; //center squares's real idex in array d
char ans[1000];
int res;

bool ok(){
    for(int i = 0; i < 7; i++){
        if(d[center[i]] != d[center[i + 1]]) return false;
    }
    return true;
}

void move(int p){
    int tmp = d[id[p][0]];
    for(int i = 0; i < 6; i++){
        d[id[p][i]] = d[id[p][i + 1]];
    }
    d[id[p][6]] = tmp;
}

int cal(int x){
    int sum = 0;
    for(int i = 0; i < 8; i++){
        if(d[center[i]] != x) sum++;
    }
    return sum;
}

int h(){
    return min(min(cal(1), cal(2)), cal(3));
}

bool dfs(int dep, int maxd){
    if(dep + h() > maxd) return false;
    if(dep >= maxd){
        if(ok()){
            res = d[center[0]];
            for(int i = 0; i < dep; i++){
                cout << ans[i];
            }
            cout << endl;
            cout << res << endl;
            return true;
        }
        return false;
    }

    for(int i = 0; i < 8; i++){
        ans[dep] = 'A' + i;
        move(i);
        if(dfs(dep + 1, maxd)) return true;
        move(reff[i]);
    }
    return false;
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(1){
        cin >> d[0];
        if(d[0] == 0) break;
        for(int i = 1; i < 24; i++){
            cin >> d[i];
        }

        if(ok()){
            cout << "No moves needed" << endl;
            cout << d[center[0]] << endl;
            continue;
        }
        int maxd;
        for(maxd = 1; ; maxd++){
            if(dfs(0, maxd)) break;
        }
    }
    
    return 0;
}
```