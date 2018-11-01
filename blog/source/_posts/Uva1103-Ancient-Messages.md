---
title: Uva1103-Ancient Messages-DFS
date: 2018-10-28 21:41:19
categories:
- ACM
tags:
- DFS
- 全局思维
---
一道让人惊呼“DFS还能这么玩”的题目。给定一个由象形文字组成的图像，要求按字典序输出图中所有象形文字（输出时每个象形文字由一个给定的英文表示）。关键是通过什么方法区分这些象形文字。
![hier](/hier.png)
<!--more-->
## 链接
[Uva1103-Ancient Messages](https://vjudge.net/problem/UVA-1103)
## 题目描述
图像的像素由 $0$ 和 $1$ 组成，其中 $0$ 代表白色像素点，$1$ 代表黑色像素点。整个图像以十六进制形式给出。图像包含六种象形符号中的至少一个，不同的符号不会相互接触。符号的形状和上图给出的拓扑等价（可以随意拉伸但不能拉断）。
<div style="width: 300px; margin: auto">
    ![1.png](/1.png)
    ![2.png](/2.png)
</div>
## 题解
需要找出能够区别出这些不同象形符号的特征量。可以发现每个符号都包含一些白色的色块区域，6个符号从左到右分别包含 $1$，$3$，$5$，$4$，$0$，$2$ 个白色区域，各不相同。所以可以根据这一特征量区分不同的象形符号。
具体做法为用dfs方法对每一块黑色或白色区域染色，这样可以把图像分为几个不同的颜色区域，再从每个黑色像素区域出发去统计在它内部的白色像素区域的个数。

*需要注意的是初始时要在图像四周多添加一圈白色像素，以保证color 1 每次都表示的是背景白色像素区域*（因为这个地方$WA$了好多次，$QAQ$）

另外这题没有测试数据也很坑2333，最后友情赠送几组测试数据。
## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Sun 28 Oct 22:24:10 CST 2018
 *
 */
 
#include <bits/stdc++.h>

using namespace std;

const int maxh = 207;
const int maxw = 50 * 4 + 7;
int h, w;
char pix[maxh][maxw];
int dig[maxh][maxw];
int color[maxh][maxw];
int dcode[256];
int dx[] = {0, 1, 0, -1};
int dy[] = {1, 0, -1, 0};
vector<int> rec;
set<int> st[maxw];
const string s = "WAKJSD";

void decode(char c, int h, int pos){
    int x = dcode[(int)c];
    for(int i = 0; i < 4; i++){
        if(x & (1 << (3 - i))) dig[h][pos + i] = 1;
        else dig[h][pos + i] = 0;
    }
}

void dye(int x, int y, int c){
    color[x][y] = c;
    for(int i = 0; i < 4; i++){
        int nx = x + dx[i], ny = y + dy[i];
        if(nx >= 0 && nx < h && ny >= 0 && ny < w && color[nx][ny] == 0 && dig[nx][ny] == dig[x][y]){
            dye(nx, ny, c);
        }
    }
}

void cal(int x, int y, int c){
    //cout << x << " " << y << endl;
    for(int i = 0; i < 4; i++){
        int nx = x + dx[i], ny = y + dy[i];
        if(nx >= 0 && nx < h && ny >= 0 && ny < w && color[nx][ny] != 1 && dig[nx][ny] == 0){
            //cout << "color:" << color[nx][ny] << endl;
            st[c].insert(color[nx][ny]);
        }
    }
}

void print(){
    for(int i = 0; i < h; i++){
        for(int j = 0; j < w; j++){
            cout << dig[i][j] << " ";
        }
    }
    cout << endl;
}

void printcolor(){
    cout << endl;
    for(int j = 0; j < h; j++){
        for(int i = 0; i < w; i++){
            cout << color[j][i] << " ";
        }
        cout << endl;
    }
}

void init(){
    dcode['0'] = 0x00;
    dcode['1'] = 0x01;
    dcode['2'] = 0x02;
    dcode['3'] = 0x03;
    dcode['4'] = 0x04;
    dcode['5'] = 0x05;
    dcode['6'] = 0x06;
    dcode['7'] = 0x07;
    dcode['8'] = 0x08;
    dcode['9'] = 0x09;
    dcode['a'] = 0x0a;
    dcode['b'] = 0x0b;
    dcode['c'] = 0x0c;
    dcode['d'] = 0x0d;
    dcode['e'] = 0x0e;
    dcode['f'] = 0x0f;
}

int main(){
    //ios::sync_with_stdio(false);
    //cin.tie(0);
    //cout.tie(0);
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    init();
    int cs = 0;
    while(cin >> h >> w){
        //cout << h << " " << w << endl;
        if(h == 0 && w == 0) break;
        cout << "Case " << (++cs) << ": ";
        memset(color, 0, sizeof(color));
        memset(dig, 0, sizeof(dig));
        for(int i = 0; i < maxw; i++){
            st[i].clear();
        }
        rec.clear();
        for(int i = 0; i < h; i++){
            cin >> pix[i];
            //cout << pix[i] << endl;
            for(int j = 0; j < w; j++){
                decode(pix[i][j], i + 1, j * 4 + 1);
            }
        }
        //给图像四周添加一圈白色像素
        h += 2;
        w = w * 4 + 2;
        
        //print();
        
        int cnt = 0;
        for(int i = 0; i < h; i++){
            for(int j = 0; j < w; j++){
                if(color[i][j] == 0){
                    dye(i, j, ++cnt);
                    if(dig[i][j] == 1)
                        rec.push_back(cnt);
                }
            }
        }
        //printcolor();
        for(auto x : rec){
            //cout << x << endl;
            for(int i = 0; i < h; i++){
                for(int j = 0; j < w; j++){
                    if(color[i][j] == x){
                        cal(i, j, x);
                    }
                }
            }
        }
        vector<int> ans;
        for(auto x : rec){
            ans.push_back(st[x].size());
        }
        char res[10000];
        int ct = 0;
        for(size_t i = 0; i < ans.size(); i++){
            res[ct++] = s[ans[i]];
        }
        sort(res, res + ct);
        for(int i = 0; i < ct; i++){
            cout << res[i];
        }
        cout << endl;
    }
    return 0;
}
```

## 测试数据及答案
```
//数据
4 4
0f00
330e
1e06
0c02
10 5
00f00
0198c
00f0e
01f8e
00604
001e0
00330
003f0
00210
003f0
5 5
0e00e
1b3ea
0e32e
043e4
000c0
5 3
fff
f0f
fff
f0f
fff
5 3
fff
70e
7fe
70e
7fe
0 0

//答案
Case 1: AW
Case 2: AKW
Case 3: AAA
Case 4: K
Case 5: K
```