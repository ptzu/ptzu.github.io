---
title: SPFA
date: 2017-12-22 14:07:27
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

可偵測負環
和 Dijkstra 差不多, 多用了 cnt 陣列去紀錄每個點被更新幾次
如果有個點被更新超過 V 次, 代表有負環

## Code
```cpp
struct Edge {
    int to, w;
};

const int MAX_V = ...;

int V;
vector<Edge> g[MAX_V];
int d[MAX_V];
int cnt[MAX_V];

bool SPFA(int S) { // 回傳有無負環
    fill(d, d + V, INF);
    fill(cnt, cnt + V, 0);
    priority_queue< pii, vector<pii>, greater<pii> > pq;

    d[S] = 0;
    pq.push(pii(0, S));
    cnt[S] = 1;

    while (!pq.empty()) {
        pii top = pq.top(); pq.pop();
        int u = top.nd;

        if (d[u] < top.st) continue;

        for (int i = 0; i < (int)g[u].size(); i++) {
            const Edge& e = g[u][i];
            if (d[e.to] > d[u] + e.w) {
                d[e.to] = d[u] + e.w;
                pq.push(pii(d[e.to], e.to));

                cnt[e.to]++; // 每次更新就 +1
                if (cnt[e.to] >= V)
                    return true;
            }
        }
    }

    return false;
}

```

## 例題
1. POJ 3259
