---
title: 樹狀數組 Binary Indexed Tree
date: 2018-01-24 16:36:06
draft: false
tags: ["Template"]
categories: [解題區, Template, Data Structure]
---

可做到:
1. 單點更新
2. 前綴和查詢(範圍查詢)
---
注意是 1-based index

## Code
```cpp
const int MAX_N = 20000;
int bit[MAX_N + 10];

int add(int i, int x){
    while(i <= n){
        bit[i] += x;
        i += i & -i;
    }
}

int sum(int i){
    int ans = 0;
    while(i){
        ans += bit[i];
        i -= i & -i;
    }
    return ans;
}
```

## 例題
1. POJ 1990
