---
title: MySQL CSV檔匯入 Invalid uft8 character
date: 2017-03-12 23:22:42
draft: false
tags: ["MySQL"]
categories: [學習札記, 資料庫]
---
環境: Windows
今天在匯入 csv 檔到資料表時
指令:
```MySQL
load data local infile 'D:\\HW1_excel\\book.csv' into table mytable
fields terminated by ',' lines terminated by '\r\n';
```
跳出了錯誤: Invalid uft8 character string: "
查了發現是編碼問題
請先確認欄位的編碼和文件編碼是否相同
欄位查詢可以使用:
```MySQL
show full fields from TABLE名稱
```

文件部份:
可以使用 nodepad++ 『Encoding』-> 『Convert to UTF-8』
小心不要像我腦殘選成 Encode in UTF-8, 這樣是不會轉換的...

如果已經確定編碼都相同
我還遇到一個問題: 1366 incorrect integer value
除了是你資料和欄位型態不同之外
還有可能是你的編碼含BOM, 所以同上改變編碼(不要選有BOM的喔！)

ps: 如果要讀txt檔, 欄位資料之間用TAB隔開

4/26 更新：
在MySQL中, 預設的資料分隔是 TAB, 換行則是 '\n'
而在 Windows 中, 換行符號則是 '\r\n'
如果沒有 lines terminated by '\r\n' 這段, '\r'會被保留下來
導致可能資料看起來明明是對的, 但其實在結尾多了 '\r'
需特別注意！！
