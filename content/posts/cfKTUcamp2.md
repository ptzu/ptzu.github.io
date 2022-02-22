---
title: KTUcamp2
date: 2016-07-26 22:46:21
draft: false
tags: ["Codeforces", "Binary Search"]
categories: [解題區, Codeforces]
---

# C. Rating Shuffle
\# binary_search
[題目](http://codeforces.com/gym/100738/problem/C)

## 題解
給 n 個人, 每個人都有評分(rating)
每場比賽每個人的評分可以上升或下降 d 分
求需要最少幾場可以使第 i 人, 評分第 i 大
也就是大到小

## 作法
可以觀察到愈多場次, 愈容易達成
所以可以用二分搜, 是否能使用 k 場來完成需求

想法： 先將第一人直接加到最大
接著看第二人需要幾場可以最接近第一人
但不超過他, 以此類推

需要注意的是奇偶數的判定
例如： 我們需要上升(或下降) 3 場, 而二分搜的場次為 4 場
但這樣不可能達成, 因為需要 +3 場, 則剩下那個一定是 -1 場
結果就為 +2 場

所以當有奇偶數場的差別時, 可以退而求其次, 不那麼接近 prev
也就是 need--

而判斷函式回傳 false 的條件: abs(need) > mid
可分為兩部份討論
1. 需要加上 +need 場數: 這種情況不影響, 因為可以不那麼接近, 而且會和 mid 取 min

2. 需要加上 -need 場數: 這種情況就有差別了, 因為不夠小會造成比前一個數還大的結果, 所以這種情況就會回傳 false

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef pair<int,int> pii;
typedef long long ll;
typedef vector <ll> vec;
typedef vector <vec> mat;

#define pb push_back
#define pf push_front
#define mp make_pair
#define sz(a) (int)a.size()
#define i128 __int128
#define INF 0x3f3f3f3f
#define st first
#define nd second
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX

int n, d;
int a[100000 + 10];

bool C(ll mid){
    ll prev = a[0] + d * mid;
    for(int i = 1; i < n; i++){
        ll need = min(mid, (prev - a[i]) / d);
        if(a[i] + d * need >= prev)
            need--;

        if(abs(need) % 2 == 0 && mid % 2 == 1)
            need--;
        else if(abs(need) % 2 == 1 && mid % 2 == 0)
            need--;

        if(abs(need) > mid)
            return false;
        prev = a[i] + d * need;
    }
    return true;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> d;
    for(int i = 0; i < n; i++)
        cin >> a[i];
    ll lb = 0, ub = 1e10;
    if(C(lb)){
        cout << lb << endl;
        return 0;
    }
    while(ub - lb > 1){
        ll mid = (lb + ub) / 2;
        if(C(mid))
            ub = mid;
        else
            lb = mid;
    }
    cout << ub << endl;
    return 0;
}
```
