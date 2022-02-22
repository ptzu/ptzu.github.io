---
title: Sollin Algorithm
date: 2018-01-12 12:59:18
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

求 MST, 不過速度有點慢

## Code
```cpp
truct edge{
    int u, v, w;
};

struct Union_Find {
    int par[1010], num[1010];
    void init(int n){
        for(int i = 0; i < n; i++){
            par[i] = i;
            num[i] = 1;
        }
    }
    int find(int x){
        if(x == par[x])
            return x;
        else
            return par[x] = find(par[x]);
    }
    void unite(int a, int b){
        a = find(a);
        b = find(b);
        if(a == b) return ;
        if(num[a] > num[b]) swap(a, b);
        par[a] = b;
        num[b] += num[a];
    }
}U;

edge edges[100000];
int V, E;
int mn[1000];
int ComponentSize; // 一開始等於V

int sollin(){
    int ans = 0;
    while(ComponentSize > 1){
        for(int i = 0; i < V; i++) mn[i] = INT_MAX;
        // 找出每個 Component 的最小邊是誰
        for(int i = 0; i < E; i++){
            edge e = edges[i];
            int a = U.find(e.u);
            int b = U.find(e.v);
            if(a == b) continue;
            if(e.w < mn[a]) mn[a] = e.w;
            if(e.w < mn[b]) mn[b] = e.w;
        }
        // 如果這條邊是兩個 Component 的最小邊, 則加入
        for(int i = 0; i < E; i++){
            edge e = edges[i];
            int a = U.find(e.u);
            int b = U.find(e.v);
            if(a == b) continue;
            if(e.w == mn[a] && e.w == mn[b]){
                U.unite(a, b);
                ans += e.w;
                ComponentSize--;
            }
        }
    }
    return ans;
}
```
例題：
1. POJ 2421