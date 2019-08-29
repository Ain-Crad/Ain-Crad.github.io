---
title: Kick Start-2019D-X or What?
date: 2019-08-02 11:19:45
categories:
- Google Kick Start
tags:
---
Kick Start-2019D 签到失败 (◉ω◉ )
<!--more-->
# 题目链接
[Kick Start-2019D-X or What?](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000051061/0000000000161426)

# 题意
给定 $N$ 个非负整数组成的序列 $A$，求最大的区间，满足区间内所有数异或的结果是 $xor-even$，即结果的二进制形式中 $1$ 的个数为偶数个。
每组数据包含 $N$ 个非负整数以及 $Q$ 个修改，每次修改将位置 $P_i$ 处（下标从0开始）的元素替换成 $V_i$。输出每次修改后满足 $xor-even$ 的最大区间中元素的个数。

# 题解
假设两个数分别为 $a$，$b$，它们二进制形式中 $1$ 的个数分别为 $i$，$j$。那么 $a$ 和 $b$ 异或后结果的二进制形式中 $1$ 的个数为 ``i + j - k * 2``，其中 $k=0,1,2,3...$。所以有如下结论：
- 若两个数都满足 $xor-even$，那么两个数异或的结果也满足 $xor-even$。
- 若两个数都满足 $xor-odd$，那么两个数异或的结果满足 $xor-even$。
- 若两个数一个满足 $xor-odd$ 另一个满足 $xor-even$，那么两个数异或的结果满足 $xor-odd$。

所以对于每次修改我们只要统计所有的 $N$ 个数中满足 $xor-odd$ 的数的个数是偶数还是奇数，并记录所有 $xor-odd$ 的数的下标。若 $xor-odd$ 的个数是偶数，那么结果是整个区间的长度；若 $xor-odd$ 的个数是奇数，那么结果是 ``max( length([0, last_Xor-odd)), length((first_Xor-odd, N - 1]) )``，其中 ``last_Xor-odd`` 表示满足 $xor-odd$ 的最后一个数的下标，``first_Xor-odd`` 表示满足 $xor-odd$ 的第一个数的下标。 

# 代码
```C++
#include <iostream>
#include <set>

#include <cstdio>

using namespace std;

const int maxn = 1e5 + 7;
int T;
int n, q;
int a[maxn];
set<int> pos;

bool checkEven(int x){
    int cnt = 0;
    while(x > 0){
        if(x & 1) cnt++;
        x >>= 1;
    }
    if(cnt % 2 == 0) return true;
    else return false;
}

int main(){
    //freopen("in.txt", "r", stdin);
    cin >> T;
    int Case = 0;

    while(T--){
        pos.clear();
        cin >> n >> q;
        for(int i = 0; i < n; i++){
            cin >> a[i];
            if(!checkEven(a[i])) pos.insert(i);
        }

        cout << "Case #" << ++Case << ":"; 
        for(int i = 0; i < q; i++){
            int p, v;
            cin >> p >> v;

            if(!checkEven(v)) pos.insert(p);
            else pos.erase(p);

            if(pos.size() % 2 == 0){
                cout << " " << n;
            }
            else{
                int l = *pos.begin();
                int r = *pos.rbegin();
                cout << " " << max(n - 1 - l, r);
            }
        }
        cout << endl;
    }

    return 0;
}
```
