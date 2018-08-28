---
title: 'CodeForces-569A-Music'
date: 2018-08-28 15:15:59
categories:
- ACM
tags:
- 思维
---
也是一道思维题，理解了题意就很简单，但是自己理解题意费了好长时间，，还好最后AC了，背景故事蛮有意思的2333.
## 链接
[CodeForces-569A-Music](http://codeforces.com/problemset/problem/569/A)
## 题意
下载一首时长为$T$的歌，首先下载$S$秒，然后边下载边播放，播放到还未下载的位置就立即返回歌的开头重新播放，求需要返回几次才能最终完整听完这首歌。歌的下载速率为每经过$q$秒下载$q - 1$秒的歌。
## 题解
一开始没有理解歌的下载速率的问题，是每经过$q$秒直接得到$q - 1$秒的歌，还是在$q$秒内一直在下载，最后累计得到$q-1$秒的歌，看了样例后发现是第二种，（好像也第二种也更符合实际）。平均每秒的下载速率是$(q-1)/q$，这不是一个整数，甚至很可能是无理数，那结果很可能就不精确了，尤其是最后刚好下载完也刚刚听完的那种情况，会多算或者少算一次，所以不能求出平均下载速率然后来模拟计算。
然后想到了一种思路，因为听的速率是大于下载的速率的，所以考虑最开始下载的$S$秒，求出这$S$秒被消耗完的时间$t$，$$t = \frac{S}{1 - \frac{q - 1}{q}}$$，化简得$$t = S * q$$.同时$t$也表示在这一时刻，听歌的进度正好赶上了下载的速度，此时判断$t$是否大于等于$T$，若大于等于则表示歌已下载完成，否则需返回歌的开头重新播放（此时的$S$变为$t$，因为已下载的进度现在是$t$）。
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Mon 27 Aug 13:48:09 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

long long T, S, q;

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    int cnt = 1;
    cin >> T >> S >> q;
    while(1){
        if(S * q >= T) break;
        else{
            S = S * q;
            cnt++;
        }
    }
    cout << cnt << endl;
    return 0;
}
```