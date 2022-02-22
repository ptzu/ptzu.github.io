---
title: 常用技巧篇
date: 2018-02-20 13:32:22
draft: false
tags: ["Template"]
categories: [解題區, Template, 其他]
---
## 列舉因數

## 判斷後綴字串
a 是否為 b 的後綴字串
```cpp
bool Suf(string a, string b){
    if(a.length() > b.length()) return false;
    return a == b.substr(b.length() - a.length(), a.length());
}
```

## 分堆

## 離散化

## 平移視窗

## 浮點數

## Graph linked list
```cpp
const int MAX_N = 50010;
struct node{
    int to, w;
    node* next;
}*g[MAX_N], edges[MAX_N * 3];
int tot;

inline void addEdge(int a, int b, int c){
    node *e = &edges[tot++];
    e->to = b;
    e->w = c;
    e->next = g[a];
    g[a] = e;
}
```
