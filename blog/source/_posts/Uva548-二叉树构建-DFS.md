---
title: Uva548-二叉树构建&&DFS
date: 2018-09-22 11:30:57
categories:
- ACM
tags:
- 二叉树
- DFS
---
首先根据二叉树的中序和后序遍历构建二叉树，然后找一个叶子节点使得它到根的路径上的权和最小。
<!--more-->
## 链接
[Uva548-Tree](https://vjudge.net/problem/UVA-548)
## 题目描述
给一棵点带权（权值各不相同，都是小于10000的正整数）的二叉树的中序和后序遍历，找一个叶子节点使得它到根的路径上的权和最小。如果有多解，该叶子本身的权应尽量小。输入中每两行表示一棵树，其中第一行为中序遍历，第二行为后序遍历。
## 题解
根据中序和后序遍历可以构建出这棵二叉，然后用DFS搜索找到结果。
## 代码
**通过设置$best\_sum$和$best$找到最优解的方法值得学习啊。**
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sat 22 Sep 11:20:12 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxn = 1e4 + 7;
int in_order[maxn], post_order[maxn], lch[maxn], rch[maxn];
string line;
int n;
int best, best_sum;

bool read(int* s){
    if(!getline(cin, line)) return false;
    stringstream ss(line);
    n = 0;
    int x;
    while(ss >> x){
        s[n++] = x;
    }
    return true;
}

int build(int l1, int r1, int l2, int r2){
    if(l1 > r1) return 0;
    int v = post_order[r2];
    int pos = 0;
    while(in_order[pos] != v) pos++;
    int len = pos - l1;
    lch[v] = build(l1, l1 + len - 1, l2, l2 + len - 1);
    rch[v] = build(l1 + len + 1, r1, l2 + len, r2 - 1);
    return v;
}

void dfs(int v, int sum){
    sum += v;
    if(!lch[v] && !rch[v]){
        if(sum < best_sum || (sum == best_sum && v < best)){
            best_sum = sum;
            best = v;
        }
        return;
    }
    if(lch[v]) dfs(lch[v], sum);
    if(rch[v]) dfs(rch[v], sum);
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    
    while(read(in_order)){
        read(post_order);
        int root = build(0, n - 1, 0, n - 1);
        best_sum = 1e9;
        dfs(root, 0);
        cout << best << endl;
    }
    
    return 0;
}
```