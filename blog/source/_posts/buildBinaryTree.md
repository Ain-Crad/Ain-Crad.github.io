---
title: Build Binary Tree
date: 2019-03-04 09:39:27
categories:
- Template
tags:
- Binary Tree
---
众所周知，leetcode上不需要自己写读入数据部分的代码。但是这样对于二叉树类型的题目，调试起来就会有点麻烦，因为需要自己现场建一颗树。不太熟悉也不太习惯用leetcode自带的调试功能，所以为了偷懒，我提前写好了一份建二叉树的模板。然后就可以方便的用sublime或者gedit调试了(σ･ω･)σYO♪
<!--more-->
## 使用方法
### 输入
![BinaryTree](/BinaryTree.png)
在源文件路径下的``in.txt``文件中写入这颗二叉树的**先序遍历序列**。和传统意义上的先序遍历序列不同的是，用-1来代表空子树。
譬如上图所示的二叉树的输入方式是：
```
1 2 -1 5 -1 -1 3
```
### 函数返回
函数返回的是指向建立好的二叉树的根节点的指针。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Mon  4 Mar 11:06:42 CST 2019
 *
 */

#include <bits/stdc++.h>

using namespace std;

struct TreeNode{
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x):val(x), left(NULL), right(NULL){}
};
int x;

TreeNode* buildTree(){
    if(cin >> x){
        if(x == -1) return NULL;
        TreeNode* node = new TreeNode(0);
        node->val = x;
        node->left = buildTree();
        node->right = buildTree();
        return node;
    }
    else return NULL;
}

int main(){
    freopen("in.txt", "r", stdin);

    TreeNode* root = buildTree();

    return 0;
}
```