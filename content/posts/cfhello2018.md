---
title: Codeforces Hello 2018
date: 2018-01-13 15:31:50
draft: false
tags: ["Codeforces", "Bit Operation"]
categories: [解題區, Codeforces]
---
目錄
===
1. 取模
2. 判斷樹的孩子數量
3. 檸檬汁購買

# A. Modular Exponentiation
[題目](http://codeforces.com/contest/913/problem/A)

## input
輸入兩個正整數 n, m, 1 <= n,m <= 10^8

## 題解
求 m mod 2^n

## 作法
實作

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;
typedef pair <int,int> pii;
typedef long long ll;

#define pb push_back
#define sz(a) (int)a.size()
#define INF 0x3f3f3f3f
#define st first
#define nd second
#define fio ios::sync_with_stdio(false), cin.tie(0)
#define DEBUG(x) printf("Here: %d\n", x), exit(0);
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX
const int MAX_V = 1010;

int main(){
    int n, m;
    scanf("%d%d", &n, &m);
    int mod = 1;
    if(n >= 32){
        printf("%d\n", m);
    }
    else{
        for(int i = 1; i <= n; i++)
            mod <<= 1;
        printf("%d\n", m % mod);
    }
    return 0;
}
```

# B. Christmas Spruce
[題目](http://codeforces.com/contest/913/problem/B)

## input
輸入一正整數 n, 3 <= n <= 1000
代表這個樹有幾個節點
接下來 n - 1 行, 輸入一正整數 p
代表他是第 i + 1 節點的父節點

## 題解
我們稱有根樹為 spruce, 當他所有的非葉節點有至少 3 個子節點
給一顆樹, 判斷是否為 spruce

## 作法
模擬就好

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;
typedef pair <int,int> pii;
typedef long long ll;

#define pb push_back
#define sz(a) (int)a.size()
#define INF 0x3f3f3f3f
#define st first
#define nd second
#define fio ios::sync_with_stdio(false), cin.tie(0)
#define DEBUG(x) printf("Here: %d\n", x), exit(0);
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX
const int MAX_V = 1010;

vector <int> g[MAX_V];

int main(){
    int n;
    scanf("%d", &n);
    for(int i = 2; i <= n; i++){
        int x;
        scanf("%d", &x);
        g[x].pb(i);
    }
    for(int i = 1; i <= n; i++){
        if(sz(g[i]) != 0){
            int cnt = 0;
            for(int j = 0; j < sz(g[i]); j++){
                int v = g[i][j];
                if(sz(g[v]) == 0) cnt++;
            }
            if(cnt < 3){
                puts("No");
                return 0;
            }
        }
        else if(sz(g[i]) == 0) continue;
    }
    puts("Yes");
    return 0;
}
```

# C. Party Lemonade
\# Bit operation
[題目](http://codeforces.com/contest/913/problem/C)

## input
第一列輸入兩個正整數 n, L, 1 <= n <= 30, 1 <= L <= 10^9
分別代表有 n 種瓶子, 和所需要的檸檬汁量
接著輸入每種瓶子的價錢

## 題解
新年派對需要檸檬汁(?
商店有賣 n 種瓶裝的檸檬汁, 價錢和容量不一樣
第 i 種瓶子的容量為 2^(i - 1)
你現在想要買至少 L 公升的檸檬汁, 最少需要花多少錢？

## 作法
首先我們可以注意到, 每種瓶裝的容量都是前一種的兩倍
所以假如第 i 種的價錢的兩倍小於第 i + 1 種的價錢: {%math%} 2 * p_{i} < p_{i+1} {%endmath%}
那我們就可以用第 i 種來取代 i + 1 種
接著我們將需求量 L, 表達成二進位
就可以很清楚的知道需要哪些容量的瓶子
而現在處理過後的價格必定是 {%math%} 2 * p_{i} <= p_{i+1} {%endmath%}
所以我們從高 bit 往下處理, 因為低 bit 的價錢去買高的一定不划算
要注意的是, L 的 bit 數可能比商店的 bit 數多(種類)
所以要考慮到該 bit 需要的數量(也只有第 n 種可能大於 1)

還有一個特殊情況是, 我們買超過 L 的數量
也就是針對某種 type i 買了兩個
和上面那種不一樣, 上面那種買超過一個是為了去填滿高位的 bit
而現在是為了填滿低位的 bit
例如說現在有 bit 0, 1, 2 這些要買
但是在買 bit 5 的時候可能比 bit 0, 1, 2 的價錢加起來還便宜
很顯然的 bit 5 的容量可以足夠填滿 bit 0, 1, 2
那我們就多買一個 bit 5 就好了, 比較多又比較便宜


## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;
typedef pair <int,int> pii;
typedef long long ll;

#define pb push_back
#define sz(a) (int)a.size()
#define INF 0x3f3f3f3f
#define st first
#define nd second
#define fio ios::sync_with_stdio(false), cin.tie(0)
#define DEBUG(x) printf("Here: %d\n", x), exit(0);
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX
const int MAX_V = 1010;

ll c[50];
int n;
ll L;

int main(){
    scanf("%d%lld", &n, &L);
    for(int i = 0; i < n; i++){
        scanf("%lld", &c[i]);
    }
    for(int i = 1; i < n; i++){
        c[i] = min(c[i], c[i - 1] * 2);
    }
    ll ans = LLONG_MAX, sum = 0;
    for(int i = n - 1; i >= 0; i--){
        ll need = L / (1 << i); // 買幾個
        sum += c[i] * need;
        L -= need << i;
        // 看是否需要多買一個, 如果 L == 0 就不需要
        ans = min(ans, sum + (L > 0) * c[i]);
    }
    printf("%lld\n", ans);
    return 0;
}
```

另一個版本

## AC code
```cpp
#include <bits/stdc++.h>

using namespace std;
typedef pair <int,int> pii;
typedef long long ll;

#define pb push_back
#define sz(a) (int)a.size()
#define INF 0x3f3f3f3f
#define st first
#define nd second
#define fio ios::sync_with_stdio(false), cin.tie(0)
#define DEBUG(x) printf("Here: %d\n", x), exit(0);
// LLONG_MIN LLONG_MAX INT_MIN INT_MAX
const int MAX_V = 1010;

ll c[50];
int n;
ll L;

int main(){
    scanf("%d%lld", &n, &L);
    for(int i = 0; i < n; i++){
        scanf("%lld", &c[i]);
    }
    for(int i = 1; i < n; i++){
        if(c[i - 1] * 2 < c[i]) c[i] = c[i - 1] * 2;
    }
    for(int i = n; i < 32; i++) c[i] = c[i - 1] * 2;
    ll ans = LLONG_MAX, sum = 0;
    for(int i = 31; i >= 0; i--){
        if((L >> i) & 1){ // 該 bit 是 1 就買
            sum += c[i];
            L &= ~(1 << i); // 把那個 bit 轉為 0
        }
        // 看是否需要多買一個, 如果 L == 0 就不需要
        ans = min(ans, sum + (L > 0) * c[i]);
    }
    printf("%lld\n", ans);
    return 0;
}
```
