---
title: Prim Algorithm
date: 2017-12-22 14:07:32
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

基本上和 Dijkstra 一樣
只是 d[ ] 從到原點的距離改成到樹的距離

## Code
```cpp
struct Edge{
    int to, w;
};

vector <Edge> g[MAX_V];
int d[MAX_V];
bool inTree[MAX_V];
int V, E;

int prim(int S){
    priority_queue <pii, vector<pii>, greater<pii> >pq;
    fill(d, d + V, INF);
    memset(inTree, 0, sizeof(inTree));
    d[S] = 0;
    pq.push((pii){0, S});
    
    int res = 0, cnt = 0;;
    while(!pq.empty()){
        pii top = pq.top();
        pq.pop();
        int u = top.nd;
        if(inTree[u] || d[u] < top.st) continue;
        
        inTree[u] = true; // 加到樹裡面
        res += d[u];
        cnt++; // 看是否有每個點都被連上
        for(int i = 0; i < sz(g[u]); i++){
            Edge e = g[u][i];
            // 更新距離
            if(e.w < d[e.to]){
                d[e.to] = e.w;
                pq.push((pii){d[e.to], e.to});
            }
        }
    }

    if(cnt != V) return INF;
    return res;
}
```
