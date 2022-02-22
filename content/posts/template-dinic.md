---
title: Max Flow
date: 2018-01-04 19:13:59
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---
Dinic 演算法
時間複雜度: O({%math%}EV^{2}{%endmath%})

## Code
```cpp
struct edge{
    int to, cap, rev;
};

vector <edge> g[MAX_V];
int iter[MAX_V];
int level[MAX_V];

void add_edge(int u, int v, int w){
    g[u].pb((edge){v, w, sz(g[v])});
    g[v].pb((edge){u, 0, sz(g[u]) - 1});
}

void bfs(int S){
    memset(level, -1, sizeof(level));
    queue <int> q;
    level[S] = 0;
    q.push(S);

    while(!q.empty()){
        int v = q.front();
        q.pop();
        for(int i = 0; i < sz(g[v]); i++){
            edge& e = g[v][i];
            if(e.cap > 0 && level[e.to] < 0){
                level[e.to] = level[v] + 1;
                q.push(e.to);
            }
        }
    }
}

int dfs(int u, int t, int f){
    if(u == t) return f;
    for(int& i = iter[u]; i < sz(g[u]); i++){
        edge& e = g[u][i];
        if(e.cap > 0 && level[u] < level[e.to]){
            int d = dfs(e.to, t, min(f, e.cap));
            if(d > 0){
                e.cap -= d;
                g[e.to][e.rev].cap += d;
                return d;
            }
        }
    }
    return 0;
}

int max_flow(int s, int t){
    int flow = 0;
    for(;;){
        bfs(s);
        if(level[t] < 0) return flow;
        memset(iter, 0, sizeof(iter));
        int f;
        while((f = dfs(s, t, INF)) > 0){
            flow += f;
        }
    }
}
```

## 例題
1. POJ 3281
