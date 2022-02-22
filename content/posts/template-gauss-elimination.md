---
title: 高斯消去法
date: 2018-03-13 16:32:04
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---

## Code
```cpp
typedef vector<int> vec;
typedef vector<vec> mat;
int tot; // 自由變數量

void gauss(mat &m){
    int n = m.size();
    tot = 0;
    for(int col = 0; col < n; col++){
        int tmp = tot;
        for(; tmp < n; tmp++) if(m[tmp][col]) break;

        if(tmp != n && m[tmp][col]){
            // 找到開頭是 1 的列, 有可能不在當前位置, 做列交換
            for(int k = 0; k <= n; k++) swap(m[tot][k], m[tmp][k]);
            for(int row = 0; row < n; row++){ // 做消去
                if(row != tot && m[row][col]){
                    for(int k = 0; k <= n; k++){
                        m[row][k] -= m[tot][k];
                    }
                }
            }
            tot++;
        }
    }
    for(int i = tot; i < n; i++){
        if(m[i][n]){
            puts("Oh,it's impossible~!!");
            return;
        }
    }
    printf("%d\n", 1 << (n - tot));
}
```

## 例題
1. POJ 1830 (求自由變數量)
