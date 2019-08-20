---
title: Uva806-Spatial Structures-四分树
date: 2018-12-04 20:48:34
categories:
- ACM
- 树
tags:
- 四分树
---
有幸做World Final的题，，做了一下午= =，刷新我的单题代码长度了，怕了怕了。
黑白图像有两种表示方法，点阵表示和路径表示。路径表示法首先需要把图像转化为四分树，然后记录所有黑结点到根的路径。
任务是在这两种表示法之间进行转换。
<!--more-->

# 链接
[Uva806-Spatial Structures](https://vjudge.net/problem/UVA-806)

# 题目描述
中间图为点阵表示：
![1](/1.png)
四分树：
![2](/2.png)
四分树中NW，NE，SW，SE分别用1、2、3、4表示。其实它们的代表是把图像四分后的左上、右上、左下和右下部分。把得到的路径数字串看成是五进制的，转化为十进制后排序。例如上面的树在转化，排序后的结果是：9 14 17 22 23 44 63 69 88 94 113。
要求给定一串数字路径还原点阵形式，或给定点阵写出所有黑结点的数字路径并排序输出。

# 题解
题目本身难度不是很大，只不过有很多细节需要注意。自己就被Presentation Error了好久= =。点阵转路径的时候，递归进行四分，如果子区域全白或全黑，跳出递归，边递归边统计路径，全黑时就把路径转成十进制存起来。路径转点阵的过程类似，可以用队列维护数字串，每取出一个就重新规划区域，直到队列为空，把当前区域全部涂黑。
*除去输出格式的问题，还有一个需要注意的是，若初始图像全白，则路径用空串来表示，需要特殊处理，同时意味着读入路径数据的时候要用getline。*

# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue  4 Dec 14:36:07 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 70;
int n;
char d[maxn][maxn];
int x, y;
stack<int> rec;
vector<int> ans;
int cnt = 0;

bool All_Black(int r, int c, int w){
    for(int i = r; i < r + w; i++){
        for(int j = c; j < c + w; j++){
            if(d[i][j] == '0') return false;
        }
    }
    return true;
}

bool All_White(int r, int c, int w){
    for(int i = r; i < r + w; i++){
        for(int j = c; j < c + w; j++){
            if(d[i][j] == '1') return false;
        }
    }
    return true;
}

void cal(){
    int sum = 0;
    stack<int> tmp = rec;
    while(!tmp.empty()){
        sum *= 5;
        sum += tmp.top();
        tmp.pop();
    }
    ans.push_back(sum);
}

void dfs(int r, int c, int w, stack<int>& rec){
    //cout << r << " " << c << " " << w << endl;
    if(All_Black(r, c, w)){
        //cout << "all black" << endl;
        cal();
        return;
    }
    if(All_White(r, c, w)){
        //cout << "all white" << endl;
        return;
    }
    rec.push(1);
    dfs(r, c, w / 2, rec);
    rec.pop();
    rec.push(2);
    dfs(r, c + w / 2, w / 2, rec);
    rec.pop();
    rec.push(3);
    dfs(r + w / 2, c, w / 2, rec);
    rec.pop();
    rec.push(4);
    dfs(r + w / 2, c + w / 2, w / 2, rec);
    rec.pop();
} 

void solve1(){
    for(int i = 0; i < n; i++){
        scanf("%s", d[i]);
    }
    while(!rec.empty()) rec.pop();
    dfs(0, 0, n, rec);
    sort(ans.begin(), ans.end());

    printf("Image %d\n", cnt);
    int len = ans.size();
    for(int i = 0; i < len; i++){
        if((i + 1) %12 == 1) printf("%d", ans[i]);
        else printf(" %d", ans[i]);

        if((i + 1) % 12 == 0) printf("\n");
    }
    if((len % 12) != 0) printf("\n");
    printf("Total number of black nodes = %d\n", len);
}

void draw(int r, int c, int w, queue<int>& que){
    if(que.empty()){
        for(int i = r; i < r + w; i++){
            for(int j = c; j < c + w; j++){
                d[i][j] = '*';
            }
        }
        return;
    }
    int tmp = que.front();
    que.pop();
    if(tmp == 1) draw(r, c, w / 2, que);
    if(tmp == 2) draw(r, c + w / 2, w / 2, que);
    if(tmp == 3) draw(r + w / 2, c, w / 2, que);
    if(tmp == 4) draw(r + w / 2, c + w / 2, w / 2, que); 
}

void Print_White(){
    printf("Image %d\n", cnt);
    for(int i = 0; i < -n; i++){
        for(int j = 0; j < -n; j++){
            printf(".");
        }
        printf("\n");
    }
}

void solve2(){
    getchar();
    int num;
    queue<int> que;
    stringstream ss1, ss2;
    string s1, s2;
    //fgets(s, 1000, stdin);
    getline(cin, s1);
    if(s1[0] == '\n') {
        Print_White();
        return;
    }

    ss1.str(s1);
    while(ss1 >> s2){
        ss2.str(s2);
        ss2 >> num;
        ss2.clear();
        if(num == -1) break;

        while(!que.empty()) que.pop();

        while(num){
            que.push(num % 5);
            num /= 5;
        }
        draw(0, 0, -n, que);
    }

    printf("Image %d\n", cnt);
    for(int i = 0; i < -n; i++){
        for(int j = 0; j < -n; j++){
            if(d[i][j] != '*'){
                d[i][j] = '.';
            }
            printf("%c", d[i][j]);
        }
        printf("\n");
    }
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif

    while(1){
        scanf("%d", &n);
        if(n == 0) break;

        memset(d, 0, sizeof(d));
        ans.clear();

        if(cnt > 0) printf("\n");
        cnt++;
        if(n > 0){
            solve1();
        }
        if(n < 0){
            solve2();
        }
    }
    return 0;
}
```

