---
title: Uva12166-Equilibrium Mobile-思维+二叉树
date: 2018-12-04 10:40:03
categories:
- ACM
tags:
- 思维
- 二叉树
---
吹爆思维好题！给定一个深度不超过16的二叉树，代表一个天平，天平两端可能是递归的子天平也可能是砝码。每个砝码的质量已知，求至少修改多少个砝码的质量才能让天平平衡。
![balance](/balance.png)
如图，把7改成3天平平衡。

## 链接
题目链接[Uva12166-Equilibrium Mobile](https://vjudge.net/problem/UVA-12166)

参考链接[crazysillynerd's csdn blog](https://blog.csdn.net/crazysillynerd/article/details/43876123)

## 题解
想要修改砝码使天平平衡，需要找其中一个砝码作为基准，用它来调整整个天平。假设基准砝码的质量为w，它在二叉树中的深度为d，则用它调整后的整个天平的重量为$w * pow(2，d)$。
可能存在一些砝码，当以他们为基准时，调整后天平的总重量相同。那么这些砝码的数量就代表了保持不变的砝码数量，总砝码数量减去保持不变的砝码数量就是需要修改的砝码数量。
用DFS构建二叉树，对每一个叶节点计算调整后的天平总重量；用STL中的map来维护不同总重量所代表的砝码个数。

```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue  4 Dec 10:02:37 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

int t;
int sum;
map<long long, int> mp;
string s;

void dfs(int st, int ed, int depth){
    //cout << st << " " << ed << " " << depth << endl;
    if(s[st] == '['){
        int p = 0;
        for(int i = st + 1; i <= ed; i++){
            if(s[i] == '[') p++;
            if(s[i] == ']') p--;
            if(s[i] == ',' && p == 0){
                //cout << st + 1 << " " << i - 1 << " " << depth + 1 << endl;
                dfs(st + 1, i - 1, depth + 1);
                //cout << i + 1 << " " << ed - 1 << " " << depth + 1 << endl;
                dfs(i + 1, ed - 1, depth + 1);
            }
        }
    }
    else{
        long long w = 0;
        for(int i = st; i <= ed; i++){
            w *= 10;
            w += s[i] - '0';
        }
        //cout << w << endl;
        sum++;
        mp[w << depth]++;
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    cin >> t;
    while(t--){
        mp.clear();
        sum = 0;

        cin >> s;
        dfs(0, s.length() - 1, 0);

        int ans = 0;
        for(auto x : mp){
            ans = max(ans, x.second);
        }
        cout << sum - ans << endl;
    }
    return 0;
}
```
