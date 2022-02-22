---
title: SQL 常見指令
date: 2017-04-07 23:18:31
draft: false
tags: ["Database", "SQL"]
categories: [學習札記, 資料庫]
---

建立資料庫: create database 資料庫名稱;

列出所有資料庫: show databases;

使用資料庫: use 資料庫名稱;

建立資料表: create table 資料表名稱(欄位名稱 型態,...)
            型態有: integer, real, char()

刪除資料表: drop table 資料表名稱;

新增資料表欄位: alter table 資料表名稱 add 欄位名稱 型態

刪除資料表欄位: alter table 資料表名稱 drop 欄位名稱 型態

插入資料: insert into 資料表名稱(欄位1, 欄位2, ...) values('值1', '值2', ...)

刪除資料: delete from 資料表名稱 where 條件式
          ex: delete from Students as S where S.name = 'Smith';
          其中 as S 是縮寫寫法, as 可以不用加

更新資料: update 資料表名稱 set 欄位名稱 = 值, ... where 條件式

更改欄位型態: alter table 資料表名稱 change column 舊的名稱 新的名稱 型態

刪除 foreign key: alter table 資料表名稱 drop foreign key CONSTRAINT的名稱
                  如果建 constraint 時沒有給名稱, 系統會預設
                  利用 show create table 資料表名稱 來查詢相關資訊
                  ![](http://i.imgur.com/IGkEBod.png)
查詢資料庫編碼: status;

更改資料庫編碼: ALTER DATABASE 資料庫名稱 CHARACTER SET utf8 COLLATE utf8_general_ci;
