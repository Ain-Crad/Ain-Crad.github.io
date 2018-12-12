---
title: Uva127-Accordian Patience-链表
date: 2018-12-12 10:25:11
categories:
- ACM
tags:
- 链表
- 栈
---
一副扑克牌（52张），按照从左往右的顺序，若当前扑克牌和它的左一或左三扑克牌的数字或花色相同，就把它移动到那张牌的上方，移动后如果继续满足条件则继续移动。左一和左三同时满足时，左三优先。当有某个位置的牌为空时，要及时把这个位置消除。
实战了一把数组模拟链表，还发现了一个大佬的博客，嚯嚯。收获了感动，坚定了继续坚持下去的信念。
<!--more-->
## 链接
题目链接[Uva127-"Accordian Patience"](https://vjudge.net/problem/UVA-127)
参考链接[shuangde800的博客](https://blog.csdn.net/shuangde800/article/details/7703781#commentBox)

## 题解
52个位置的牌堆分别用一个栈来维护。因为有消除牌堆的过程，所以会有频繁的移动，用链表来维护牌堆之间的关系效率比较高。代码里用两个数组next_idx和front_idx模拟了双向链表。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 11 Dec 20:19:53 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

stack<string> d[60];
int next_idx[60];
int front_idx[60];

bool input(){
    memset(next_idx, -1, sizeof(next_idx));
    memset(front_idx, -1, sizeof(front_idx));
    string s;
    stack<string> stk;
    for(int i = 0; i < 52; i++){
        cin >> s;
        if(s == "#") return false;
        stk.push(s);
        d[i] = stk;
        stk.pop();
    }
    return true;
}

//judge if we can move 3 steps
bool ok_3(int i){
    int last3;
    last3 = front_idx[front_idx[front_idx[i]]];
    //if(last3 < 0) return false;
    if(d[last3].top()[0] == d[i].top()[0] || d[last3].top()[1] == d[i].top()[1]){
        d[last3].push(d[i].top());
        d[i].pop();
        return true;
    }
    else return false;
}

//judge if we can move 1 step
bool ok_1(int i){
    int last1;
    last1 = front_idx[i];
    //if(last1 < 0) return false;
    if(d[last1].top()[0] == d[i].top()[0] || d[last1].top()[1] == d[i].top()[1]){
        d[last1].push(d[i].top());
        d[i].pop();
        return true;
    }
    else return false;
}

bool move(){
    int cnt = 0;
    for(int i = 0; i != -1; i = next_idx[i], cnt++){
        if(cnt > 2)
            if(ok_3(i)) return true;
        if(cnt > 0)
            if(ok_1(i)) return true;
    }
    return false;
}

//erase empty position
void erase(){
    for(int i = 0; i != -1; i = next_idx[i]){
        if(d[i].empty()){
            int l = front_idx[i];
            int r = next_idx[i];
            next_idx[l] = r;
            front_idx[r] = l;
            next_idx[i] = front_idx[i] = -1;
        }
    }
}

int main(){
    #ifndef ONLINE_JUDGE    
        freopen("in.txt", "r", stdin);
    #endif

    while(input()){
        for(int i = 0; i < 51; i++){
            next_idx[i] = i + 1;
        }
        for(int i = 1; i < 53; i++){
            front_idx[i] = i - 1;
        }

        while(move()){
            erase();
        }

        vector<int> ans;
        for(int i = 0; i != -1; i = next_idx[i]){
            ans.push_back(d[i].size());
        }
        int len = ans.size();
        if(len == 1){
            cout << "1 pile remaining: " << ans[0] << endl;
        }
        else{
            cout << len << " piles remaining:";
            for(int x : ans){
                cout << " " << x;
            }
            cout << endl;
        }
    }
    return 0;
}

```