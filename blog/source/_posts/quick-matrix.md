---
title: 矩阵快速幂
date: 2018-05-14 09:41:44
categories:
- ACM
tags:
- math
---
最近在做一道DP的题时发现数据范围竟然是1e18，这样的话只能用log(n)复杂度的算法来实现了，于是从队长那里了解到了矩阵快速幂。在这里记录一下快速幂以及其它矩阵处理的模板。
矩阵快速幂模板参考自[eecrazy](https://github.com/eecrazy/ACM/blob/master/%E6%95%B0%E8%AE%BA/poj3233%E7%9F%A9%E9%98%B5%E5%BF%AB%E9%80%9F%E5%B9%82.cpp)
<!-- more -->
# 常数快速幂
```C++
typedef long long ll;
ll mod_pow(ll x, ll n, ll mod){
    ll res = 1;
    while(n > 0){
        if(n & 1) res *= x % mod;
        x = x * x % mod;
        n >>= 1;
    }
    return res;
}
```
# 矩阵快速幂
``` C++
struct Ma
{
    int m[maxn][maxn];
};

Ma a,per;
int n, mod;

void init()//矩阵初始化
{
    int i,j;
    for(i = 0; i < n; i++)
        for(j = 0; j < n; j++)
        {
            scanf("%d", &a.m[i][j]);
            a.m[i][j] %= mod;
            per.m[i][j] = (i == j);
        }
}

Ma add(Ma a,Ma b)//矩阵加法
{
    Ma c;
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < n; ++j)
        {
            c.m[i][j] = (a.m[i][j] + b.m[i][j]) % mod;
        }
    }
    return c;
}


Ma multi(Ma a,Ma b)//矩阵乘法
{
    Ma c;
    for(int i = 0; i < n; i++)
        for(int j = 0; j < n; j++)
        {
            c.m[i][j] = 0;
            for(int k = 0; k < n; k++)
            {
                c.m[i][j] += a.m[i][k] * b.m[k][j];
            }
            c.m[i][j] %= mod;
        }
    return c;
}

Ma quick_pow(long long x)//矩阵快速幂
{
    Ma res = per;
    Ma p = a;
    while(x)
    {
        if(x&1) res = multi(res,p);
        p = multi(p,p);
        x >>= 1;
    }
    return res;
}
```