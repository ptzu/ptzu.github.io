---
title: LCA (Lowest Common Ancestor)
date: 2018-02-16 19:57:50
draft: false
tags: ["Template"]
categories: [解題區, Template, Tree]
---
待學方法: Tarjan 離線O(n), RMQ在線

使用倍增法(binary lifting)求 LCA
par[v][k] 為點 v 往上走 {%math%}2^{k}{%endmath%} 次的點
首先跑一次 DFS 求出各點的深度
接著跑 LCA 時, 先將比較深的那個點, 往上走到相同高度
由於 LCA 那個點往上走, 每個點都是共同祖先
所以可以用二分搜尋的概念
```
0 0 0 0 1 1 1 1
^             ^
走 2^0 次     走 2^7 次
0 代表不是祖先
1 代表是祖先
```
一開始走最大步, 接著逐漸縮小
如果該點不是祖先, 就往上走
是祖先就不動, 最後停下來的地方就是邊界

可求任意點對的 LCA
注意: 點是 0-indexed, 需先求好每個點走 1 步的 par
複雜度: {%math%}O(NlogN){%endmath%}

## Code
```cpp
const int MAX_N = 10000;
const int MAX_LOG_N = 14;

int N, root;
int depth[MAX_N];
int par[MAX_N][MAX_LOG_N];
vector <int> g[MAX_N];

void init(){
    memset(par, -1, sizeof(par));
    memset(g, 0, sizeof(g));
}

void dfs(int u, int p, int d){
    depth[u] = d;
    for(int i = 0; i < sz(g[u]); i++){
        int v = g[u][i];
        if(v != p){
            dfs(v, u, d + 1);
        }
    }
}

void build(){
    for(int i = 0; i < N; i++){
        if(par[i][0] == -1){
            root = i;
            break;
        }
    }
    dfs(root, -1, 0);
    // 建立倍增數組
    for(int k = 0; k + 1 < MAX_LOG_N; k++){
        for(int v = 0; v < N; v++){
            if(par[v][k] < 0) par[v][k + 1] = -1; // 目前已經超過根
            else par[v][k + 1] = par[par[v][k]][k];
        }
    }
}

int lca(int u, int v){
    if(depth[u] > depth[v]) swap(u, v);
    for(int k = 0; k < MAX_LOG_N; k++){
        if((depth[v] - depth[u]) & (1 << k)){
            v = par[v][k];
        }
    }
    if(u == v) return u;
    for(int k = MAX_LOG_N - 1; k >= 0; k--){
        if(par[u][k] != par[v][k]){
            u = par[u][k];
            v = par[v][k];
        }
    }
    return par[u][0];
}
```

## 例題
1. [POJ 1330](http://abcd40404.github.io/2018/02/16/poj1330/)
