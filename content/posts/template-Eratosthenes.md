---
title: Eratosthenes 模板
date: 2017-09-17 22:39:17
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---
求取小於 n 的質數

## code
```cpp
const int MAX_N = 1e5;
vector <int> prime;
bool is_prime[MAX_N + 10];

int sieve(int n){
    fill(is_prime, is_prime + n, true);
    is_prime[0] = is_prime[1] = false;
    for(int i = 2; i <= n; i++){
        if(is_prime[i]){
            //紀錄質數
            prime.push_back(i);
            // 消除 i 的倍數
            for(int j = i * 2; j <= n; j += i){
                is_prime[j] = false;
            }
        }
    }
    return (int)prime.size();
}
```
