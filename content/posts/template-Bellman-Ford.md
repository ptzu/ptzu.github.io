---
title: Bellman Ford
date: 2017-12-22 14:07:15
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---


## Code
```cpp

struct Edge{
    int from, to, w;
};

int V, E;
int d[MAX_V];
int cnt[MAX_V];
vector <Edge> edges;
set <int> ring; // 紀錄負環

void bellman(int S){
    fill(d, d + V, INF);
    d[S] = 0;
    // 因為邊的順序不一定, 所以負環不一定每輪都會被更新, 多跑幾次
    for(int i = 0; i < V + 10; i++){ 
        for(int j = 0; j < E; j++){
            Edge e = edges[j];
            
            if(d[e.from] + e.w < d[e.to]){
                d[e.to] = d[e.from] + e.w;
                cnt[e.to]++;
                if(cnt[e.to] >= V) ring.insert(e.to);
            }
        }
    }
}
```
