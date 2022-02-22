---
title: Kruskal Algorithm
date: 2018-01-12 13:23:37
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

時間瓶頸在排序
複雜度： O(ElogV)

## Code
```cpp

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

struct edge{
    int u, v, w;
    bool operator <(const edge& e) const{
        return w < e.w;
    }
};
int n;
vector <edge> edges;

int kruskal(){
    sort(edges.begin(), edges.end());
    int ans = 0;
    int cnt = 0;
    for(int i = 0; i < sz(edges); i++){
        if(cnt > n - 1)
            break;
        edge e = edges[i];
        if(U.find(e.u) != U.find(e.v)){
            U.unite(e.u, e.v);
            ans += e.w;
            cnt++;
        }
    }
    return ans;
}
```
