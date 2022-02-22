---
title: 工作排程問題統整
date: 2018-04-13 20:58:24
draft: false
tags: ["Others"]
categories: [解題區, 其他]
---


# Job Scheduling 1
[題目](https://practice.geeksforgeeks.org/problems/job-sequencing-problem/0)

## input
第一列輸入 T, 代表 T cases
每筆測資開頭輸入 n (1 <= N <= 100)
代表 n 個工作
每個工作輸入 id, deadline, profit

## 題解
有 n 個工作
每個工作花費 1 單位時間完成
只要在 deadline 前完成就可以
求最大的 profit 為多少

## 作法
紀錄最大的 deadline
profit 大到小排序後
每次從 max deadline 往前掃, 若有空間就放
時間複雜度: {%math%}O(n^2){%endmath%}

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

int TC, n;
pii a[1000];
bool has[1000];

int main(){
    scanf("%d", &TC);
    while(TC--){
        memset(has, 0, sizeof(has));
        scanf("%d", &n);
        int mx = -1;
        for(int i = 0; i < n; i++){
            int id;
            scanf("%d%d%d", &id, &a[i].nd, &a[i].st);
            mx = max(mx, a[i].nd);
        }
        sort(a, a + n, greater<pii>());
        int ans = 0, cnt = 0;
        int flag = mx;
        for(int i = 0; i < n; i++){
            for(int j = flag; j >= 1; j--){
                if(!has[j] && j <= a[i].nd){
                    cnt++;
                    ans += a[i].st;
                    has[j] = true;
                    break;
                }
            }
        }
        printf("%d %d\n", cnt, ans);
    }
    return 0;
}
```

# Job Scheduling 2
[題目](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=12&problem=967)

## input
第一列輸入 T, 代表 T cases
每筆測資開頭輸入 n (1 <= N <= 1000)
代表 n 個工作
每個工作輸入需要花費的時間及懲罰

## 題解
有 n 個工作
每個工作若一天沒完成, 就要付相對應的罰金
求工作的順序使得罰金最少
若相同則輸出字典序最小

## 作法
罰金/天數去排序
從大排到小

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

struct node{
    double v;
    int id;
    bool operator <(const node& a)const{
        if(v == a.v) return id < a.id;
        return v > a.v;
    }
};

node a[1010];

int main(){
    int TC;
    scanf("%d", &TC);
    while(TC--){
        int n;
        scanf("%d", &n);
        for(int i = 0; i < n; i++){
            int d, p;
            scanf("%d%d", &d, &p);
            a[i] = (node){1.0 * p / d, i + 1};
        }
        sort(a, a + n);
        for(int i = 0; i < n; i++){
            if(i) printf(" ");
            printf("%d", a[i].id);
        }
        puts("");
        if(TC != 0) puts("");
    }
    return 0;
}
```

# Job Scheduling 3
[TIOJ 1072]()

有 n 人吃飯, 每人吃的速度不同
如何使飯局結束時間最早

# Job Scheduling 4
[Uva 11269]()
有 n 道題目
第一個人做完之後才能給第二個人做
問最少花多少時間

