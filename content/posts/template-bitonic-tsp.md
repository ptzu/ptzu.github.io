---
title: 雙調TSP (Bitonic TSP)
date: 2018-02-08 17:09:02
draft: false
tags: ["Template"]
categories: [解題區, Template, DP]
---
## 前言
從字面理解不知道是什麼怪東西。。。
據說是 CLRS 書 15-1 的習題, 有空再去翻看看~

簡單來說是 TSP 的簡化版, 限定從最左走到最右, 而且必須嚴格遞增/減
所以不再是 NP-Hard, 而是有 {%math%}O(n^{2}){%endmath%} 的作法

## 狀態
```
定義dp[i][j]: 從起點 1 出發, 走到 i 和 j 的最小總和(上下不同兩條路徑)
dp[N][N] 即為答案, 因為可以看成 1 走到 N, N 走到 1
```
![](https://i.imgur.com/QNvTQxF.png)

## 轉移
要怎麼保證上下兩條路徑, 不會走到相同的點呢？
針對每個點, 每次只會把它放到上面或下面的路徑
假設目前要加的點是 j
上面那條路的最後點有可能是 1 ~ j - 1
分為兩個 case 討論
1. i < j - 1
點 j 可以從 j - 1 推導過來
```
dp[i][j] = dp[i][j - 1] + dis[j - 1][j], for i < j - 1
```
![](https://i.imgur.com/CZZVqlg.png)
2. i == j - 1
點 j 不能從 j - 1 推導過來, 為什麼呢？
看著上面的遞迴式, 若是這樣, 就會變成從 dp[j - 1][j - 1] 推過來
而這樣代表上下兩路都走到了 j - 1 這點, 違反了題目限制
也許你想那就從 dp[j - 1][1 ~ j - 2] 推過來就好了呀？
這樣的話是沒錯, 但不就跟上面那種 case 一樣了嘛？(上下兩路交換本質沒差)
所以我們換個想法, 不從 j - 1 推導, 而是從另外一條路加上點 j
```
dp[j - 1][j] = min(dp[i][j - 1] + dis[i][j]), for i < j - 1
```
![](https://i.imgur.com/RLiLRqd.png)

## AC code
```cpp
double TSP(){
    dp[1][2] = dis[1][2];
    for(int j = 3; j <= n; j++){ // 目前要加入的點
        // i < j - 1 的 case
        for(int i = 1; i < j - 1; i++){ // 上路不同的終點
            dp[i][j] = dp[i][j - 1] + dis[j - 1][j];
        }
        dp[j - 1][j] = INF;
        // i == j - 1 的 case, 迴圈的 i 非彼 i, 而是列舉不同的終點, 將 j 加上去
        for(int i = 1; i < j - 1; i++){
            dp[j - 1][j] = min(dp[j - 1][j], dp[i][j - 1] + dis[i][j]);
        }
    }
    dp[n][n] = dp[n - 1][n] + dis[n - 1][n];
    return dp[n][n];
}
```

## 例題
1. POJ 2677
