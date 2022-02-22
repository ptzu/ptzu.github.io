---
title: Dijkstra Algorithm
date: 2017-10-01 11:16:22
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

## Code
```cpp
struct edge{
    int to, w;
};
int d[MAX_N];
vector <edge> g[MAX_N];

int dijkstra(int S){
    priority_queue <pii, vector<pii>, greater<pii> > pq; //d, v
    fill(d, d + n, INF);
    d[S] = 0;
    pq.push((pii){0, S});

    while(!pq.empty()){
        pii top = pq.top();
        pq.pop();
        int u = top.nd;

        if(d[u] < top.st) continue;

        for(int i = 0; i < sz(g[u]); i++){
            edge e = g[u][i];
            if(d[u] + e.w < d[e.to]){
                d[e.to] = d[u] + e.w;
                pq.push((pii){d[e.to], e.to});
            }
        }
    }
    return ...;
}
```

## 例題
1. Uva 10986
