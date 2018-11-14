---
title: Uva221-Urban Elevations-离散化
date: 2018-11-14 10:00:10
categories:
- ACM
tags:
- 离散化
---
输入每个建筑物左下角坐标、宽度、深度和高度，输出正视图中能看到的所有建筑物，按照左下角x坐标从小到大进行排序。坐下角x坐标相同时，按y坐标从小到大排序。
![building](/building.png)
*第一眼看上去很像当时面试北航计算机时候的机试题，后来做着做着发现不对劲= =。结尾放一个彩蛋，把印象中的机试题和解题思路说一下。*

## 链接
[Uva221-Urban Elevation](https://vjudge.net/problem/UVA-221)

## 题解
因为是正视图，所以判断可见性的时候忽略深度参数，只把它当做判断前后层次的一个信息。判断一个建筑物是否可见，只要满足建筑物在x方向上有一些点（大于一个）可见，那么建筑物可见。因为x值连续有无穷个，所以无法枚举x。需要进行离散化，把无穷变为有限。
把所有x坐标排序去重，则任意两个相邻x坐标形成的区间要么完全可见，要么完全不可见。等效于按照所有建筑物的垂直轮廓线进行区间划分，然后只要对这几个区间进行判断，任取区间一点，若这点对建筑物可见，则这段区间对建筑物可见，则可以说这个建筑物可见。判断一个建筑物是否在某个坐标点可见的条件是：建筑物坐标中必须包含这个坐标；建筑物前方不能有包含这个坐标且高于它的其它建筑物。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Wed 14 Nov 07:22:41 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 107;
struct Building{
    int id;
    double x, y, w, d, h;
};
Building bld[maxn];
int xp[maxn * 2];
int n;

bool cmp(const Building& a, const Building& b){
    if(a.x == b.x){
        return a.y < b.y;
    }
    else{
        return a.x < b.x;
    }
}

bool visible(int ct, int x){
    ///cout << "target: " << bld[ct].id << endl;
    for(int i = 0; i < n; i++){
        if(i == ct) continue;
        if(bld[i].y <= bld[ct].y && bld[i].x <= x && bld[i].x + bld[i].w >= x){
            //cout << bld[i].id << endl;
            if(bld[i].h >= bld[ct].h) return false;
        }
    }
    //cout << "******" << endl;
    return true;
}

int main(){
    #ifndef ONLINE_JUDGE
        freopen("in.txt", "r", stdin);
    #endif
    int cnt = 1;
    while(cin >> n){
        if(n == 0) break;
        memset(bld, 0, sizeof(bld));
        double x, y, w, d, h;
        for(int i = 0; i < n; i++){
            bld[i].id = i + 1;
            cin >> x >> y >> w >> d >> h;
            bld[i].x = x, bld[i].y = y, bld[i].w = w, bld[i].d = d, bld[i].h = h;
            xp[2 * i] = x, xp[2 * i + 1] = x + w;
        }
        sort(bld, bld + n, cmp);
        sort(xp, xp + 2 * n);
        int m = unique(xp, xp + 2 * n) - xp;
        vector<int> vec;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m - 1; j++){
                if(bld[i].x >= xp[j + 1] || bld[i].x + bld[i].w <= xp[j]) continue;
                if(visible(i, (xp[j] + xp[j + 1]) / 2)){
                    vec.push_back(bld[i].id);
                    break;
                }
            }
        }

        if(cnt > 1) cout << endl;
        printf("For map #%d, the visible buildings are numbered as follows:\n", cnt++);
        int len = vec.size();
        for(int i = 0; i < len; i++){
            if(!i) cout << vec[i];
            else cout << " " << vec[i];
        }
        cout << endl;

    }
    return 0;    
}
```

## 彩蛋
北航那道机试题的题意大致是模拟点击window窗口的操作。平面上有几个窗口，以左下角坐标、长度和宽度的形式给出，同时以字符串形式给出每个窗口中的内容。接着给定n组坐标，表示鼠标点击的位置，（鼠标点击到某个窗口的有效位置，即未被遮挡的位置时，该窗口上升到第一层），要求输出这n次点击之后，其中一个指定窗口的可见内容部分。例如，一个窗口中的内容为$"I am the content in this window"$，经过n次点击之后，它在第三层，在它之上有两个窗口且对它的内容有部分遮挡，假设它的可见部分为$"I"$，$"the"$，$"in"$，$"win"$，那么应输出$"I\*\*\*\*the\*\*\*\*\*\*\*\*\*in\*\*\*\*\*\*win\*\*\*"$，其中$'\*'$表示不可见。
思路就是进行模拟。定义一个结构体保存窗口编号、坐标、长度、宽度、内容以及它当前所在层。接下来模拟点击，鼠标每点击一次，根据点击区域调整各个窗口所在层并重新按层次进行排序。模拟完点击之后，对要输出内容的窗口中的每一个字符首先求出它的坐标，接着判断这个坐标是否包含在它之上窗口的区域内，若包含则输出$'\*'$，若不包含则输出原字符。
之所以说后面发现和上面那道题不一致，是因为北航这道题其实本来就是离散的，一个字符相当于一个坐标点，不存在小数坐标点的情况。

*北航计算机的机试基本不涉及算法，比较考验基本功。考场提供的IDE有codeblocks、DEV和VC++6.0，语言要求C/C++，不限制STL的使用，时间是2个小时，题目数量2~3道，我那年是两道，第一道是简单的排序题，用sort函数加上自定义比较函数就能解决*