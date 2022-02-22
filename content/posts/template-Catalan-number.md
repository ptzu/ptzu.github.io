---
title: 卡塔蘭數 Catalan number
date: 2018-02-10 00:02:58
draft: false
tags: ["Template"]
categories: [解題區, Template, DP]
---

一般式: {%math%} C_{n} = \dfrac {1}{n+1}\binom{2n}{n} = \dfrac {\left( 2n\right) !}{\left( n+1\right) !n!} {%endmath%}
遞迴式: {%math%}C_{n} = \dfrac {4*n-2}{n+1}C_{n-1}{%endmath%}
若可以觀察出
h(0) = 1,h(1) = 1
h(n) = h(0)\*h(n-1) + h(1)\*h(n-2) + ... + h(n-1)h(0) (其中 n >= 2)
則有可能是卡塔蘭數
可用於 二元樹種類個數...等問題

## Code
```cpp
big fac[MAX_N * 2];
big res = fac[2 * n] / fac[n + 1] / fac[n];
```

## 例題
1. Uva 10007
2. POJ 2084
