---
title: 牛客暑期ACM多校训练营(第三场)A题-PACM Team-多维01背包+记录路径
date: 2018-07-26 23:16:37
categories:
- ACM
tags:
- DP
- 01背包
---
DP果然是最弱的，记录路径更是十分懵，在多校训练营里被虐的抬不起头，，，算了，只要不死，就会更强(赛亚人的特性其实也就是人的特性)

## 题目描述
Eddy was a contestant participating in ACM ICPC contests. ACM is short for Algorithm, Coding, Math. Since in the ACM contest, the most important knowledge is about algorithm, followed by coding(implementation ability), then math. However, in the ACM ICPC World Finals 2018, Eddy failed to solve a physics equation, which pushed him away from a potential medal.

Since then on, Eddy found that physics is actually the most important thing in the contest. Thus, he wants to form a team to guide the following contestants to conquer the PACM contests(PACM is short for Physics, Algorithm, Coding, Math). 
<!-- more -->
There are N candidate groups each composed of pi physics experts, ai algorithm experts, ci coding experts, mi math experts. For each group, Eddy can either invite all of them or none of them. If i-th team is invited, they will bring gi knowledge points which is calculated by Eddy's magic formula. Eddy believes that the higher the total knowledge points is, the better a team could place in a contest. But, Eddy doesn't want too many experts in the same area in the invited groups. Thus, the number of invited physics experts should not exceed P, and A for algorithm experts, C for coding experts, M for math experts.

Eddy is still busy in studying Physics. You come to help him to figure out which groups should be invited such that they doesn't exceed the constraint and will bring the most knowledge points in total.

输入描述:

The first line contains a positive integer N indicating the number of candidate groups.
Each of following N lines contains five space-separated integer pi, ai, ci, mi, gi indicating that i-th team consists of pi physics experts, ai algorithm experts, ci coding experts, mi math experts, and will bring gi knowledge points.
The last line contains four space-separated integer P, A, C, M indicating the maximum possible number of physics experts, algorithm experts, coding experts, and math experts, respectively.

 1 ≤ N ≤ 36
 0 ≤ pi,ai,ci,mi,gi ≤ 36
 0 ≤ P, A, C, M ≤ 36

输出描述:

The first line should contain a non-negative integer K indicating the number of invited groups.
The second line should contain K space-separated integer indicating the index of invited groups(groups are indexed from 0).

You can output index in any order as long as each index appears at most once. If there are multiple way to reach the most total knowledge points, you can output any one of them. If none of the groups will be invited, you could either output one line or output a blank line in the second line.

示例1:

输入:
2
1 0 2 1 10
1 0 2 1 21
1 0 2 1
输出:
1
1

示例2:

输入:
1
2 1 1 0 31
1 0 2 1

输出：
0

## 代码
``` C++
#include <bits/stdc++.h>

using namespace std;

const int maxn = 40;
int p[maxn], a[maxn], c[maxn], m[maxn];
int v[maxn];
short d[maxn][maxn][maxn][maxn][maxn];
int n;
int Pm, Am, Cm, Mm;
int used[maxn];//记录路径

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);

    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    memset(used, 0, sizeof(used));
    scanf("%d", &n);
    for(int i = 0; i < n; i++){
        scanf("%d%d%d%d%d", p + i, a + i, c + i, m + i, v + i);
    }

    scanf("%d%d%d%d", &Pm, &Am, &Cm, &Mm);
    for(int i = 0; i <= Pm; i++){
        for(int j = 0; j <= Am; j++){
            for(int k = 0; k <= Cm; k++){
                for(int l = 0; l <= Mm; l++){
                    d[0][i][j][k][l] = 0;
                }
            }
        }
    }
    for(int i = 0; i < n; i++){
        for(int j = 0; j <= Pm; j++){
            for(int k = 0; k <= Am; k++){
                for(int l = 0; l <= Cm; l++){
                    for(int q = 0; q <= Mm; q++){
                        d[i + 1][j][k][l][q] = d[i][j][k][l][q];
                        if(j >= p[i] && k >= a[i] && l >= c[i] && q >= m[i]){
                            int tmp = v[i] + d[i][j - p[i]][k - a[i]][l - c[i]][q - m[i]];
                            if(tmp > d[i + 1][j][k][l][q]) {
                                d[i + 1][j][k][l][q] = tmp;
                            }
                        }
                    }
                }
            }
        }
    }
    //记录路径部分
    for(int i = n - 1; i >= 0; i--){
        if(d[i + 1][Pm][Am][Cm][Mm] != d[i][Pm][Am][Cm][Mm]){
            used[i] = 1;
            Pm -= p[i];
            Am -= a[i];
            Cm -= c[i];
            Mm -= m[i];
        }
    }
    int cnt = 0;
    for(int i = 0; i < n; i++){
        if(used[i] == 1){
            cnt++;
        }
    }
    printf("%d\n", cnt);
    if(cnt){
        for(int i = 0; i < n; i++){
            if(used[i]) printf("%d ", i);
        }
    }
    printf("\n");
    return 0;
}
```
## 坑点
数据d的类型要是short类型，int类型会爆内存，char类型会数据溢出．

## 背包记录路径问题总结
### 方法一核心代码
``` C++
    for(int i = 0; i < n; i++){
        for(int j = 0; j <= W; j++){
            if(j < w[i]){
                d[i + 1][j] = d[i][j];
            }
            else{
                d[i + 1][j] = max(d[i][j], d[i][j - w[i]] + v[i]);
            }
        }
    }

    for(int i = n - 1; i >= 0; i--){
        if(d[i + 1][W] != d[i][W]){
            used[i] = 1;
            W -= w[i];
        }
    }
```
### 方法二核心代码
``` C++
    for(int i = 0; i < n; i++){
        for(int j = 0; j <= W; j++){
            d[i + 1][j] = d[i][j];
            if(j >= w[i]){
                int tmp = d[i][j - w[i]] + v[i];
                if(tmp > d[i + 1][j]){
                    d[i + 1][j] = tmp;
                    path[i][j] = 1;
                }
            }
        }
    }

    for(int i = n - 1; i >= 0; i--){
        if(path[i][W] == 1){
            used[i] = 1;
            W -= w[i];
        }
    }
```
