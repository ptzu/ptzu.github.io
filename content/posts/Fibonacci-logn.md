---
title: 以 Log(n) 求取 Fibonacci 數列
date: 2017-10-09 08:35:53
draft: false
tags: ["Fast Pow"]
categories:
---
#
[題目]()

## input

## 題解

## 作法

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

int n, MOD, a, b, c;
typedef vector<ll> vec;
typedef vector<vec> mat;

mat A, B;

void printMatrix(mat& a){
    for(int i = 0; i < int(a.size()); i++){
        for(int j = 0; j < int(a.size()); j++){
            printf("%lld ", a[i][j]);
        }
        puts("");
    }
}

mat mul(const mat& a, const mat& b) {
    mat res(a.size(), vec(b[0].size(), 0));
    for (int r = 0; r < int(a.size()); r++)
        for (int c = 0; c < int(b[0].size()); c++)
            for (int k = 0; k < int(b.size()); k++)
                res[r][c] = (res[r][c] + a[r][k] * b[k][c] % MOD) % MOD;

    return res;
}

mat fastpow(const mat& a, int b) {
    mat ans(a.size(), vec(a.size(), 0));
    for (int i = 0; i < int(a.size()); i++)
        ans[i][i] = 1;

    mat base = a;
    while (b) {
        if (b & 1) ans = mul(ans, base);
        base = mul(base, base);
        b >>= 1;
    }
    return ans;
}

int solve(int n){
    if(n == 1 || n == 2 || n == 3)
        return 1;
    mat ans = fastpow(B, n - 3);
    ans = mul(A, ans);

    return ans[0][0];
}

int main(){
    // fio;
    // #ifdef DEBUG
    // printf ("TEST\n");
    // #else
    // printf("FAIL\n");
    // #endif
    while(scanf("%d%d%d%d%d", &n, &MOD, &a, &b, &c) != EOF){
        A.clear();
        B.clear();

        int tmp[][3] = {{1, 1, 1}, {0, 0, 0}, {0, 0, 0}};
        int tmp2[][3] = {{a, 1, 0}, {b, 0, 1}, {c, 0, 0}};

        for(int i = 0; i < 3; i++){
            A.pb(vec(tmp[i], tmp[i] + 3));
            B.pb(vec(tmp2[i], tmp2[i] + 3));
        }
        printf("%d\n", solve(n));
    }
    return 0;
}
```
