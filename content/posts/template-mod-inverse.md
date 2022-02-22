---
title: Mod Inverse 模板
date: 2017-10-09 08:30:52
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---


## AC code
```cpp
int mod_inverse(int a, int m){
    int x, y;
    // x 是所求
    extgcd(a, m, x, y);
    return (m + x % m) % m;
}
```
