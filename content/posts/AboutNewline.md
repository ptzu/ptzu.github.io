---
title: 關於 '\n' 的各種輸入
date: 2017-10-22 10:20:09
draft: false
tags: ["I/O"]
categories: [學習札記, 其他]
---

在 C/C++ 輸入字串的時候
輸入一個整數 n, 接著讀取 n 個字串
常常會發生下面這種問題

![](https://i.imgur.com/YWgrdh9.png)

咦！！我的第 5 行勒？？
不用懷疑, 絕對不是你的電腦壞掉, 而是你不夠了解程式內部的細節
事實上, 當我們每按一次 enter(換行) 的時候
他其實是輸入換行符號
在 Windows 上是 CR+LF, 在 Linux 上是 LF(也就是'\n'), 在 MAC OS 上是 CR
所以當我們進行輸入的時候, 資料會先被放在 buffer(暫存區)
而像 scanf 這些 input 函數會去 buffer 將資料讀取到記憶體
所以當 scanf 讀取一個數字時, 其實有個 '\n' 被遺留在 buffer
就像如此:
![](https://i.imgur.com/DlGHGYr.png)
而這個時候 fgets 讀取就會讀到 '\n', 理所當然的就少一行資料

以下就來分析各種 input 函數對於 '\n' 的處理方式

## scanf("%s")
遇到 '\n' 就中止, 不讀入 '\n'
![](https://i.imgur.com/d8WiCBJ.png)

## fgets
遇到 '\n' 就中止, 讀入 '\n', 會放到子串裡面
![](https://i.imgur.com/wJ12zwD.png)

## cin >> s
遇到 '\n' 就中止, 不讀入 '\n'
![](https://i.imgur.com/04j5HSA.png)

## getline
遇到 '\n' 就中止, 讀入 '\n', 不會放到子串裡面
![](https://i.imgur.com/2oFKXHD.png)

## scanf("%d\n")
其實 scanf 是一個匹配函數, 甚至好幾個 '\n' 都被處理掉了
![](https://i.imgur.com/UJxN1M2.png)

看完以上的例子, 應該會更清楚要怎麼處理這種 input 了！