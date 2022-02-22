---
title: KMP Algorithm 探討
date: 2016-07-28 13:06:36
draft: false
tags: ["String"]
categories: [學習札記, 演算法]
---
## 關於KMP
一般再搜尋字串時, 暴力搜尋將每個位置都搜尋一次
同時比對要搜尋的字串(pattern)
時間複雜度為O(nm)

KMP演算法改進了一些不必要的搜尋
使得時間複雜度為O(n + m)

## 名詞定義
失敗函數(failure function): 當比對失敗時, pattern需位移幾位才能成為前綴, 一般用在字串比對失敗時查找, 故稱失敗函數

## 建立 F 函數
這邊放上網路上常見兩種寫法
不變的是 j 代表的是目前比對的字元的 '前一個字'
f[i] 代表比對失敗時, j 要變成的位置
1. 字串從 0 開始存, 沒匹配字元時, f 值為 -1
```cpp
void getF(string s){
    f[0] = -1;
    for(int i = 1; i < s.length(); i++){
        int j = f[i - 1];
        //如果不相等, 先位移到適合位置
        while(j >= 0 && s[j + 1] != s[i])
            j = f[j];

        if(s[j + 1] == s[i])
            f[i] = j + 1;
        //還是不相等, 此時 j = -1
        else
            f[i] = j;
    }
}
```
2. 字串從 1 開始存, 沒匹配字元時, f 值為 0
```cpp
void getF(string s)  {    
   for (int i = 2; i <= s.length(); i++){  
        int j = f[i-1];  
        while (j && s[j+1] != s[i])
            j = f[j];  
        if (s[j + 1] == s[i])
            f[i] = j + 1;
        //這邊沒有 else 判斷是因為, 在全域宣告的陣列初始值為 0
        // 0 在這種寫法的意義和第一種寫法的 -1 相同

    }
}  
```

## KMP 搜尋
如果存在該字串, 回傳字串開頭位置, 否則回傳 -1
```cpp
int KMP(char S[], char pat[]){
  int lens = strlen(S);
  int lenp = strlen(pat);
  int i = 0, j = 0;
  while(i < lens && j < lenp)
  {
    if(S[i] == pat[j]){
      i++; j++;
    }
    else if(j == 0)
      i++;
    else
      j = failure[j-1] + 1;
  }
  return ((j == lenp) ? (i-lenp) : -1);
}
```
