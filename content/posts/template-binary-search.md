---
title: 二分搜
date: 2017-03-01 00:13:45
draft: false
tags: ["Template"]
categories: [解題區, Template, 其他]
---
## 0, 1型
```cpp
bool C(int x){

}

int sol(){
    int l = 0, u = N;
    if(C(l)) return l;
    if(!C(u)) return -1;

    while(u - l > 1){
        int mid = (u + l) / 2;
        if(C(mid)) l = mid;
        else u = mid;
    }
    return u;
}
```

## 1, 0型
```cpp
bool C(int x){

}

int sol(){
    int l = 0, u = N;
    if(!C(l)) return -1;
    if(C(u)) return u;

    while(u - l > 1){
        int mid = (u + l) / 2;
        if(C(mid)) u = mid;
        else l = mid;
    }
    return l;
}
```
