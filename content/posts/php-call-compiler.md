---
title: 在 PHP 調用 Compiler
date: 2017-06-04 17:24:29
draft: false
tags: ["PHP"]
categories: [學習札記, 網頁設計]
---
今天在做 Online Judge 時
來到了調用 Compiler 來編譯的步驟
在 PHP 方面有三個函數可以選擇
分別是 system(), exec(), shell_exec()
詳細差異就請自行去搜尋吧

先講結論: 不需要 sudo 權限就可以執行 g++

一開始我以為呼叫 g++ 需要 sudo權限
於是去找了怎麼使 apache 有權限
可以呼叫
```php
System('id');
```
來查看 apache 是用什麼身份
我的是 www-data 這個帳號
接著打 visudo 來編輯 sudoer file, 不建議直接修改 sudoer file
因為需要更改到 sudoer file 的權限, 改來改去可能會有權限上的問題
我在檔案內加了這行
```
www-data    ALL(ALL:ALL)    NOPASSWD:ALL
```
其中 NOPASSWD 是呼叫 sudo 時不用打密碼
本來以為終於要可以執行了
結果跳出了訊息 sudo: no tty present and no askpass program specified
雖然在找資料過程已經知道
apache 沒有 tty, 所以會有這個問題
但網路上沒有一個方法可以解決阿！
網路上幾乎都是提到
1.在sudoer file 把 Defaluts requiretty 註解掉(可是我根本沒有這一行= =)
2.添加 Defaluts:www-data !requiretty(同樣無效)
甚至我把 www-data 從 sudoer file 拿掉, 還是出現這個訊息

眼看無望, 已經從早上弄到下午
只好求助學長
結果學長說: 應該不用 sudo 權限吧
...
... what?
真的不用欸QAQ, 因為我前面都是用 mkdir 的之類的指令再試
結果 g++ 不需要= =
最後在編譯的時候, 又出現一個訊息
```
usr/bin/ld: cannot open output file code/t.out: Permission denied collect2: error: ld returned 1 exit status 1
```
這個問題就好解決了, 就是輸出的資料夾權限不夠
把該資料夾權限改為 777 就可以了
![](http://i.imgur.com/WVjMkaf.png)

參考資料:
1.https://blog.allenchou.cc/php-compile-and-run-cpp/
2.http://emn178.pixnet.net/blog/post/72463845-php%E5%9F%B7%E8%A1%8C%E5%A4%96%E9%83%A8%E7%A8%8B%E5%BC%8F
3.http://ibookmen.blogspot.tw/2010/11/unix-2.html
4.http://www.jikker.net/2011/01/phprootcommand-line.html
5.http://www.arthurtoday.com/2010/03/sudoer.html
6.http://blog.xuite.net/chingwei/blog/24939464-%E3%80%90%EF%BC%AC%E3%80%91SUSE+%3A+%E7%B0%A1%E5%96%AE%E7%9A%84sudo%E8%A8%AD%E5%AE%9A
