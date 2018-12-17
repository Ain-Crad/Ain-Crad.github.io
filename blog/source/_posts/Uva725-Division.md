---
title: Uva725-Division-暴力枚举
date: 2018-12-17 19:41:14
categories:
- ACM
- 暴力枚举
tags:
- 枚举
---
求满足$$\frac{abcde}{fghij} = N$$的所有$abcde$ 和 $fghij$的值，其中$2 <= N <= 79。$
刘汝佳老师：``即使采用暴力法求解问题，对问题进行一定的分析往往会让算法更简介、高效。``
受益匪浅。

## 链接
[Uva725-Division](https://vjudge.net/problem/UVA-725)

## 题解
一开始直接DFS暴力出来0~9的不同排列，判断是否满足条件，TL了。看了刘汝佳老师的解法，只枚举$fghij$，然后算出$abcde$，再判断是否所有数字都不相同。而且当$abcde$ 和 $fghij$加起来超过10位时可以终止枚举。复杂度大大降低。

##代码-循环枚举
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Mon 17 Dec 07:05:55 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

int n;
char buff[100];
bool flag;
struct Node{
    Node(int a, int b):a(a), b(b){}
    int a, b;
};
vector<Node> vec;
int cnt = 0;

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(cin >> n){
        if(n == 0) break;
        if(cnt++) cout << endl;
        vec.clear();
        for(int a = 1234; ; a++){
            int b = n * a;
            //cout << a << " " << b << endl;
            sprintf(buff, "%05d%05d", a, b);
            if(strlen(buff) > 10) break;
            sort(buff, buff + 10);
            flag = false;
            for(int i = 0; i < 10; i++){
                if(buff[i] != '0' + i){
                    flag = true;
                    break;
                }
            }
            if(!flag) vec.push_back(Node(a, b));
        }

        int len = vec.size();
        if(len == 0) cout << "There are no solutions for " << n << "." << endl;
        else{
            for(auto x : vec){
                cout << setfill('0') << setw(5) << x.b;
                cout << " / ";
                cout << setfill('0') << setw(5) << x.a;
                cout << " = ";
                cout << n << endl;
            }
        }
    }
    return 0;
}
```

##代码-DFS枚举
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun 16 Dec 17:39:29 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

int n;
bool vis[10];
bool test[10];
vector<int> vec;
int a, b;
struct Node{
    Node(int u, int v):u(u), v(v){}
    int u, v;
};
vector<Node> ans;
bool flag;

bool ok(){
    a = 0;
    for(int i = 0; i < 5; i++){
        a *= 10;
        a += vec[i];
    }
    b = a * n;
    vector<int> tmp;
    int b2 = b;
    for(int i = 0; i < 5; i++){
        tmp.push_back(vec[i]);
        tmp.push_back(b2 % 10);
        b2 /= 10;
    }
    if(b2){
        flag = true;
        return false;
    }

    memset(test, 0, sizeof(test));
    int cnt = 0;
    for(int i = 0; i < 10; i++){
        int val = tmp[i];
        if(!test[val]){
            cnt++;
            test[val] = 1;
        }
    }
    if(cnt == 10) return true;
    else return false;

}

void dfs(){
    if(flag) return; // over 10 bits, no need to enumerate
    if(vec.size() == 5){
        if(ok()){
            ans.push_back(Node(b, a));
        }
        return;
    }
    for(int i = 0; i <= 9; i++){
        if(!vis[i]){
            vec.push_back(i);
            vis[i] = 1;
            dfs();
            vec.pop_back();
            vis[i] = 0;
        }
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    int cnt = 0;
    while(cin >> n){
        if(n == 0) break;

        cnt++;
        if(cnt > 1) cout << endl;
        
        memset(vis, 0, sizeof(vis));
        ans.clear();
        vec.clear();
        flag = false;

        dfs();
        if(ans.size() == 0){
            cout << "There are no solutions for " << n << "." << endl;
        }
        else{
            for(auto x : ans){
                cout << right << setfill('0') << setw(5) << x.u << " / ";
                cout << right << setfill('0') << setw(5) << x.v << " = " << n << endl;
            }
        }
    }
    return 0;
}
```