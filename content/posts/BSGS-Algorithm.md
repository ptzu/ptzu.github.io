---
title: BSGS 演算法
date: 2017-10-28 13:25:28
draft: false
tags: ["Math"]
categories: [學習札記, 數論]
---

BSGS 原名Baby Steps Giant Steps，又名大小步算法
用途是用來解**離散對數**, {% math %} a^{x}\equiv b\left( mod\ p\right) {% endmath %}
求最小 x 的解

步驟為
1. 令 {% math %} x=im-j {% endmath %}
2. {% math %} m = \left \lceil \sqrt{p} \right \rceil {% endmath %}
3. {% math %} a^{im-j} \equiv b(mod\ p) {% endmath %} 移項 {% math %} a^{im} \equiv a^{j}b(mod\ p) {% endmath %}
4. 對於 {% math %} a^{j}b {% endmath %} 列舉 0 ~ m 存起來
再對於 {% math %} a^{im} {% endmath %} 列舉 1 ~ m
5. 對於第一個符合方程式的 i, j 即為所求, 代回去可得到 x

## 問題與證明
1. 何時無解？
有解必須滿足費馬小定理, p 為質數, 且 gcd(a, p) = 1

2. 為何只要列舉到 {% math %} m = \left \lceil \sqrt{p} \right \rceil {% endmath %} ？
{% math %} x = im - j, i\in \left [ 1, m \right ], j\in \left [ 0, m \right ]{% endmath %} 當我們限制 i, j 在這個範圍時, x <= p, 那要怎麼保證這個範圍內有解
根據費馬小定理的一個結論, 得到 {% math %} a^{k\ mod\ p-1} \equiv a^{k}(mod\ p) {% endmath%}
所以 {% math %} x\in \left [ 0, p-1 \right ] {% endmath %} 就會和 {% math %} a^{x} {% endmath %} 同解

3. 為何第一個 i 列舉到的 im - j 為最小？
首先不管是 i 還是 j, 計算出來的 value 都有可能重複
要保證 im - j 最小, j 就必須最大
從 j 先開始列舉, 後面覆蓋的 j, 一定會比較大
而我們想要 i 最小, 所以自然不能先列舉 i

4. 為何 i 要從 [1, m] 開始？
因為 i 為 0 可能導致負數

參考：
1. http://mky.moe/posts/2017-02-25-bsgs/
2. http://blog.csdn.net/clove_unique/article/details/50740412
