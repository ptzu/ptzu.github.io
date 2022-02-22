---
title: 快速冪(FastPow) 模板
date: 2017-09-18 01:06:00
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---

在 log(N) 時間內求取冪次方

## code
```cpp
int fastpow(int a,int b)  
{  
    int ans = 1,base = a;  
    while(b!=0)  
    {  
        if(b&1)  
            ans *= base;  
        base *= base;  
        b>>=1;  
    }  
    return ans;  
}  
```
