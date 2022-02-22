---
title: 關節點、橋
date: 2018-03-02 12:59:19
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---
# 關節點
關節點必須符合兩個性質其中一個：
1. 假設目前點為 x, 子孫為 v, low[v] >= dfn[x]
2. 若該點是 root(起始點), 則他的小孩數量 > 1

先來看第一個性質, 當 low[v] 都大於等於當前的編號
就代表所有子孫不會連到當前點的祖先
也就是當前點斷掉的話, 就會分成兩塊

接下來看為何為特別將 root 設個條件(搭配下面的測資)
root 的子孫 low 值一定都比 dfn[root] 大
那拿掉 root 一定可以斷開嘛？
答案是否, 當圖案為一直線
拿掉起點不會產生新的 SCC
只有當 root_child > 1 時才會

## 相關測資
```
#root
Input:
5
1 2
2 3
3 4
4 5
0
0
Output:
3
# dfn and low
Input:
5
1 3 2
2 4 5 3
4 5
0
5
1 2 3
2 3 4 5
4 5
0
Output:
1
1
```

## Code
```cpp
// 割點的 bccid 沒有意義
const int MAX_V = ...;
int V, E;
vector <int> g[MAX_V];
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
                while(1){
                    int u = S.top(); S.pop();
                    bccid[u] = bcc_cnt;
                    if(u == v) break;
                }
                bccid[x] = bcc_cnt;
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

# 橋
求橋就簡單多了, 不需要 stack 去紀錄點

## code
```cpp
const int MAX_V = 10010;
int V, E;
vector <int> g[MAX_V];
int dfn[MAX_V], low[MAX_V], tot;
vector <pii> ans;

void dfs(int x, int fa){
    dfn[x] = low[x] = ++tot;
    for(int i = 0; i < sz(g[x]); i++){
        int v = g[x][i];
        if(v == fa) continue;
        if(!dfn[v]){
            dfs(v, x);
            low[x] = min(low[x], low[v]);
            if(low[v] > dfn[x]){
                ans.pb({x, v});
            }
        }
        else{
            low[x] = min(low[x], dfn[v]);
        }
    }
}

void bcc_tarjan(){
    for(int i = 0; i < V; i++){
        if(!dfn[i]) dfs(i, -1);
    }
}
```

# 例題
1. POJ 1144
2. Uva 796
3. POJ 2117 (求拿掉割點增加的連通單元)
4. POJ 1523 (求拿掉割點增加的連通單元)

# 參考資料
1. http://www.voidcn.com/article/p-mvhamgfv-sm.html
2. www.voidcn.com/link?url=http://www.cnblogs.com/en-heng/p/4002658.html
3. http://www.cnblogs.com/c1299401227/p/5402747.html
4. http://blog.csdn.net/fuyukai/article/details/51303292
5. http://sunmoon-template.blogspot.tw/2016/01/bridge-bridge-connected-component-bcc.html

