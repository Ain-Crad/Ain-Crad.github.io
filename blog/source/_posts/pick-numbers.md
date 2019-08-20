---
title: pick numbers-DFS
date: 2018-08-11 17:23:02
categories:
- ACM
- DFS
tags:
- DFS
---
从$n$个整数中任选$k$个整数相加，求得到的一系列和中为素数的共有多少种．
<!-- more -->
# 链接
[NOIP2002](http://acmoj.shu.edu.cn/problem/264/)
# 题目
已知$n$个整数$x1, x2,...,xn$，以及一个整数$k$．从$n$个整数中任选$k$个整数相加，可分别得到一系列的和．例如当$n = 4, k = 3$, $4$个整数分别为$3, 7, 12, 19$时，可得到全部的组合与它们的和为：
$3 + 7 + 12 = 22$
$3 + 7 + 19 = 29$
$7 + 12 + 19 = 38$
$3 + 12 + 19 = 34$
现在，要求你计算和为素数共有多少种．
# 思路
DFS对每一种情况遍历．
# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sat 11 Aug 16:32:41 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxn = 30;
int n, k;
int d[maxn];
int ans = 0;
int sum = 0;

bool is_prime(){
    if(sum < 2)
        return false;
    int tmp = sqrt(sum);
    for(int i = 2; i <= tmp; i++){
        if(!(sum % i)) 
            return false;
    }
    return true;
}

void dfs(int now, int dep){
    if(dep == k){
        if(is_prime())
            ans++;
        return;
    }
    if(now == n){
        return;
    }
    for(int i = now; i < n; i++){
        sum += d[i];
        dfs(i + 1, dep + 1);
        sum -= d[i];
    }
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        //freopen("in.txt", "r", stdin);
    #endif
    
    while(cin >> n >> k){
        ans = 0;
        sum = 0;
        //cout << n << " " << k << endl;
        for(int i = 0; i < n; i++){
            cin >> d[i];
        }
        dfs(0, 0);
        cout << ans << endl;
    }
    
    return 0;
}
```
