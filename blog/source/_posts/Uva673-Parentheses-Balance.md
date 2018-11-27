---
title: Uva673-Parentheses Balance-栈
date: 2018-11-27 07:08:53
categories:
- ACM
tags:
- Stack
---
输入一个包含 $“()”$ 和 $“[]”$ 的括号序列，判断是否合法。具体规则：
- 空串合法
- 如果A和B都合法，则AB合法
- 如果A合法则(A)和[A]都合法

可能最近DFS，BFS做多了，总想往搜索上套...不过最后能想到栈还是很鸡冻＼（≧▽≦）／
<!--more-->
## 链接
[Uva673-Parentheses Balance](https://vjudge.net/problem/UVA-673)

## 题解
能想到用栈之后这道题基本就可解了，将字符序列依次放入栈中遇到可以匹配的括号对　$“()”$ 或 $“[]”$　就出栈。如果最后栈中还有未匹配的括号，则不合法；否则若栈空，则合法。
*可能含有空串所以读入的时候用getline(cin, s)读入，因为cin会自动忽略'\n'，即忽略空串。*

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Mon 26 Nov 23:30:58 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 130;
string s;
int n;
stack<char> stk;
bool p;

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    cin >> n;
    getchar();
    while(n--){
        while(!stk.empty()){
            stk.pop();
        }

        getline(cin , s);
        if(s.length() % 2){
            cout << "No" << endl;
            continue;
        }
        for(size_t i = 0; i < s.length(); i++){
            p = false;
            if(!stk.empty()){
                if((stk.top() == '(' && s[i] == ')') || (stk.top() == '[' && s[i] == ']')){
                    stk.pop();
                    p = true;
                }
            }
            if(!p) stk.push(s[i]);
        }

        if(!stk.empty()) cout << "No" << endl;
        else cout << "Yes" << endl;
    }
    return 0;
}
```

