---
title: NYOJ 16 矩形嵌套
date: 2018-02-12 01:33:06
draft: false
tags: ["DP"]
categories: [解題區, 其他]
---

[題目](http://acm.nyist.edu.cn/JudgeOnline/problem.php?pid=16)

## input
第一行是一个正正数N(0<N<10)，表示测试数据组数，
每组测试数据的第一行是一个正正数n，表示该组测试数据中含有矩形的个数(n<=1000)
随后的n行，每行有两个数a,b(0<a,b<100)，表示矩形的长和宽

## 題解
描述
有n个矩形，每个矩形可以用a,b来描述，表示长和宽。矩形X(a,b)可以嵌套在矩形Y(c,d)中当且仅当a<c,b<d或者b<c,a<d（相当于旋转X90度）。例如（1,5）可以嵌套在（6,2）内，但不能嵌套在（3,4）中。你的任务是选出尽可能多的矩形排成一行，使得除最后一个外，每一个矩形都可以嵌套在下一个矩形内。

输出
每组测试数据都输出一个数，表示最多符合条件的矩形数目，每组输出占一行

## 作法
DAG 上 DP, 求最長路徑
若矩型 a, 可嵌套在矩型 b, 則 a 連一條邊到 b
形成一 DAG

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;
typedef pair <int,int> pii;
typedef long long ll;

#define pb push_back
#define sz(a) (int)a.size()
#define INF 0x3f3f3f3f
#define st first
#define nd second
#define fio ios::sync_with_stdio(false), cin.tie(0)
#define DEBUG(x) printf("Here: %d\n", x), exit(0);
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX
const int MAX_N = 16;

pii p[1010];
int dp[1010];
vector <int> g[1010];

bool ok(pii a, pii b){
    if((a.st < b.st && a.nd < b.nd) || (a.st < b.nd && a.nd < b.st))
        return true;
    return false;
}

int dfs(int x){
    if(dp[x]) return dp[x];
    dp[x] = 1;
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        dp[x] = max(dp[x], dfs(v) + 1);
    }
    return dp[x];
}

int main(){
    int TC;
    scanf("%d", &TC);
    while(TC--){
        memset(dp, 0, sizeof(dp));
        memset(g, 0, sizeof(g));
        int n;
        scanf("%d", &n);
        for(int i = 0; i < n; i++){
            scanf("%d%d", &p[i].st, &p[i].nd);
        }
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                if(ok(p[i], p[j])) g[i].pb(j);
            }
        }
        int mx = -1;
        for(int i = 0; i < n; i++){
            mx = max(mx, dfs(i));
        }
        printf("%d\n", mx);
    }
    return 0;
}
```
