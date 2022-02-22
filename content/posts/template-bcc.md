---
title: 雙連通單元 BCC
date: 2018-03-04 19:30:28
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---
在一個無向圖中
若任兩點, 存在"兩條點不重複路徑", 則稱此圖為 點-雙連通
而點-雙連通內部沒有割點
若任兩點, 存在"兩條邊不重複路徑", 則稱此圖為 邊-雙連通

點-雙連通的最大子圖稱為雙連通單元(分量)

# 點-雙連通單元
性質:
1. 條件: low[v] >= dfn[x]
2. 割點可以屬於多個連通單元
3. 任兩個連通單元的共同點最多一個, 也就是割點
4. 連通單元內不會有割點
5. 可順便求割點

這在網路上有兩種寫法
一種是在 stack 存點, 另一種存邊
由於割點可以屬於多個單元, 所以在 pop 時, 不能將割點 pop 出去
而這份 code, 不會將割點 pop 出去, 但有個副作用是 root 會留在 stack
多筆測資使用要小心

存邊的寫法有空再來理解

## 存點 Code
```cpp
// 割點的 bccid 沒有意義
const int MAX_V = ...;
int V, E;
vector <int> g[MAX_V], bcc[MAX_V];
int dfn[MAX_V], low[MAX_V], tot, bccid[MAX_V], bcc_cnt;
bool cut[MAX_V];
stack <int> S;

void dfs(int x, int fa){
    int child = 0;
    dfn[x] = low[x] = ++tot;
    S.push(x);
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        if(!dfn[v]){
            dfs(v, x);
            child++;
            low[x] = min(low[x], low[v]);
            if(low[v] >= dfn[x]){
                cut[x] = true;
                bcc[bcc_cnt].clear();
                while(1){
                    int u = S.top(); S.pop();
                    bccid[u] = bcc_cnt;
                    bcc[bcc_cnt].pb(u);
                    if(u == v) break;
                }
                bccid[x] = bcc_cnt; // 沒有意義
                bcc[bcc_cnt].pb(x);
                bcc_cnt++;
            }
        }
        else if(dfn[v] < dfn[x] && v != fa){ // 反向邊
            low[x] = min(low[x], dfn[v]);
        }
    }
    // 樹根
    if(fa == -1 && child < 2) cut[x] = false;
}

void bcc_tarjan(){
    bcc_cnt = 0;
    for(int i = 0; i < V; i++){
        if(!dfn[i]) dfs(i, -1);
    }
}
```
# 邊-雙連通分量
性質:
1. 條件: low[v] > dfn[x]
2. 連通單元之間沒有共同點, 可以安心 pop
3. 橋即為連通單元之間的連結, 因此拔掉橋, 就得到各個連通單元
4. 連通單元內沒有橋
5. 可順便求橋

我有試過用上面的 code, 改一下條件
不過答案會錯
因此就改寫法了

## Code
```cpp
const int MAX_V = ...;
int V, E;
vector <int> g[MAX_V], bcc[MAX_V];
int dfn[MAX_V], low[MAX_V], tot, bccid[MAX_V], bcc_cnt;
stack <int> S;

void dfs(int x, int fa){
    int child = 0;
    dfn[x] = low[x] = ++tot;
    S.push(x);
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        if(!dfn[v]){
            dfs(v, x);
            child++;
            low[x] = min(low[x], low[v]);
            if(low[v] > dfn[x]){
                ...存橋
            }
        }
        else if(dfn[v] < dfn[x] && v != fa){ // 反向邊
            low[x] = min(low[x], dfn[v]);
        }
    }
    if(dfn[x] == low[x]){ // 不會走到祖先, 因此整團為一個單元
        while(1){
            int v = S.top(); S.pop();
            bccid[v] = bcc_cnt;
            bcc[bcc_cnt].pb(v);
            if(v == x) break;
        }
        bcc_cnt++;
    }
}
void bcc_tarjan(){
    bcc_cnt = 0;
    for(int i = 0; i < V; i++){
        if(!dfn[i]) dfs(i, -1);
    }
}
```

# 構造 邊-雙連通單元
若一張圖有橋, 該怎麼把整張圖變成 邊-雙連通單元呢？
首先將連通單元求出, 縮點後
需要加的邊為: (度數為 1 的點) + 1 / 2
可參考 POJ 3352

# 例題
1. POJ 1144(求割點)
2. POJ 3352(邊-雙連通單元)

# 參考
1. https://www.zhihu.com/question/40746887
2. https://www.byvoid.com/zht/blog/biconnect