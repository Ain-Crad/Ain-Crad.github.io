---
title: Uva1374-Power Calculus-IDA*
date: 2019-02-08 16:36:27
categories:
- ACM
- IDA*
tags:
- IDA*
---
给定一个数n，求从$x$计算到$x^n$至少需要多少步，允许得到的各中间变量之间进行乘法和除法。
<!--more-->
# 链接
题目链接：[Uva1374-Power Calculus](https://vjudge.net/problem/UVA-1374)

# 题解
看到这道题，最开始想到的是快速幂的计算方法。但是快速幂计算其实是分为“计算过程数据”和“有效过程数据相乘”两部分构成的，步数不一定最少，而且，这道题目中允许除法的运算。所以不能将快速幂的计算方法的过程步骤作为最少步数的结果。
采用递归的方式进行搜索，由于具体的步数即深度不清楚，所以适合采用迭代加深搜索来做。同时将``maxVal * pow(2, maxd - d)``与n的大小关系作为剪枝的条件。其中maxVal为当前中间变量构成的序列中的最大值。若上式的值仍小于n，那么在当前的深度限制下不可能达到n值，需要进行剪枝。

**用位运算``maxVal << maxd - d``的方式来代替``maxVal * pow(2, maxd - d)``的运算方式有助于提升速度。**

# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Fri  8 Feb 16:18:30 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 1007;
int a[maxn];
int n;
int res;

int findMaxVal(int dep){
    int ans = 0;
    for(int i = 0; i <= dep; i++){
        ans = max(ans, a[i]);
    }
    return ans;
}

bool dfs(int dep, int maxd){
    if(dep >= maxd){
        if(a[dep] == n) return true;
        else return false;
    }

    //prune
    int maxVal = findMaxVal(dep);
    if(maxVal << (maxd - dep) < n) return false;

    for(int i = dep; i >= 0; i--){
        a[dep + 1] = a[dep] + a[i];
        if(dfs(dep + 1, maxd)) return true;
        if(a[dep] - a[i] > 0){
            a[dep + 1] = a[dep] - a[i];
            if(dfs(dep + 1, maxd)) return true;
        }
    }

    return false;
}

int main(){
    #ifndef    ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> n){
        if(n == 0) break;
        if(n == 1){
            cout << 0 << endl;
            continue;
        }

        a[0] = 1;
        for(int maxd = 1; maxd <= maxn; maxd++){
            if(dfs(0, maxd)){
                res = maxd;
                break;
            }
        }

        cout << res << endl;
    }
    return 0;
}
```