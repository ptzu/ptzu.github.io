---
title: 併查集 Disjoint set
date: 2017-02-02 18:22:18
draft: false
tags: ["Template"]
categories: [解題區, Template, Data Structure]
---
## code
```cpp
const int MAX_N = ...;

struct Union_Find {
    int par[MAX_N], num[MAX_N];
    void init(int n){
        for(int i = 0; i < n; i++){
            par[i] = i;
            num[i] = 1;
        }
    }
    int find(int x){
        if(x == par[x]) return x;
        else return par[x] = find(par[x]);
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
```
