---
title: 大數取 MOD
date: 2017-10-09 08:40:40
draft: false
tags: ["Math"]
categories: [解題區, Template, Math]
---


## Code
```cpp
ll getMod(string s, ll mod){
    ll r = 0;
    for(char c : s)
        r = (r * 10 + c - '0') % mod;
    return r;
}
```
