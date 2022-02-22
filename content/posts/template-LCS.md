---
title: LCS 模板
date: 2017-03-16 10:34:32
draft: false
tags: ["Template"]
categories: [解題區, Template, DP]
---

定義 dp[i][j]: s1...si 和 t1...tj 的最長共同子序列
```cpp
string s1, s2;
int dp[100][100];

int main(){
    cin >> s1 >> s2;
    int len1 = s1.length();
    int len2 = s2.length();
    for(int i = 0; i < len1; i++){
        for(int j = 0; j < len2; j++){
            if(s1[i] == s2[j])
                dp[i + 1][j + 1] = dp[i][j] + 1;
            else
                dp[i + 1][j + 1] = max(dp[i + 1][j], dp[i][j + 1]);
        }
    }
    cout << dp[len1][len2] << endl;
    return 0;
}
```
