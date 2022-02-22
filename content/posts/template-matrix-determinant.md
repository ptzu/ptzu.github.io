---
title: 矩陣行列式 Matrix Determinant
date: 2018-03-11 16:44:05
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---


## Code
```cpp
typedef vector<ll> vec;
typedef vector<vec> mat;

ll determinant(mat m){
    int n = m.size();
    ll det = 1;
    for(int col = 0; col < n; col++){
        for(int row = col + 1; row < n; row++){
            int x = col, y = row; // 目前列, 下一列
            while(m[y][col]){ // 下一列不為 0 就繼續做
                ll q = m[x][col] / m[y][col];
                for(int k = 0; k < n; k++){
                    m[x][k] -= m[y][k] * q;
                }
                swap(x, y);
            }
            if(x != col){
                swap(m[col], m[row]);
                det = -det;
            }
        }
        if(m[col][col] == 0) return 0;
        else det *= m[col][col];
    }
    return det;
}
```

## 例題
1. Uva 684