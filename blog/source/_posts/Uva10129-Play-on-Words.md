---
title: Uva10129-Play on Words-欧拉路
date: 2018-11-12 07:25:48
categories:
- ACM
- 欧拉路
tags:
- 欧拉路
- DFS
---
输入n个单词，是否可以把所有这些单词排成一个序列，使得每个单词的第一个字母和上一个单词的最后一个字母相同。
<!--more-->
# 链接
[Uva10129-Play on Words](https://vjudge.net/problem/UVA-10129)

# 题解
建图。将字母看做结点，单词看做有向边。例如单词acm, mouse建图为a->m->e。如果该图存在一条欧拉路则问题有解。首先判断忽略边方向后的无向图是否连通，有两种方法来判断，一种是DFS，另一种是并查集（此代码采用DFS判断）。然后依据建图时统计的各个结点的出度和入度信息判断是否有欧拉路存在。
**有一个需要注意的地方是，可能两个结点之间存在多条边，但是判断连通性的时候可以忽略，多边存在时按单边处理，不影响连通性。**

# 代码
```C++
/*
 *
 * Author : Aincrad
 *
 * Date : Mon  12 Nov 07:00:18 CST 2018
 *`
 */
 
#include <bits/stdc++.h>

using namespace std;

struct Degree{
	int in;
	int out;
};
int G[30][30];
Degree d[30];
int T, n;

void dfs(int x){
	for(int i = 0; i < 26; i++){
		if(G[x][i]){
			G[x][i] = G[i][0] = 0;
			dfs(i);
		}
	}
}

bool JudgeEulr(){
	vector<int> vec;
	for(int i = 0; i < 26; i++){
		if(d[i].in != d[i].out){
			vec.push_back(i);
		} 
	}
	int len = vec.size();
	if(len == 0) 
		return true;
	else if(len == 2){
		if((d[vec[0]].in - 1 == d[vec[0]].out && d[vec[1]].out - 1 == d[vec[1]].in) || (d[vec[0]].out - 1 == d[vec[0]].in && d[vec[1]].in - 1 == d[vec[1]].out))
			return true;
		else
			return false;
	}
	else
		return false;
}

int main(){
	//ios::sync_with_stdio(false);
	//cin.tie(0);
	//cout.tie(0);
	#ifndef ONLINE_JUDGE
		freopen("in.txt", "r", stdin);
	#endif

	cin >> T;
	while(T--){
		cin >> n;
		memset(G, 0, sizeof(G));
		memset(d, 0, sizeof(d));
		string s;
		int a, b;
		for(int i = 0; i < n; i++){
			cin >> s;
			int len = s.length();
			a = s[0] - 'a', b = s[len - 1] - 'a';
			G[a][b] = 1, G[b][a] = 1;
			d[a].out++, d[b].in++;
		}
		int cnt = 0;
		for(int i = 0; i < 26; i++){
			for(int j = 0; j < 26; j++){
				if(G[i][j]){
					dfs(i);
					cnt++;
				}
			}
		}
		//cout << cnt << endl;
		if(cnt > 1){
			cout << "The door cannot be opened." << endl;
		}
		else{
			if(JudgeEulr())
				cout << "Ordering is possible." << endl;
			else
				cout << "The door cannot be opened." << endl;
		}
	}

	return 0;
}

```