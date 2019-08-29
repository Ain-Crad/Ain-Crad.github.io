---
title: STL中自定义比较函数和重载运算符总结
date: 2018-08-07 22:13:16
categories:
- C++
tags:
- STL
---

STL中自定义比较函数和重载运算符总结。部分内容转载自[Ohazyi Zone's blog](https://www.ohazyi.com/c_cmp/)
<!-- more -->

# sort
## 自定义比较函数
```C++
//示例
struct node{
    int x, y;
};

bool cmp(const node& a, const node& b){
    return a.x > b.x //对x降序排列
}

node d[maxn];

int main(){
    sort(d, d + n, cmp);
    return 0;
}
```
注：自定义比较函数的形式同样也适用于pair类型数据排序。

## 重载运算符<
```C++
//示例
struct node{
    int x, y;
};

bool operator <(const node& a, const node& b){
    return a.x > b.x; //重载为降序排列
}

node d[maxn];

int main(){
    sort(d, d + n);
    return 0;
}
```
注：由于sort默认less(升序),所以重载'<'运算符。重载运算符的操作不能用于pair类型数据的排序，只能作用于结构体或类对象。

# priority_queue
## 重载运算符< (运算符重载函数放到结构体外)
```C++
struct node{
    int x, y;
};

bool operator <(const node& a, const node& b){
    return a.x > b.x //less默认大顶堆，改为小顶堆
}

//bool operator >(const node& a, const node& b){
//    return a.x < b.x //greater默认小顶堆，改为大顶堆
//}

int main(){
    priority_queue<node, vector<node>, less<node> > que;
    //priority_queue<node, vector<node>, greater<node> > que;
    return 0;
}
```
注：同样重载运算符的操作不能用于pair类型数据的排序，只能作用于结构体或类对象。

## 重载运算符< (运算符重载函数放到结构体内)
```C++
struct node{
    int x, y;
    bool operator <(const node& a) const { //必须加const
        return x > a.x;
    }
    //bool operator >(const node& a) const { //必须加const
    //    return x < a.x;
    //}
};

int main(){
    priority_queue<node, vector<node>, less<node> > que;
    //priority_queue<node, vector<node>, greater<node> > que;
    return 0;
}
```
注：同上。

## 自定义比较函数
```C++
struct node{
    int x, y;
};

struct cmp1{
    bool operator()(node a, node b){
        return a.x > b.x; //小顶堆
    }
}

struct cmp2{
    bool operator()(node a, node b){
        return a.x < b.x; //大顶堆
    }
}

int main(){
    priority_queue<node, vector<node>, cmp1> que1; //小顶堆
    priority_queue<node, vector<node>, cmp2> que2; //大顶堆
    return 0;
}
```
注：同样适用于pair类型数据排序。

# 其它
```C++
set<int, greater<int> > st; //按照从大到小，默认是less<int>
typedef pair <int,int> P;
set<P> st; //按照pair的第一个元素来排，第一个相等的话按第二个来排
set<P, greater<P> > st;//按照从大到小的greater来排
```
注：set、map的自定义比较函数和重载运算符与优先队列priority_queue类似。