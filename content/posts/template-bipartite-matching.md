---
title: Bipartite Matching
date: 2018-01-06 16:19:43
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---
匈牙利演算法(Hungarian Algorithm)
時間複雜度: O({%math%}n^{3}{%endmath%})

## Code
```cpp
const int MAX_V = 1010;
int V;
vector <int> g[MAX_V];
int match[MAX_V];
bool vis[MAX_V];

void add_edge(int u, int v){
    g[u].pb(v);
    g[v].pb(u);
}

bool dfs(int u){
    vis[u] = true;
    for(int i = 0; i < sz(g[u]); i++){
        int v = g[u][i], w = match[v];
        if(w < 0 || (!vis[w] && dfs(w))){
            match[u] = v;
            match[v] = u;
            return true;
        }
    }
    return false;
}

int bipartite_matching(){
    int res = 0;
    memset(match, -1, sizeof(match));
    for(int i = 0; i < V; i++){
        if(match[i] < 0){
            memset(vis, 0, sizeof(vis));
            if(dfs(i)) res++;
        }
    }
    return res;
}
```
