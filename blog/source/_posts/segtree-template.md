---
title: 线段树单点更新模板
date: 2018-05-06 21:47:10
categories:
- ACM
tags:
- 线段树
---
之前一直在用大白上的线段树单点更新模板，最近渐渐发现有一些不太舒服的地方，一方面是大白上的模板会强行把线段树建成一个满二叉树，这样虽然比较好理解一些，但同时也会因为要考虑多出来的那些点的影响而相对麻烦。另一方面是因为大家貌似都不用大白上的线段树模板= =||，这样搜题解以及和队友交流的时候会存在一些障碍（其实这一方面是主要原因），所以最后通过权衡还是选择了一个自己觉得通用性比较好也比较好实现的模板。[参考博客链接](http://www.hysonn.com/2017/01/26/segtree/)
ps:关于线段树区间更新目前正在学习中，之后再补上关于区间更新的模板。

## 基于最大值查询的线段树模板
```C++
//首先定义线段树的结构体
struct SegTree{
    int vl, vr;         //每个tree[k]对应一个节点，其中vl, vr是节点对应的区间
    int mv;             //表示节点维护的数据，视情况而定，以维护区间最大值为例
}tree[maxn << 2];       //绝对安全的情况是建议开四倍数据量的大小

void build(int k, int l, int r){
    tree[k].vl = l;
    tree[k].vr = r;
    if(l == r){
        tree[k].mv = d[l];
        return;
    }
    int mid = (l + r) / 2;
    build(k << 1, l, mid);
    build(k << 1 | 1, mid + 1, r);
    tree[k].mv = max(tree[k << 1].mv, tree[k << 1 | 1].mv);
}

void update(int k, int pos, int val){
    if(tree[k].vl == tree[k].vr){
        tree[k].mv = val;
        return;
    }
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(pos <= mid)
        update(k << 1, pos, val);
    else
        update(k << 1 | 1, pos, val);
    tree[k].mv = max(tree[k << 1].mv, tree[k << 1 | 1].mv); 
}

int query(int k, int l, int r){
    if(l <= tree[k].vl && r >= tree[k].vr)
        return tree[k].mv;
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        return query(k << 1, l, r);
    if(l > mid)
        return query(k << 1 | 1, l, r);
    return max(query(k << 1, l, mid), query(k << 1 | 1, mid + 1, r));
	
}

```
## 例题实现
[HDU-1754--I Hate It(线段树单点更新入门题)](http://acm.hdu.edu.cn/showproblem.php?pid=1754)
AC代码
```C++
#include <bits/stdc++.h>

using namespace std;

const int maxn = 2e5 + 7;
int n, m;
struct SegTree{
    int vl, vr;
    int mv;
}tree[maxn << 2];

int d[maxn];

void build(int k, int l, int r){
    tree[k].vl = l;
    tree[k].vr = r;
    if(l == r){
        tree[k].mv = d[l];
        return;
    }
    int mid = (l + r) / 2;
    build(k << 1, l, mid);
    build(k << 1 | 1, mid + 1, r);
    tree[k].mv = max(tree[k << 1].mv, tree[k << 1 | 1].mv);
}

void update(int k, int pos, int val){
    if(tree[k].vl == tree[k].vr){
        tree[k].mv = val;
        return;
    }
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(pos <= mid)//doubt
        update(k << 1, pos, val);
    else
        update(k << 1 | 1, pos, val);
    tree[k].mv = max(tree[k << 1].mv, tree[k << 1 | 1].mv); 
}

int query(int k, int l, int r){
    if(l <= tree[k].vl && r >= tree[k].vr)
        return tree[k].mv;
    int mid = (tree[k].vl + tree[k].vr) / 2;
    if(r <= mid)
        return query(k << 1, l, r);
    if(l > mid)
        return query(k << 1 | 1, l, r);
    return max(query(k << 1, l, mid), query(k << 1 | 1, mid + 1, r));
	
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
	
    while(~scanf("%d%d", &n, &m)){
        char c[10];
        int a, b;
        for(int i = 1; i <= n; i++){
            scanf("%d", &d[i]);
        }
        build(1, 1, n);
        for(int i = 1; i <= m; i++){
            scanf("%s%d%d", c, &a, &b);
            if(c[0] == 'Q')
                printf("%d\n", query(1, a, b));
            else if(c[0] == 'U')
            update(1, a, b);
        }
		
	}
	
    return 0;
}
```

