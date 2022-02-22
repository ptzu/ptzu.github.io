---
title: 位元DP
date: 2018-02-04 11:54:45
draft: false
tags: ["Template"]
categories: [解題區, Template, DP]
---
以 旅行推銷員問題(TSP) 為例
複雜度: {%math%} O(2^{n}n^{2}) {%endmath%}

# BOTTOM UP

## 狀態
```
S代表目前已經走過的點集合, v為目前的點
dp[S][v]: 已經走完 S, 目前在 v, 所花費的最小權重和
```
## 轉移
找一點不在集合的點 u, 列舉集合的點 v 走到 u 的距離
{%math%}dp[S | (1 << u)][u] = min\left \{  dp[S | (1 << u)][i], dp[S][v] + d[v][u], u \notin S \right \}{%endmath%}

## Code
```cpp
for(int i = 0; i < (1 << n); i++)
    fill(dp[i], dp[i] + n, INF);
dp[1][0] = 0;
for(int S = 1; S < (1 << n); S++){
    for(int i = 0; i < n; i++){
        if((S & (1 << i)) == 0){
            for(int j = 0; j < n; j++){
                if(S & (1 << j))
                    dp[S | (1 << i)][i] = min(dp[S | (1 << i)][i], dp[S][j] + d[j][i]);
            }
        }
    }
}
```

# TOP DOWN 作法
top-down 的作法, 我覺得有點難想, 要搭配圖案

## 狀態
```
S代表目前已經走過的點集合, v為目前的點
dp[S][v]: 從 v 開始走, 走完剩餘的點, 回到起點"需要的"最小權重和
```

## 轉移
我們要從 v 走回起點, 我們需要的子問題值是, v 下一步的點走回起點的值
所以我們才要用 top-down, 把前面的路都先算好, 我們再走過去

{%math%}dp[S][v] = min\left \{  dis[v][u] + dp[S | 1 << u][u], u \notin S \right \}{%endmath%}

這個遞迴式意思是, 找一個不在集合的點 u
他走回起點的值為  dp[S | 1 << u][u]
所以要從 v 走回起點就是: dis[v][u] + dp[S | 1 << u][u]
而 dp[S | 1 << u][u] 已經事先計算好了

## 圖解
初始狀態: dp[0][0] (還沒走過任何點, 從起點開始走回起點, 所需要的最小權重和)
結束狀態: dp[S][0] (已經走過全部點, 從起點開始走回起點, 所需要的最小權重和)
集合 S 從 S - 1 慢慢列舉到 0, 在每個狀態都列舉不同的目前點和下個點
直到 dp[0][0] 就是答案所求

## Code
```cpp
int dp[1 << MAX_N][MAX_N], d[MAX_N][MAX_N];
int solve(){
    for(int i = 0; i < (1 << n); i++){
        fill(dp[i], dp[i] + n, INF);
    }
    dp[(1 << n) - 1][0] = 0; // 已經走完所有的點, 目前點為起點 0, 所以走回起點需要的值為 0
    // 這個初始值想了很久, (1 << n) - 1 代表所有的點都走過
    // 但我們要列舉他的前一步, 所以是 ((1 << n) - 1) - 1
    for(int S = (1 << n) - 2; S >= 0; S--){ // 列舉所有狀態
        for(int v = 0; v < n; v++){ // 列舉目前起點
            for(int u = 0; u < n; u++){ // 列舉下一步的點
                if (!((S >> u) & 1))
                    dp[S][v] = min(dp[S][v], dp[S | 1 << u][u] + d[v][u]);
            }
        }
    }
    // 什麼點都還沒走, 目前的點是起點 0, 要走完剩餘的點回到起點的值即為答案
    return dp[0][0];
}
```

## 例題
1. POJ 3311
2. HDU 5418
