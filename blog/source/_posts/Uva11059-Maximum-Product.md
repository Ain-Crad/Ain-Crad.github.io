---
title: Uva11059-Maximum Product-暴力枚举
date: 2018-12-17 20:07:59
categories:
- ACM
- 暴力枚举
tags:
- 暴力枚举
---
输入$n$的元素组成的序列$S$，找出一个乘积最大的连续子序列。如果这个最大的乘积不是正数，输出0。$1 <=n <= 18，-10 <= S_i <= 10$。

## 链接
[Uva11059-Maximum Product](https://vjudge.net/problem/UVA-11059)

## 题解
**连续子序列有两个要素：起点和终点**。因此枚举起点和终点即可。

*注意用long long存储。*

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun 16 Dec 23:18:45 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 20;
int n;
int d[maxn];

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    int cnt = 0;
    while(cin >> n){
        for(int i = 0; i < n; i++){
            cin >> d[i];
        }
        
        long long res = 0, p = 1;
        for(int i = 0; i < n; i++){
            for(int j = i; j < n; j++){
                p = 1;
                for(int k = i; k <= j; k++){
                    p *= d[k];
                }
                res = max(res, p);
            }
        }        

        cout << "Case #" << ++cnt << ": The maximum product is " << res << "." << endl << endl;
    }
    return 0;
}
```
循环枚举部分还可以写成这种稍难理解但简单的形式：
```C++
for(int i = 0; i < n; i++){
    p = 1;
    for(int j = i; j < n; j++){
        p *= d[j];
        res = max(res, p);
    }
}
```