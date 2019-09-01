---
title: Binary Search以及lower_bound & upper_bound实现
date: 2019-07-27 15:22:51
categories:
- Template
tags:
- Binary Search
---
![BinarySearch](/BinarySearch.jpeg)
<!--more-->

# Binary Search
　　在数组 $A$ 的 $[x, y)$ 区间中查找值 $v$，并返回其位置下标，若不存在则返回 $-1$。

```C++
int bsearch(int *A, int x, int y, int v){
    int m;
    while(x < y){
        m = x + (y - x) / 2;
        if(A[m] == v) return m;
        else if(A[m] > v) y = m;
        else x = m + 1;
    }

    return -1;
}
```

# lower_bound
　　当 $v$ 存在时返回它出现的第一个位置；如果不存在，返回这样一个下标 $i$：在此处插入 $v$ 后序列仍然有序。

```C++
int lower_bound(int *A, int x, int y, int v){
    int m;
    while(x < y){
        m = x + (y - x) / 2;
        if(A[m] >= v) y = m;
        else x = m + 1;
    }

    return x;
}
```
　　最后的返回值不仅可能是 $x, x + 1, x + 2, ..., y - 1$ 中的一个，还可能是 $y$，如果 $v$ 大于 $A[y-1]$ 就只能插入这里了。这样，这样尽管查找区间是左闭右开区间 $[x, y)$，返回值的候选区间却是闭区间 $[x, y]$。

- A[m] = v：至少已经找到一个v，而左边可能还有，因此区间变为[x, m]。
- A[m] > v：所求位置不可能在后面，但有可能是m，因此区间变为[x, m]。
- A[m] < v：m和前面都不可行，因此区间变为[m + 1, y]。

# upper_bound
　　当 $v$ 存在时返回它出现的最后一个位置的后面一个位置；如果不存在，返回这样一个下标 $i$：在此处插入 $v$ 后序列仍然有序。

```C++
int upper_bound(int *A, int x, int y, int v){
    int m;
    while(x < y){
        m = x + (y - x) / 2;
        if(A[m] > v) y = m;
        else x = m + 1; 
    }

    return x;
}
```

# 参考

《算法竞赛入门经典（第2版）》