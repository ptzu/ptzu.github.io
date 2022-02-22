---
title: 在 Ubuntu 安裝 phpmyadmin
date: 2017-05-31 22:25:55
draft: false
tags: ["Database", "PHP"]
categories: [學習札記, 資料庫]
---

在 Ubuntu 安裝 phpmyadmin 非常簡單

## 安裝 MySQL
```
sudo apt-get install mysql-server
```
過程中會要求你輸入 root 密碼

## 安裝 phpmyadmin
```
sudo apt-get install phpmyadmin
```
過程會有
1.選擇伺服器(apache2)
2.剛剛設置的 mysql root 密碼

phpMyAdmin的設置文件在: /etc/phpmyadmin/config.inc.php

安裝好之後還不能馬上使用
需要在 apache2.conf 下添加
```
Include /etc/phpmyadmin/apache.conf
```
之後在重啟 apache server

就可以透過 host/phpmyadmin 來登入
