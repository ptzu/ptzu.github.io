---
title: 2-SAT
date: 2018-03-08 12:02:50
draft: false
tags: ["Template"]
categories: [解題區, Template, Graph]
---

當有{%math%}(a\vee b){%endmath%}這種型式的時候
可以轉成{%math%}(\neg a\Rightarrow b\wedge \neg b\Rightarrow a){%endmath%}
箭頭左邊建一條邊到右邊

addClause 的 flag 是代表該變數要不要取 not

只要{%math%}x, \neg x{%endmath%}不在同一個 SCC 內, 命題為真

## 邏輯定律
1. 肯定前件律(MP): {%math%}p\supset q,\ p/\ q{%endmath%}
2. 否定後件律(MT): {%math%}p\supset q,\ \neg q/\ \neg p{%endmath%}
3. 連言律(Conj): {%math%}p,\ q/\ p\wedge q{%endmath%}
4. 簡化律(Simp): {%math%}p\wedge q/\ p{%endmath%}
5. 選言三段論(DS): {%math%}p\vee q,\ \neg p/\ q{%endmath%}
6. 假言三段論(HS): {%math%}p\supset q,\ q\supset r/\ p\supset r{%endmath%}
7. 添加律(Add): {%math%}p/\ p\vee q{%endmath%}
8. 建構兩難律(CD): {%math%}p\supset q,\ r\supset s,\ p\vee r/\ q\vee s{%endmath%}
9. 雙重否定律(DN): {%math%}p\equiv \neg \neg p{%endmath%}
10. 笛摩根定律(DeM): {%math%}\neg (p\wedge q)\equiv \neg p \vee \neg q{%endmath%}
11. 交換律(Comm): {%math%}p\vee q\equiv q\vee p{%endmath%}
12. 結合律(Assoc): {%math%}p\vee (q\vee r) \equiv (p\vee q)\vee r{%endmath%}
13. 分配律(Dist): {%math%}p\wedge (q\vee r)\equiv (p\wedge q)\vee (p\wedge r){%endmath%}
{%math%}p\vee (q\wedge r)\equiv (p\vee q)\wedge (p\vee r){%endmath%}
14. 換值換位律(Contra): {%math%}p\supset q\equiv \neg q\supset \neg p{%endmath%}
15. 蘊涵律(Impl): {%math%}p\supset q\equiv \neg p\vee q{%endmath%}
16. 等值律(Equiv): {%math%}(p\equiv q)\equiv (p\supset q)\wedge (q\supset p)\equiv (p\wedge q)\vee (\neg p\wedge \neg q){%endmath%}
17. 移出律(Exp): {%math%}(p\wedge q)\supset r\equiv p\supset (q\supset r){%endmath%}
18. 恆真律(Taut): {%math%}p\supset q\equiv \neg p\vee q{%endmath%}

其中 {%math%}\supset {%endmath%} 是條件符號
## Code
```cpp
const int MAX_V = ...;

struct TwoSAT{
    int V, E;
    vector <int> g[MAX_V * 2];
    int dfn[MAX_V * 2], low[MAX_V * 2], sccid[MAX_V * 2], tot, scc_cnt;
    stack <int> S;
    bool in_stack[MAX_V * 2];

    void init(int n){
        this->V = n * 2;
        for(int i = 0; i < V; i++) g[i].clear();
        memset(dfn, 0, sizeof(dfn));
        memset(low, 0, sizeof(low));
        memset(sccid, 0, sizeof(sccid));
        tot = 0;
    }

    void addClause(int x, int xflag, int y, int yflag){
        x = x * 2 + xflag;
        y = y * 2 + yflag;
        g[x ^ 1].pb(y);
        g[y ^ 1].pb(x);
    }

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
                sccid[v] = scc_cnt;
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

    bool solve(){
        tarjan();
        for(int i = 0; i < V; i += 2){
            if(sccid[i] == sccid[i ^ 1]) return false;
        }
        return true;
    }
}solver;
```

# 例題
1. Uva 10319
