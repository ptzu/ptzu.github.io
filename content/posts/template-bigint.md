---
title: 大數運算
date: 2017-10-24 23:31:43
draft: false
tags: ["Template"]
categories: [解題區, Template, 其他]
---
第三次寫大數了
在比賽裡面因為 JAVA 有內建大數
所以此類題目比較少出
不過刷題的時候還是會遇到

```cpp
struct big{
    vector <int> digit;

    big(){

    }

    big(string s){
        int len = s.length();
        for(int i = 0; len - 1 - i >= 0; i++){
            digit.push_back(s[len - 1 - i] - '0');
        }
    }

    big operator +(const big& obj)const{
        int carry = 0;
        int maxSize = max(digit.size(), obj.digit.size()), minSize = min(digit.size(), obj.digit.size());
        big res;
        for(int i = 0; i < minSize; i++){
            int num = digit[i] + obj.digit[i] + carry;
            carry = num / 10;
            num %= 10;
            res.digit.push_back(num);
        }
        for(int i = minSize; i < maxSize; i++){
            int num;
            if(digit.size() > minSize)
                num = digit[i] + carry;
            else
                num = obj.digit[i] + carry;
            carry = num / 10;
            num %= 10;
            res.digit.push_back(num);
        }
        if(carry){
            res.digit.push_back(carry);
            carry = 0;
        }
        return res;
    }

    void print(){
        for(int i = (int)digit.size() - 1; i >= 0; i--){
            printf("%d", digit[i]);
        }
        puts("");
    }
};
```
## 簡單版乘除
有押四位
index 小存低位

```cpp
#define base 10000

void mul(vector<int> &a, int num){
    int carry = 0;
    for(int i = 0; i < sz(a); i++){
        carry += a[i] * num;
        a[i] = carry % base;
        carry /= base;
    }
    if(carry) a.pb(carry);
}

void divide(vector<int> &a, int num){
    int borrow = 0;
    for(int i = sz(a) - 1; i >= 0; i--){
        borrow = borrow * base + a[i];
        a[i] = borrow / num;
        borrow %= num;
        if(sz(a) != 1 && a[i] == 0) a.pop_back();
    }
}
```
