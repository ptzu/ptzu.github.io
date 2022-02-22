---
title: 線段樹 Segment Tree
date: 2018-01-24 10:14:23
draft: false
tags: ["Template"]
categories: [解題區, Template, Data Structure]
---
可做到:
1. 範圍更新
2. 範圍查詢
---

主要有兩種版本
一種是直接開 input 大小, 另一種是開滿 2 的倍數
陣列大小一定要開到 input 的 4 倍
原因是線段樹本身就需要 2 * N 的空間
樹高會是 {%math%}\left \lceil log_{2}N \right \rceil{%endmath%}
所以可能會多一層 (不管有沒有補滿 2 的倍數都會)

經過測試
update 的時候一定要 push 再 pull
query 可以只做 push 就好

## Code
這種寫法是直接開 input 大小
1-indexed, 閉區間 [l, r]
在時間上和空間上都比較優秀
缺點是要 trace 的時候不太好做

```cpp
#define L(x) (x<<1)
#define R(x) (1+(x<<1))
#define mid ((l+r)>>1)
const int MAX_N = 101010;

int seg[MAX_N * 4], lazy[MAX_N * 4];
int a[MAX_N];

void seg_build(int idx, int l, int r){
    if(l == r){
        seg[idx] = a[l];
        return ;
    }
    seg_build(L(idx), l, mid);
    seg_build(R(idx), mid + 1, r);
    seg[idx] = seg[L(idx)] + seg[R(idx)];
}

void seg_push(int idx, int l, int r){
    if(lazy[idx]){
        seg[L(idx)] += lazy[idx] * (mid - l + 1);
        seg[R(idx)] += lazy[idx] * (r - (mid + 1) + 1);
        lazy[L(idx)] += lazy[idx];
        lazy[R(idx)] += lazy[idx];
        lazy[idx] = 0;
    }
}

void seg_update(int a, int b, int x, int idx, int l, int r){
    if(r < a || l > b) return ;
    if(a <= l && r <= b){
        seg[idx] += (r - l + 1) * x;
        lazy[idx] += x;
        return ;
    }
    seg_push(idx, l, r);
    seg_update(a, b, x, L(idx), l, mid);
    seg_update(a, b, x, R(idx), mid + 1, r);
    seg[idx] = seg[L(idx)] + seg[R(idx)];
}

int seg_query(int a, int b, int idx, int l, int r){
    if(r < a || l > b) return 0;
    if(a <= l && r <= b) return seg[idx];
    seg_push(idx, l, r);
    return seg_query(a, b, L(idx), l, mid) + seg_query(a, b, R(idx), mid + 1, r);
}
```

## Code
這是開滿 2 的倍數版本
0-indexed, 開區間 [l, r)

```cpp
const int MAX_N = ...;

int seg[MAX_N * 4];
int lazy[MAX_N * 4];
int SN;

void seg_init(int N){
    SN = 1;
    while(SN < N) SN <<= 1;
    memset(seg, 0, sizeof(seg));
    memset(lazy, 0, sizeof(lazy));
}

void seg_pull(int idx){
    seg[idx] = seg[idx * 2 + 1] + seg[idx * 2 + 2];
}

void seg_push(int idx, int l, int m, int r){
    if(lazy[idx]){
        seg[idx * 2 + 1] += lazy[idx] * (m - l);
        seg[idx * 2 + 2] += lazy[idx] * (r - m);
        lazy[idx * 2 + 1] += lazy[idx];
        lazy[idx * 2 + 2] += lazy[idx];
        lazy[idx] = 0;
    }
}

void seg_update(int a, int b, int x, int idx, int l, int r){
    if(r <= a || l >= b) return ;
    if(a <= l && r <= b){
        seg[idx] += x * (r - l);
        lazy[idx] += x;
        return ;
    }

    int m = (l + r) / 2;
    seg_push(idx, l, m, r);
    seg_update(a, b, x, idx * 2 + 1, l, m);
    seg_update(a, b, x, idx * 2 + 2, m, r);
    seg_pull(idx);
}

int seg_query(int a, int b, int idx, int l, int r){
    if(r <= a || l >= b) return 0;
    if(a <= l && r <= b) return seg[idx];

    int m = (l + r) / 2;
    seg_push(idx, l, m, r);
    int ans = 0;
    ans += seg_query(a, b, idx * 2 + 1, l, m);
    ans += seg_query(a, b, idx * 2 + 2, m, r);
    seg_pull(idx);
    return ans;
}
```

## 例題
1. POJ 3468
