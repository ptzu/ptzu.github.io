---
title: Educational Codeforces Round 18
date: 2017-03-30 16:32:26
draft: false
tags: ["Codeforces", "Greedy", "DP"]
categories: [解題區, Codeforces]
---
目錄
===
1. 距離 sort
2. 使用 vector erase
3. 數字 digit 總於除 3

# A. New Bus Route
[題目](http://codeforces.com/contest/792/problem/A)

## input
n 個城市: 2 ≤ n ≤ 2·10^5
城市座標: - 10^9 ≤ a ≤ 10^9

## 題解
有 n 座城市, 要規劃新的公車路線
所以要求出任兩城市的最短距離, 以及符合該距離的 pair 有幾個？

## 作法
sort

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
    fio;
    int n;
    cin >> n;
    vector <int> v;
    for(int i = 0; i < n; i++){
        int x;
        cin >> x;
        v.pb(x);
    }
    sort(v.begin(), v.end());
    int ans = INT_MAX;
    for(int i = 0; i + 1 < n; i++){
        int dis = abs(v[i + 1] - v[i]);
        if(dis < ans)
            ans = dis;
    }
    int cnt = 0;
    for(int i = 0; i + 1 < n; i++){
        int dis = abs(v[i + 1] - v[i]);
        if(dis == ans)
            cnt++;
    }
    cout << ans << " " << cnt << endl;
    return 0;
}
```

# B. Counting-out Rhyme
[題目](http://codeforces.com/contest/792/problem/B)

## input
n 個人, k 回合: 2 ≤ n ≤ 100, 1 ≤ k ≤ n - 1
數 a 個人: 1 ≤ ai ≤ 10^9

## 題解
n 個小孩圍成一圈, 每個人編號 1 ~ n
最一開始編號 1 當作 leader, 這個遊戲持續 k 回合
第 i 步驟由 leader 從他起始數 ai 個人, 將那個人剔除遊戲
並由被剔除的下一人當作 leader
輸出每一回合被剔除的人

## 作法
會用 vector 的 erase 就簡單實作

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

vector <int>v;
int a[1000];

int main(){
    int n, k;
    scanf("%d %d", &n, &k);
    for(int i = 0; i < k; i++){
        scanf("%d", &a[i]);
    }
    for(int i = 1; i <= n; i++)
        v.pb(i);
    int leader = 0;
    for(int i = 0; i < k; i++){
        int num = a[i];
        leader += a[i];
        leader %= (n - i);
        printf("%d ", v[leader]);
        v.erase(v.begin() + leader);
    }
    puts("");
    return 0;
}
```

# C. Divide by Three
\# greedy \# DP
[題目](http://codeforces.com/contest/792/problem/C)

## input
數字 n: 1 ≤ n < 10^100000

## 題解
數字每個 digit 的總和必須要被 3 整除
我們可以移除一些 digit 使達到條件
數字不能 leading zero
求移除最少 digit 的數字, 如果不能達到則輸出 -1

## 作法
只會有兩種情況
1. 餘數為 1: 移除 1, 4, 7 皆可, 或是移除 2, 5, 8 任兩個
2. 餘數為 2: 移除 2, 5, 8 皆可, 或是移除 1, 4, 7 任兩個
---
我們只要分別將移除 1 個數字, 移除 2 個數字的答案求出
然後輸出答案最長的那個(不一定能移除)

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

string s;
vector <string> v;

string tran(string st){
    int now = 0;
    while(now < st.length() - 1 && st[now] == '0')
        now++;
    return st.substr(now);
}

int main(){
    cin >> s;
    int sum = 0;
    for(int i = 0; i < s.length(); i++)
        sum += s[i] - '0';
    sum %= 3;
    if(sum != 0){
        for(int i = s.length() - 1; i >= 0; i--){
            if((s[i] - '0') % 3 == sum){
                string tmp = s.substr(0, i) + s.substr(i + 1);
                tmp = tran(tmp);
                v.pb(tmp);
                break;
            }
        }
        int first = -1;
        for(int i = s.length() - 1; i >= 0; i--){
            if((s[i] - '0') % 3 == 0 || (s[i] - '0') % 3 == sum)
                continue;
            if(first == -1){
                first = i;
                continue;
            }
            string tmp = s.substr(0, i) + s.substr(i + 1, first - 1 - i) + s.substr(first + 1);
            tmp = tran(tmp);
            v.pb(tmp);
            break;
        }
    }
    else{
        v.pb(s);
    }
    string ans = "";
    for(string i : v){
        if(i.length() > ans.length())
            ans = i;
    }
    if(ans.empty()) puts("-1");
    else cout << ans << endl;
    return 0;
}```
