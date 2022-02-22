---
title: ITSA第五屆桂冠賽 挑戰組
date: 2017-05-01 09:41:49
draft: false
tags: ["ITSA"]
categories: [解題區, 其他]
---
目錄
===

# [A1] Extended Absolute Mode
[題目]()

## input

## 題解

## 作法

## AC code
```cpp
#include <cstdio>
#include <algorithm>
#include <vector>

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

vector <int> v;
int cnt[200];
int pre[200];

int main(){
    int TC;
    scanf("%d", &TC);
    while(TC--){
        int n, d;
        v.clear();
        memset(cnt, 0, sizeof(cnt));
        memset(pre, 0, sizeof(pre));
        scanf("%d %d", &n, &d);
        for(int i = 0; i < n; i++){
            int x;
            scanf("%d", &x);
            v.pb(x);
            cnt[x]++;
        }
        pre[0] = cnt[0];
        for(int i = 1; i <= 200; i++){
            pre[i] = pre[i - 1] + cnt[i];
        }
        int ub = -1, lb = INF;
        for(int i = 1; i <= 100; i++){
            if(cnt[i] == 0)
                continue;
            int u = i + d;
            int l = i - d;
            int num = pre[u] - pre[l - 1];
            if(num > n / 2){
                ub = max(ub, i);
                lb = min(lb, i);
            }
        }
        if(ub == -1 && lb == INF){
            puts("No");
        }
        else{
            printf("%d %d\n", lb, ub);
        }
    }
    return 0;
}
```
# [A3] Extended Absolute Mode
[題目]()

## input

## 題解

## 作法

## AC code
```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int main(){
    int TC;
    scanf("%d", &TC);
    while(TC--){
        ll n;
        scanf("%lld", &n);
        int need = 0;
        int M = 1;
        while(n != 1){
            need += n / 2;
            M++;
            n /= 2;
        }
        printf("%d %d\n", need, M);
    }
    return 0;
}
```
# [A4] Minimum Sum
[題目](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?a=15724)

## input

## 題解

## 作法

## AC code
```cpp
#include <cstdio>  
#include <algorithm>  
#include <vector>  

using namespace std;  

typedef long long ll;  

#define pb push_back  
#define sz(a) (int)a.size()  

int main(){  
    int TC;  
    scanf("%d", &TC);  
    while(TC--){  
        int n;  
        scanf("%d", &n);  
        vector <int> v;  
        for(int i = 0; i < n; i++){  
            int x;  
            scanf("%d", &x);  
            v.pb(x);  
        }  
        sort(v.begin(), v.end());  
        vector <int> v1, v2;  
        for(int i = 0; i < sz(v); i++){  
            if(v[i] != 0){  
                v1.pb(v[i]);  
                v.erase(v.begin() + i);  
                break;  
            }  
            if(i == sz(v) - 1 && v[i] == 0){  
                v1.pb(0);  
                v.pop_back();  
            }  
        }  
        for(int i = 0; i < sz(v); i++){  
            if(v[i] != 0){  
                v2.pb(v[i]);  
                v.erase(v.begin() + i);  
                break;  
            }  
            if(i == sz(v) - 1 && v[i] == 0){  
                v2.pb(0);  
                v.pop_back();  
            }  
        }  
        if(v1[0] != 0 && v2[0] != 0){  
            for(int i = 0; i < sz(v); i++){  
                if(i & 1){  
                    v2.pb(v[i]);  
                }  
                else  
                    v1.pb(v[i]);  
            }  
        }  
        else{  
            for(int i = 0; i < sz(v); i++){  
                v1.pb(v[i]);  
            }  
        }  
        reverse(v1.begin(), v1.end());  
        reverse(v2.begin(), v2.end());  
        int len = min(sz(v1), sz(v2));  
        if(sz(v1) > sz(v2)){  
            int carry = 0;  
            for(int i = 0; i < len; i++){  
                v1[i] = v1[i] + v2[i] + carry;  
                carry = v1[i] / 10;  
                v1[i] %= 10;  
            }  
            if(carry)  
                v1.pb(carry);  
            for(int i = sz(v1) - 1; i >= 0; i--){  
                printf("%d", v1[i]);  
            }  
            puts("");  
        }  
        else{  
            int carry = 0;  
            for(int i = 0; i < len; i++){  
                v2[i] = v2[i] + v1[i] + carry;  
                carry = v2[i] / 10;  
                v2[i] %= 10;  
            }  
            if(carry)  
                v2.pb(carry);  
            for(int i = sz(v2) - 1; i >= 0; i--){  
                printf("%d", v2[i]);  
            }  
            puts("");  
        }  
    }  
    return 0;  
}  
```
