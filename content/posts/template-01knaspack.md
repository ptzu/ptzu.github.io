---
title: 0/1 背包問題
date: 2018-02-02 17:11:55
draft: false
tags: ["Template"]
categories: [解題區, Template, DP]
---

## 狀態
```
dp[i] = 在背包容量 i 情況下的最大價值
```

## 轉移
```
dp[j] = max(dp[j - wi] + vi), for i < n
```

## Code
```cpp
```

## 例題
1. Uva 10130
2. POJ 3624
