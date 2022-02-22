---
title: KMP Algorithm
date: 2018-03-18 09:54:26
draft: false
tags: ["Template"]
categories: [解題區, Template, String]
---

index 從 0 開始
無配對為 -1

## AC code
```cpp
const int MAX_N = ...;

int fail[MAX_N];

void getf(string s){
    fail[0] = -1;
    int idx = -1;
    for(int i = 1; i < s.length(); i++){
        while(idx != -1 && s[i] != s[idx + 1]){
            idx = fail[idx];
        }
        if(s[i] == s[idx + 1]){
            fail[i] = idx + 1;
            idx++;
        }
        else fail[i] = -1;
    }
}

void KMP(string s, string t){
    int idx = -1, t_len = t.length();
    for(int i = 0; i < s.length(); i++){
        while(idx != -1 && s[i] != t[idx + 1]){
            idx = fail[idx];
        }
        if(s[i] == t[idx + 1]){
            idx++;
        }
        if(idx == t_len - 1){
            idx = fail[idx];
        }
    }
}
```
## 例題
1. POJ 3461

## 參考資料
1. https://www.ptt.cc/bbs/b99902HW/M.1300796484.A.EE1.html