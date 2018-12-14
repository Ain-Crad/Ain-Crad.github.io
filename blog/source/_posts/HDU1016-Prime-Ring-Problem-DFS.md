---
title: HDU1016-Prime Ring Problem-DFS
date: 2018-08-12 23:16:27
categories:
- ACM
- DFS
tags:
- DFS
---
渐渐对深搜类的题有了自信, 虽然还只能做一些简单的题, 但是感觉还不错٩('ω')و .
题目描述：由$1-n$这$n$个自然数组成一个环,要求任意相邻两数之和为素数, 按顺序输出这些数.
<!-- more -->
## 链接
[HDU1016-Prime Ring Problem](http://acm.hdu.edu.cn/showproblem.php?pid=1016)
## 原题
A ring is compose of $n$ circles as shown in diagram. Put natural number $1, 2,...,n$ into each circle separately, and the sum of numbers in two adjacent circles should be a prime.
Note: the number of first circle should always be 1.
![example](/dfs.png)
## 题解
DFS遍历每一种情况.
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun 12 Aug 22:10:25 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxn = 30;
int n;
vector<int> vec;
bool vis[maxn];

void ans_print(){
    int cnt = 1;
    for(auto x : vec){
        if(cnt == 1) cout << x;
        else cout << " " << x;
        cnt++;
    }
    cout << endl;
}

bool is_prime(int x){
    if(x < 2) return false;
    int tmp = sqrt(x);
    for(int i = 2; i <= tmp; i++){
        if(!(x % i)) return false;
    }
    return true;
}

void dfs(int dep){
    if(dep == n - 1){
        for(int i = 2; i <= n; i++){
            if(!vis[i] && is_prime(i + vec[dep - 1]) && is_prime(i + 1)){
                vec.push_back(i);
                ans_print();
                vec.pop_back();
            }
        }
        return;
    }
    for(int i = 2; i <= n; i++){
        if(!vis[i] && is_prime(i + vec[dep - 1])){
            vec.push_back(i);
            vis[i] = 1;
            dfs(dep + 1);
            vec.pop_back();
            vis[i] = 0;
        }
    }
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        //freopen("in.txt", "r", stdin);
    #endif
    int cnt = 1;
    while(cin >> n){
        vec.clear();
        memset(vis, 0, sizeof(0));
        printf("Case %d:\n", cnt++);
        vec.push_back(1);
        vis[1] = 1;
        dfs(1);
        cout << endl;
    }
    
    return 0;
}
```
