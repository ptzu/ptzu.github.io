---
title: Extgcd 模板
date: 2017-09-17 15:23:05
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---
求取 ax + by = gcd(a, b) 此方程式之解
並回傳 gcd(a, b)

## code
```cpp
int extgcd(int a, int b, int &x, int &y){
    int d = a;
    if(b != 0){
        d = extgcd(b, a % b, y, x);
        y -= (a / b) * x;
    }
    else{
        x = 1;
        y = 0;
    }
    return d;
}
```
