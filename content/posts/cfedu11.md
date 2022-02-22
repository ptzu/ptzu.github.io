---
title: Educational Codeforces Round 11
date: 2016-07-24 22:12:59
draft: false
tags: ["Codeforces", "Binary Search"]
categories: [解題區, Codeforces]
---

# C. Hard Process
\# binary_search \# two points
[題目](http://codeforces.com/contest/660/problem/C)

## 題解
給定長度為 n 只有 0, 1 的數列<br>
現有 k 次機會可將 0 轉換成 1<br>
求可連續的 1 最長是多少<br>

## 作法
可利用二分搜, 假設長度 x 是否能在 k 次轉換得到<br>
由於觀察到長度愈長, 愈難達成<br>
有此單調性, 故可用二分搜<br>
是 1 1 1 1 0 0 0 0 類型<br>

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef pair<int,int> pii;
typedef long long ll;

#define pb push_back
#define pf push_front
#define mp make_pair
#define sz(a) (int)a.size()
#define i128 __int128
#define INF 0x3f3f3f3f
// LLONG_MIN LLONG_MaX INT_MIN INT_MaX

int n, k;
int a[303030];

bool C(int x, int &pos){
    int block_1 = 0;
    for(int i=0; i<x; i++){
        if(a[i] == 1)
            block_1++;
    }

    int need = x - block_1;
    for(int i=1; i + x - 1 < n; i++){
        if(a[i - 1] == 1)
            block_1--;
        if(a[i + x - 1] == 1)
            block_1++;

        if(x - block_1 < need){
            need = x - block_1;
            pos = i;
        }
    }
    return need <= k;
}

int binary_search(int &pos){
    //[0,n], 1 1 1 1 0 0 0 0 type
    int lb = 0;
    int ub = n;

    if(C(lb, pos) == 0)
        return 0;
    if(C(ub, pos) == 1)
        return n;

    while(ub - lb > 1){
        int mid = (lb + ub) / 2;
        if(C(mid, pos))
            lb = mid;
        else
            ub = mid;
    }

    //Make the pos in the right place
    C(lb, pos);

    return lb;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> k;
    for(int i=0; i<n; i++){
        cin >> a[i];
    }

    int pos = 0;
    int ans = binary_search(pos);

    for(int i=0; i<ans; i++){
        a[pos + i] = 1;
    }
    cout << ans << endl;
    for(int i=0; i<n; i++){
        cout << a[i] << " ";
    }
    cout << endl;
    return 0;
}
```
