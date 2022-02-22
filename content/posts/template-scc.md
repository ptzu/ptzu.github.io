---
title: 強連通單元 SCC
date: 2018-02-27 23:58:21
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---
強連通單元為一集合, 集合內任取兩點, 都可以互相連通
任何有向圖都能分解成幾個 SCC
將 SCC 縮成一個點就會變成 DAG
求 SCC 數量方法有兩種
1. Tarjan
2. 跑兩次 DFS

# Tarjan
dfn[i]: DFS的順序(時間編號)
low[i]: ~~能到達 i 的最小的時間編號~~ i 能達到的最小時間編號

在理解的時候有幾個問題
1. stack 的作用？
用來存放已經走過, 但還沒變成 SCC 的點
2. in_stack 陣列存在的必要性？
在看網路很多模版的時候, 他們是取作 vis 陣列的
而我想說既然有 dfn 陣列, 那還沒有時間戳的點肯定是沒走過的
為何需要 vis 陣列？
首先走過的點可分為兩種
一種是在 stack 裏面, 另一種是不在了
當一個點不在 stack 內, 代表已經成為一個 SCC
若我們拿別的 SCC 來更新自己 low 值肯定出錯
因為已成為 SCC 的點不可能再走到當前點了
所以我認為取做 in_stack 更加恰當, 只有在 stack 的點可以更新當前 low 值
3. 要用 dfn 還是 low 更新？
第 19 行網路上幾乎都是用 dfn 更新
但我想想, 在 stack 的點若可走到當前點
那個點的 low 值也一定可以走到當前點
所以用 low 值更新應該沒錯吧(?
而且也比較好記

<font color=red>
更新: 原來自己 low 的定義搞錯了
low 是 i 可以走到的最小點, 而不是走到 i 的最小點
所以拿 dfn 更新是正確的
雖然拿 low 更新, 在算 SCC 時不會錯
因為如果那點可以走到某個點, 那我接上他, 我必定可以走到那個點
不過在算關節點就會錯了GG(因為會以為有一條邊連到祖先)
</font>


## Code
```cpp
const int MAX_V = 10010;
int V, E;
vector <int> g[MAX_V];
int dfn[MAX_V], low[MAX_V], component[MAX_V], tot, ans, scc_cnt;
stack <int> S;
bool in_stack[MAX_V];

void dfs(int x){
    dfn[x] = low[x] = ++tot;
    in_stack[x] = true;
    S.push(x);
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        if(!dfn[v]){
            dfs(v);
            low[x] = min(low[x], low[v]);
        }
        else if(in_stack[v]){
            low[x] = min(low[x], dfn[v]);
        }
    }
    if(dfn[x] == low[x]){
        while(1){
            int v = S.top(); S.pop();
            in_stack[v] = false;
            component[v] = scc_cnt;
            if(v == x) break;
        }
        scc_cnt++;
    }
}

void tarjan(){
    scc_cnt = 0;
    for(int i = 0; i < V; i++){
        if(!dfn[i]) dfs(i);
    }
}
```

# 兩次 DFS
首先建圖的時候還要額外建一個反向後的圖
然後跑第一次 DFS, 回溯時替點加上編號, 也就是愈晚拜訪的編號愈小
最後再從編號最大的跑第二次 DFS, 而這次 DFS 跑的是反向的邊
所有經過的點就是一個 SCC

為何這樣就可以找到 SCC 呢？
原因是 SCC 內的點互通, 反向後不影響連通性
但 SCC 外的點由於是指向 SCC, 所以反向後就斷開了

時間複雜度： O(|V|+|E|)

## Code
```cpp
const int MAX_V = 10010;
int V, E;
vector <int> g[MAX_V], rg[MAX_V], vs;
bool vis[MAX_V];
int component[MAX_V];

void dfs(int x){
    vis[x] = true;
    for(int i = 0; i < sz(g[x]); i++){
        int u = g[x][i];
        if(!vis[u]) dfs(u);
    }
    vs.pb(x);
}

void rdfs(int x, int k){
    vis[x] = true;
    component[x] = k;
    for(int i = 0; i < sz(rg[x]); i++){
        int u = rg[x][i];
        if(!vis[u]) rdfs(u, k);
    }
}

int scc(){
    memset(vis, 0, sizeof(vis));
    vs.clear();
    for(int i = 0; i < V; i++){
        if(!vis[i]) dfs(i);
    }
    memset(vis, 0, sizeof(vis));
    int k = 0;
    for(int i = sz(vs) - 1; i >= 0; i--){
        if(!vis[vs[i]]) rdfs(vs[i], k++);
    }
    return k;
}
```

## 例題
1. POJ 2186(非裸題)
2. UOJ 146