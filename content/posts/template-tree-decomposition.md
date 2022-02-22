---
title: 重心分解 Tree Decomposition
date: 2018-03-17 16:11:19
draft: false
tags: ["Template"]
categories: [解題區, Template, Tree]
---
又稱為重心剖分、點分治

由於重心有個特性是, 子樹大小不超過 n / 2
n 為整顆樹的大小

所以每次找重心可以將時間複雜度壓到 O(logn)


## Code
```cpp
struct edge{
    int to, w;
};

int V, k;
vector <edge> g[10010];
bool vis[10010];
int son[10010], centroid, balance_size;
int dis[10010];

void tree_centroid(int x, int fa, const int size){
    int mx_son = 0;
    son[x] = 1;
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i].to;
        if(v == fa || vis[v]) continue;
        tree_centroid(v, x, size);
        son[x] += son[v];
        mx_son = max(mx_son, son[v]);
    }
    mx_son = max(mx_son, size - son[x]);
    if(mx_son < balance_size){
        balance_size = mx_son;
        centroid = x;
    }
}

void tree_decomposition(int x, const int size){
    balance_size = INF;
    tree_centroid(x, -1, size);
    int cent = centroid;
    vis[cent] = true;
    do something...
    for(int i = 0; i < sz(g[cent]); i++){
        int v = g[cent][i].to, w = g[cent][i].w;
        if(vis[v]) continue;
        do something...
        tree_decomposition(v, son[v]);
    }
}

```

## 例題
1. POJ 1741

## 參考資料
1. https://wenku.baidu.com/view/e932a21614791711cc791725.html
2. http://codeforces.com/blog/entry/58025
3. https://www.cnblogs.com/zinthos/p/3899075.html