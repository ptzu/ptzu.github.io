---
title: BSGS Algorithm
date: 2018-01-03 23:08:32
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---

求離散對數
時間複雜度: O({%math%}\sqrt {n}{%endmath%})

## Code
```cpp

map <ll, int> mp; //value, idx

int fastpow(ll a, ll b){
    ll base = a, ans = 1;
    while(b){
        if(b & 1) ans = ans * base % P;
        base = base * base % P;
        b >>= 1;
    }
    return ans;
}

int BSGS(int a, int b, int P){
    mp.clear();
    int m = ceil(sqrt(P));
    ll value = b % P; // j = 0, value = b
    mp[value] = 0;
    for(int j = 1; j <= m; j++){
        value = value * a % P; // a^j * b
        mp[value] = j;
    }
    ll t = fastpow(a, m, P); // a ^ m
    value = 1;
    for(int i = 1; i <= m; i++){
        value = value * t % P; // a ^ (i*m)
        if(mp.find(value) != mp.end()){ // 找到一樣的值
            int ans = i*m - mp[value];
            return ans;
        }
    }
    return -1;
}
```
