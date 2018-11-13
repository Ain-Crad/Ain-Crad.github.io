---
title: Uva10562-Undraw the trees
date: 2018-11-13 07:33:42
categories:
- ACM
tags:
- 二叉树
---
看图写树。把类似如图所示的树用"$(A(B()C(E()F())D(G())))$"的形式输出出来。在奇怪的地方卡了好久= =，记录一下。
![tree](/tree.png)
<!--more-->
## 链接
[Uva10562-Undraw the trees](https://vjudge.net/problem/UVA-10562)

## 题解
直接递归输出。主要说一下自己的卡点：最开始代码34行部分写的是$r = y$，WA到怀疑人生，后来突然想到把字符串读入字符数组中时是覆盖操作，也就是说如果新读入的字符串的长度比已经缓存的要短的话，之前的字符串会有残留。例如，buf[10] = "abcdef"，然后再读入新的字符串"xyz"后，buf中的内容是"xyz0ef"（其中0表示字符串结束标志'\0'）,自己也就是坑在了这个地方。解决方法有两种，一种是让$r = l$，然后遍历到'\0'时结束,也就是注释的那种写法；另一种是每次都memset一下= =（经历这次事件后一定每次都记得memset...）。

## 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Tue 13 Nov 08:09:46 CST 2018
 *
 */

#include <bits/stdc++.h>

using namespace std;

const int maxn = 200 + 7;
char mp[maxn][maxn];
int t;
int n;

void dfs(int x, int y){
	char c = mp[x][y];
	cout << c << "(";
	if(x + 1 < n && mp[x + 1][y] == '|'){
		int l = y;
		while(l > 0 && mp[x + 2][l - 1] == '-') l--;
		int r = l;
		while(mp[x + 2][r] == '-' && mp[x + 3][r] != '\0') r++;
		for(int i = l; i < r; i++){
			if(!isspace(mp[x + 3][i])){
				dfs(x + 3, i);
			}
		}
		
		/*
		while(mp[x + 2][l] == '-' && mp[x + 3][l] != '\0'){
			if(!isspace(mp[x + 3][l])) dfs(x + 3, l);
			l++;
		}
		*/

	}
	cout << ")";
}

int main(){
	#ifndef ONLINE_JUDGE
		freopen("../in.txt", "r", stdin);
	#endif
	cin >> t;
	getchar();
	while(t--){
		memset(mp, 0, sizeof(mp));
		n = 0;
		cout << "(";
		while(fgets(mp[n], maxn, stdin)){
			if(mp[n][0] == '#') break;
			n++;
		}
		if(n){
			for(int i = 0; i < maxn; i++){
				if(mp[0][i] != ' '){
					dfs(0, i);
					break;
				}
			}
		}
		cout << ")" << endl;
	}
	return 0;
}
```