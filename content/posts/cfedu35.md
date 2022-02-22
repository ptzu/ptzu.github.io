---
title: Educational Codeforces Round 35
date: 2018-02-22 20:48:46
draft: false
tags: ["Codeforces", "Brute Force", "Math"]
categories: [解題區, Codeforces]
---
目錄
===
1. 最小數字最近距離
2. 裝蛋糕使最少塊最多
3. 燈泡週期性
4. 逆序數對奇偶性

# A. Nearest Minimums
[題目](http://codeforces.com/contest/911/problem/A)

## input
輸入 N (2 ≤ n ≤ 10^5), 代表 array 大小
接著輸入 N 個值

## 題解
找最小值之間最近的距離

## 作法
紀錄好值和 index, 排序後去找

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

pii p[101010];
int n;

int main(){
    scanf("%d", &n);
    for(int i = 0; i < n; i++){
        int x;
        scanf("%d", &x);
        p[i].st = x;
        p[i].nd = i;
    }
    sort(p, p + n);
    int mn = p[0].st;
    int ans = INF;
    for(int i = 0; i + 1< n;i++){
        if(p[i].st == mn && p[i + 1].st == mn){
            ans = min(ans, abs(p[i].nd - p[i + 1].nd));
        }
        else break;
    }
    printf("%d\n", ans);
    return 0;
}
```

# B. Two Cakes
[題目](http://codeforces.com/contest/911/problem/B )

## input
輸入 n, a, b (1 ≤ a, b ≤ 100, 2 ≤ n ≤ a + b)
代表 n 個盤子, 以及切成 a 塊和 b 塊

## 題解
現在有兩個蛋糕, 分別切成 a 塊和 b 塊
並有 n 個盤子
Ivan 希望每塊蛋糕都能放上盤子, 每個盤子至少一塊
而且每個盤子只能放同種蛋糕
這些盤子當中一定有放最少塊的
目標要使最少塊的最多, 請問是多少？

## 作法
列舉不同盤子數量分給兩種即可

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

int main(){
    int n, a, b;
    scanf("%d%d%d", &n, &a, &b);
    int ans = -1;
    for(int num = 1; num <= n - 1; num++){
        // 至少一盤給b
        int mna = a / num;
        int mnb = b / (n - num);
        int mn = min(mna, mnb);
        ans = max(ans, mn);
    }
    printf("%d\n", ans);
    return 0;
}
```

# C. Three Garlands
[題目](http://codeforces.com/contest/911/problem/C)

## input
輸入 k1, k2, k3 (1 ≤ k ≤ 1500)
代表亮的周期

## 題解
Mishka 正在裝飾聖誕樹, 他有3個花環要放到樹上並使他們發亮
3個花環亮的周期分別是 k1, k2, k3
問是否能找到一個起始時間 x1, x2, x3
使得每秒至少都有一個花環亮

## 作法
先將 k 排序好
用類似 greedy 的想法
若當前位置沒亮, 就擺一個花環在那個時候亮
如果需要超過 3 個, 答案就是 NO

另外有個 O(1) 的解法
1. 當 k 有一個為 1
2. 當 k 有至少兩個 2
3. 當 k 全為 3
4. 當 k = {2, 4, 4}

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
const int mod = 1e9 + 7;

int k[3];
int pos[3];

int main(){
    scanf("%d%d%d", &k[0], &k[1], &k[2]);
    sort(k, k + 3);
    int idx = 0;
    for(int i = 0; i <= 10000; i++){
        bool ok = false;
        for(int j = 0; j < 3; j++){
            if((i - pos[j]) % k[j] == 0) ok = true;
        }
        if(!ok){
            if(idx == 3){
                puts("NO");
                return 0;
            }
            pos[idx++] = i;
        }
    }
    puts("YES");
    return 0;
}
```

## AC code 2
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
const int mod = 1e9 + 7;

int k[3];
int pos[3];

int main(){
    scanf("%d%d%d", &k[0], &k[1], &k[2]);
    sort(k, k + 3);
    if(k[0] == 1 || (k[0] == 2 && k[1] == 2) ||
       (k[0] == 3 && k[1] == 3 && k[2] == 3) || (k[0] == 2 && k[1] == 4 && k[2] == 4)){
           puts("YES");
       }
    else{
        puts("NO");
    }
    return 0;
}
```

# D. Inversion Counting
[題目](http://codeforces.com/contest/911/problem/D)

## input
輸入 n (1 ≤ n ≤ 1500)
代表數列大小, 接著輸入 n 個數
輸入 m (1 ≤ m ≤ 2·10^5)
代表 m 個 query
接著輸入 m 個 query: l r

## 題解
目前有一數列
想求他的逆序數對
但有 m 個 query
每個 query 輸入 l, r
代表將 [l, r] 翻轉
求翻轉後的逆序數對
若為奇數輸出 odd, 反之輸出 even

## 作法
本來以為真的要翻轉再求
沒想到這題是數學阿！
詳情請見[置換的奇偶性](https://en.wikipedia.org/wiki/Parity_of_a_permutation)

可以先 O(n^2) 求出奇偶
reverse 可以視為很多個 swap
每次 swap 就會改變奇偶性
因此只要求出幾次 swap 即可

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
const int mod = 1e9 + 7;

int a[2000];

int main(){
    int n, m;
    scanf("%d", &n);
    for(int i = 0; i < n; i++) scanf("%d", &a[i]);
    int parity = 0;
    for(int i = 0; i < n; i++){
        for(int j = i + 1; j < n; j++){
            if(a[i] > a[j]) parity ^= 1;
        }
    }
    scanf("%d", &m);
    while(m--){
        int l, r;
        scanf("%d%d", &l, &r);
        int num = r - l + 1;
        num = num * (num - 1) / 2;
        if(num & 1) parity ^= 1;
        if(parity) puts("odd");
        else puts("even");
    }
    return 0;
}
```
