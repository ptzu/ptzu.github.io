---
title: PHP 時間函數
date: 2017-05-17 20:57:24
draft: false
tags: ["PHP"]
categories: [學習札記, 網頁設計]
---
## 設定時區
```php
date_default_timezone_set("Asia/Taipei")
```

## 日期
```php
date('Y-m-j');
```
2017-05-17

年: Y(四位數字), y(兩位數字)
月: F(英文全稱), M(英文縮寫), m(數字前導), n(數字無前導)
日: j(沒前導), d(有前導)

## 時:分:秒
```php
date('g:i:s a');
```
11:19:30 pm

時: g(12小時制無前導), h(12小時制有前導), 小寫 a 代表 am, pm, 大寫 A 代表 AM, PM

```php
echo date('T');
```
UTC
大寫T表示伺服器的時間區域設置

```php
echo date('U');
```
1170769424
大寫U表示從1970年1月1日到現在的總秒數，就是Unix時間紀元的UNIX時間戳。
