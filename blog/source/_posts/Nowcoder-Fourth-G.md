---
title: 牛客暑期ACM多校训练营(第四场)G题-Maxinum Mode-思维+map
date: 2018-07-29 20:06:46
categories:
- ACM
tags:
- 逆向思维
---
一道逆向思维的好题,比赛的时候没有做出来,赛后补题的时候按原来的思路写出来了,然而代码超时了= =,阅读了别人的代码后发现自己想复杂了,从逆向来考虑感觉非常巧妙,思路也很顺畅.从正向考虑反而要顾及很多情况,可能就是因为考虑的太多代码冗余就TLE了.
<!-- more -->
## 链接
[Nowcoder-Fourth-G](https://www.nowcoder.com/acm/contest/142/G)
## 题目描述
The mode of an integer sequence is the value that appears most often. Chiaki has n integers a1,a2,...,an. She woud like to delete exactly m of them such that: the rest integers have only one mode and the mode is maximum.
输入描述:
There are multiple test cases. The first line of input contains an integer T, indicating the number of test cases. For each test case:
The first line contains two integers n and m (1 ≤ n ≤ 105, 0 ≤ m < n) -- the length of the sequence and the number of integers to delete.
The second line contains n integers a1, a2, ..., an (1 ≤ ai ≤ 109) denoting the sequence.
It is guaranteed that the sum of all n does not exceed 106.
输出描述:
For each test case, output an integer denoting the only maximum mode, or -1 if Chiaki cannot achieve it.
输入:
5
5 0
2 2 3 3 4
5 1
2 2 3 3 4
5 2
2 2 3 3 4
5 3
2 2 3 3 4
5 4
2 2 3 3 4
输出:
-1
3
3
3
4
## 题解
题目大意是给定n个数,从这n个数中删掉m个数,使得出现次数最多的数只有一个且要求尽量大.求解这个出现次数最多的数.
思路:
map存数据及数据个数, 按键值从大到小排(实际上map会按键值从小到大自动排列,那么就从后往前判断就好了).然后不考虑删除数的情况而是考虑剩余数(逆向思维)的情况,首先判断剩余数的个数是否小于等于最大值的个数,若满足则最终的结果就是最大值;若不满足则把剩余的数进行分配,看是否可以在满足min(d[max] - 1,d[i])的分配条件下将剩余的数分配完(其中d[i]表示第i个数的个数,用d[max]表示了最大的数的个数),如果可以分配完则最终结果就是最大值.若最大值不能满足上述情况则考虑次大值,以此类推,若全都不满足则无解,输出-1.

## 代码
``` C++
#include <bits/stdc++.h>

using namespace std;

const int maxn = 1e5 + 7;
typedef pair<int, int> P;
P d[maxn];
int t;
int n, m;
map<int, int> mp;

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    
    scanf("%d", &t);
    while(t--){
        mp.clear();
        scanf("%d%d", &n, &m);
        int tmp;
        for(int i = 0; i < n; i++){
            scanf("%d", &tmp);
            mp[tmp]++;
        }
        
        int cnt = 0;
        for(auto x : mp){
            d[cnt].first = x.first;
            d[cnt].second = x.second;
            cnt++;
        }
        
        int flag = 0;
        int rem = n - m;//剩余数的个数
        int i;
        for(i = cnt - 1; i >= 0; i--){
            int res = rem - d[i].second;
            if(res <= 0){
                flag = 1;
                break;
            }
            for(int j = 0; j < cnt; j++){
                if(j == i) continue;
                res -= min(d[i].second - 1, d[j].second);
                if(res <= 0){
                    flag = 1;
                    break;
                }
            }
            if(flag) break;
        }
        
        if(flag) printf("%d\n", d[i].first);
        else printf("-1\n");
    }
    
    return 0;
}
```