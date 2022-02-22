---
title: 樹重心 Tree Centroid
date: 2018-03-14 14:42:43
draft: false
tags: ["Template"]
categories: [解題區, Template, Tree]
---

將樹的一點移除, 會產生許多子樹
其中使子樹中最大數量最小的點為重心

方法為跑一次 DFS, 紀錄每個點的子樹數量
同時也更新最小值

## Code
```cpp
vector <int> g[20010];
int son[20010], centroid, balance_size;
int V;

void dfs(int x, int pa){
    int mx_son = 0;
void tree_centroid(int x, int fa, const int size){
    int mx_son = 0;
    son[x] = 1;
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        if(v == fa) continue;
        tree_centroid(v, x, size);
        son[x] += son[v];
        mx_son = max(mx_son, son[v]);
    }
    mx_son = max(mx_son, size - son[x]);
    if(mx_son < balance_size || (mx_son == balance_size && x < centroid)){
        balance_size = mx_son;
        centroid = x;
    }
}
```

## 例題
1. POJ 1655
