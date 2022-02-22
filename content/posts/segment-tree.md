---
title: 資料結構 － Segment Tree
date: 2016-12-04 20:09:49
draft: false
tags: ["Segment Tree"]
categories: [學習札記, 資料結構]
---
# 用途
多筆區間查詢時, 如果有某點被更新, 則每次都要O(n)重新計算
這樣非常耗時, 因此有了線段樹這個資料結構

# 變數定義
N: input 個數
NN: 恰好大於 N 的 2次方
seg[]: 線段樹陣列

# 圖解
以 N = 7 為例子(不是2的指數次)
input 為 1, 2, 3 ,4, 5, 6, 7

# init
<img src = http://i.imgur.com/frKJ0OO.jpg width="50%" height="50%">
會先宣告出 2 * NN 大小的 seg[]
分為兩個區塊
上半部為區間值(大小為2, 4, 8...依此類推)
最下面那部份為長度 1 的區間, 也就是你的 input

#build (建構線段樹)
參數說明: idx 為 該區塊在 seg[]陣列中實際的index
<img src = http://i.imgur.com/3skpRi7.jpg width="50%" height="50%">
合併
<img src = http://i.imgur.com/LIGwzpj.jpg width="50%" height="50%">
建構完成！
