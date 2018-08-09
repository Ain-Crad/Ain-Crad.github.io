---
title: Uva12096-The SetStack Computer-STL
date: 2018-08-09 22:44:02
categories:
- ACM
tags:
- stack
- STL
---
最近在看刘汝佳老师的《算法竞赛入门经典（第2版）》，收获很大，很多知识都在上面查漏补缺，书上有一道STL的例题，用到了stack、vector、map、set，而且“化集合为ID”的思路很好，记录一下。
## 链接
[Uva12096-The SetStack Computer](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3248)
## 题目描述
有一个专门为了集合运算而设计的“集合栈”计算机。该机器有一个初始为空的栈，并且支持一下操作。
- PUSH:空集“{}”入栈。
- DUP:把当前栈顶元素复制一份后再入栈。
- UNION:出栈两个集合，然后把二者的并集入栈。
- INTERSECT:出栈两个集合，然后把出栈的集合加入到后出栈的集合中，把结果入栈。
每次操作后，输出栈顶集合的大小（即元素个数）。
## 题解
本题的集合并不是简单的整数集合或者字符串集合，而是集合的集合。为了方便为每个不同的集合分配一个唯一的ID，则每个集合都可以表示成所包含元素的ID的集合，这样就可以用STL的set<int> 来表示了，而整个栈则是一个stack<int>。map用来把集合映射成ID，vector则根据ID取集合。
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Thu  9 Aug 16:19:36 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

int t;
int n;
typedef set<int> Set;
map<Set, int> mp;
vector<Set> vec;

int ID(Set st){
    if(mp.count(st)) return mp[st];
    vec.push_back(st);
    return mp[st] = vec.size() - 1;
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        //freopen("in.txt", "r", stdin);
    #endif
    
    cin >> t;
    while(t--){
        cin >> n;
        stack<int> stc;
        string s;
        vec.clear();
        mp.clear();
        for(int i = 0; i < n; i++){
            cin >> s;
            if(s[0] == 'P') stc.push(ID(Set ()));
            else if(s[0] == 'D') stc.push(stc.top());
            else{
                Set st1 = vec[stc.top()]; stc.pop();
                Set st2 = vec[stc.top()]; stc.pop();
                Set st;
                if(s[0] == 'U') set_union(st1.begin(), st1.end(), st2.begin(), st2.end(), inserter(st, st.begin()));
                if(s[0] == 'I') set_intersection(st1.begin(), st1.end(), st2.begin(), st2.end(), inserter(st, st.begin()));
                if(s[0] == 'A') {st = st2; st.insert(ID(st1));}
                stc.push(ID(st));
            }
            cout << vec[stc.top()].size() << endl;
        }
        cout << "***" << endl;
    }
    
    return 0;
}
```
## 补充
### 并集
对两个集合取交集可以使用std::set_union，该函数需要两对迭代器，以及一个输出迭代器。
```C++
set<int> st1;
set<int> st2;
set<int> st;
set_union(st1.begin(), st1.end(), st2.begin(), st2.end(), inserter(st, st.begin()));
```
### 交集
对两个集合取交集可以使用std::set_intersection，该函数需要两对迭代器，以及一个输出迭代器。
```C++
set<int> st1;
set<int> st2;
set<int> st;
set_intersection(st1.begin(), st1.end(), st2.begin(), st2.end(), inserter(st, st.begin()));
```
类似的还有：
- set_difference:差集，从第一个区间去除第二个区间的元素。
- set_merge:合并，与set_union不同，set_merge不会去除重复的元素（不能用于set，可用于vector）。
注意：set_union和set_intersection等也可用于vector，但必须是排好序的vector。
