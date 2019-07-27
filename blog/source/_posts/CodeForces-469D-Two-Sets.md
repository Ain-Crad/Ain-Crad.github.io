---
title: CodeForces-469D-Two Sets
date: 2019-07-27 18:46:03
categories:
- ACM
- 贪心
tags:
- 思维
- 贪心
---
<!--more-->
# 题目链接
　　[CodeForces-496D-Two Sets](http://codeforces.com/problemset/problem/469/D)

# 题意
　　有 $n$ 个不同的整数，要把它们分到 $A$ 和 $B$ 两个集合中。要求满足以下两个条件：
- 如果 $x$ 属于集合 $A$，那么 $a-x$ 也必须属于集合$A$。
- 如果 $x$ 属于集合 $B$，那么 $b-x$ 也必须属于集合$B$。

# 题解
　　贪心法。首先对这 $n$ 个不同的整数进行排序，顺序进行判断，假设 $b > a$，那么对于 $x$ 来说，先判断 $b-x$ 是否存在，之后再判断 $a-x$ 是否存在。因为 $b > a$，所以 $b-x>a-x$，应该优先将较大的一方也就是 $b-x$　与 $x$ 配对，因为由于序列递增若先将 $a-x$ 与 $x$，配对，那么 $b-x$ 此后将没有机会再次配对。
　　查找 $b-x$ 和 $a-x$ 时使用二分搜索，这也是将数列进行排序的意义。

# 代码
```C++
#include <iostream>
#include <map>
#include <algorithm>

#include <cstdio>
#include <cstring>


using namespace std;

const int maxn = 1e5 + 7;
int n, a, b;
int d[maxn];
int dd[maxn];
bool res[maxn];
map<int, bool> mp;
bool flag = 0;
bool vis[maxn];

int bSearch(int x, int y, int v){
    int m;
    while(x < y){
        m = x + (y - x) / 2;
        if(dd[m] == v) return m;
        else if(dd[m] > v) y = m;
        else x = m + 1;
    }
    return -1;
}

int main(){
    //freopen("in.txt", "r", stdin);
    cin >> n >> a >> b;
    for(int i = 0; i < n; i++){
        cin >> d[i];
    }
    memset(vis, 0, sizeof(vis));
    memcpy(dd, d, sizeof(dd));
    sort(dd, dd + n);

    if(a > b){
        swap(a, b);
        flag = 1;
    }

    for(int i = 0; i < n; i++){
        if((b - dd[i] >= 0) && !vis[i]){
            int index = bSearch(0, n, b - dd[i]);
            if(index >= 0 && !vis[index]){
                mp[dd[i]] = 1;
                mp[b - dd[i]] = 1;
                vis[i] = 1;
                vis[index] = 1;
            }
        }
        if((a - dd[i] >= 0) && !vis[i]){
            int index = bSearch(0, n, a - dd[i]);
            if(index >= 0 && !vis[index]){
                mp[dd[i]] = 0;
                mp[a - dd[i]] = 0;
                vis[i] = 1;
                vis[index] = 1;
            }
        }

        if(!vis[i]){
            cout << "NO" << endl;
            return 0;
        }
    }

    for(int i = 0; i < n; i++){
        if(flag) res[i] = !mp[d[i]];
        else res[i] = mp[d[i]];
    }

    cout << "YES" << endl;
    for(int i = 0; i < n; i++){
        if(i == 0) cout << res[i];
        else cout << " " << res[i];
    }
    cout << endl;

    return 0;
}
```

# 参考链接
　　[Two Sets My greedy solution~~](http://codeforces.com/blog/entry/13880)