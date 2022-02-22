---
title: String 切 Token
date: 2018-01-12 12:46:31
draft: false
tags: ["String"]
categories: [解題區, Template, 其他]
---
有兩種方法
1. 將 string 轉為 c 字串, 然後用 strtok
2. 利用 stringstream

第一種：
```cpp
#include <bits/stdc++.h>

using namespace std;

string s;

int main(){
    while(getline(cin, s)){
        char cstr[10000];
        strcpy(cstr, s.c_str());
        char *token = strtok(cstr, " ");
        while(token != NULL){
            printf("%s ", token);
            token = strtok(NULL, " ");
        }
        puts("");
    }
    return 0;
}
```

第二種:
```cpp
#include <bits/stdc++.h>

using namespace std;

string s;
stringstream ss;

int main(){
    while(getline(cin, s)){
        ss.str("");
        ss.clear();
        // 上面兩行是清空 stringstream, 兩行都必加
        ss << s;
        int n;
        while(ss >> n){
            cout << n << " ";
        }
        cout << endl;
    }
    return 0;
}
```

測試資料:
```
1 3 5 7 9
2 4 6
5 7 10 13
20 27
```