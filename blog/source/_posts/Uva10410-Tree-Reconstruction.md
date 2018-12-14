---
title: Uva10410-Tree Reconstruction-树还原
date: 2018-12-14 20:02:42
categories:
- ACM
- 树
tags:
- 树
---
根据一棵树的BFS和DFS序列还原这颗树的结构。关于树的一种新的类型的题，mark。
<!--more-->
## 链接
题目链接[Uva10410-Tree Reconstruction](https://vjudge.net/problem/UVA-10410)
参考链接[20143605--pcx's Blog](https://www.cnblogs.com/20143605--pcx/p/4859424.html)和[Chengrui's Blog](https://wcr1996.com/2015/02/24/uva-10410-tree-reconstruction/)

## 题解
用BFS序列去分离DFS，首先根据BFS序列顺序记录每个节点的位置。子结点的下标一定比父结点的下标至少大于1，（根节点除外，根结点和第一个子结点的下标距离等于1）。
用栈维护DFS序列，根据条件不断判断栈中的top结点和新读入结点之间的位置关系，``若top结点下标 + 1 < 新结点下标，或者top结点是根节点``，则表示新结点是top的结点的一个子结点；``若top结点下标 + 1 = 新结点下标``，表示新结点和top结点之间是兄弟结点，同时表示当前top结点往下的分支已扫描完毕，pop出top结点；``若top结点下标 + 1 > 新结点下标``，表示新结点已不在top结点所在子树，同样pop出top结点。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Fri 14 Dec 19:36:24 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 1e3 + 7;
int n;
int pos[maxn];
stack<int> dfs;
vector<int> tree[maxn];

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> n){
        while(!dfs.empty()) dfs.pop();
        
        int x;
        for(int i = 0; i < n; i++){
            cin >> x;
            pos[x] = i;
            tree[i + 1].clear();
        }

        int root;
        cin >> root;
        dfs.push(root);
        for(int i = 1; i < n; i++){
            cin >> x;
            while(1){
                int u = dfs.top();
                if(u == root || pos[u] + 1 < pos[x]){
                    tree[u].push_back(x);
                    dfs.push(x);
                    break;
                }
                dfs.pop();
            }
        }

        for(int i = 1; i <= n; i++){
            cout << i << ":";
            int len = tree[i].size();
            for(int j = 0; j < len; j++){
                cout << " " << tree[i][j];
            }
            cout << endl;
        }
    }
    return 0;
}
```