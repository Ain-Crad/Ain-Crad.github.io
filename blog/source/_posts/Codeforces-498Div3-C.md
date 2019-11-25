---
title: 'Codeforces-#498Div3-C-前&&后缀和'
date: 2018-07-31 23:08:57
categories:
- ACM
- 前缀和
tags:
- 前缀和
- 后缀和
---
好题，虽然比较简单，但是觉得这道题的解题思路挺棒的，用到了前缀和，后缀和以及思路的转换．解出来之后还是很开心的，嚯嚯嚯（一个只能做做Div3的小菜鸡|･ω･｀)）
<!-- more -->
# 链接
[Codeforces-#498Div3-C](http://codeforces.com/contest/1006/problem/C)
# 题解
题目大意：给定n个整数（范围1-1e9）,要求把这n个整数划分为连续的三段，使得第一段的数的总和与第三段的数的总和相等且要求这个总和尽量大．
思路：
因为n的范围是1-2e5，而且又涉及求和，所以应该可以比较自然的想到前缀和，题目又要求是前后两段的总和相等所以后缀和也要统计一下．接下来就要考虑什么时候两端的和相等且最大了，因为第一段和第三段长度不定，所以相当于是两个变量，想到它们的长度变化其实可以归一到第二段数据的长度变化，这样的话复杂度变为O(n)，就可以通过了．具体做法是对于[1, n]这n个数据，初始化第二段数据区间为[2, n - 1], 然后判断此时第一段和第三段数据和的大小关系，因为已经统计了前缀和和后缀和，所以这部分复杂度是O(1)．如果第一段数据和等于第三段数据和则记录这个数据和同时缩小第二段数据的区间，变为[3, n - 2]，继续判断，看是否还有更大的数据和满足条件；如果第一段数据和大于第三段数据和则保持第二段区间左端点不变收缩右端点，即变为[2, n - 2];同样的如果第一段数据和小于第三段数据和则保持第二段区间右端点不变，左端点收缩，即变为[3, n - 2];以此类推，直到第二段区间消失，此时最后一次记录的数据和即为结果．
# 代码
``` C++
#include <bits/stdc++.h>

using namespace std;

const int maxn = 2e5 + 7;
int d[maxn];
int n;
long long psum[maxn], bsum[maxn];
vector<long long> vec;

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        //freopen("in.txt", "r", stdin);
    #endif
    
    scanf("%d", &n);
    for(int i = 0; i < n; i++){
        scanf("%d", d + i);
        if(i == 0) psum[i] = d[i];
        else psum[i] = psum[i - 1] + d[i];
    }
    for(int i = n - 1; i >= 0; i--){
        if(i == n - 1) bsum[i] = d[i];
        else bsum[i] = bsum[i + 1] + d[i];
    }

    int i, j;
    for(i = 0, j = n - 1; i < n;){
        if(i >= j) break;
        if(psum[i] == bsum[j]){
            vec.push_back(psum[i]);
            i++;
            j--;
        }
        else if(psum[i] < bsum[j]) i++;
        else if(psum[i] > bsum[j]) j--;
    }
    if(vec.size() != 0) printf("%lld\n", vec[vec.size() - 1]);
    else printf("0\n");
    return 0;
}
```