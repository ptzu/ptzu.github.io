---
title: MySQL 新增使用者
date: 2017-05-31 21:38:43
draft: false
tags: ["MySQL"]
categories: [學習札記, 資料庫]
---

## 新增帳號
mysql> GRANT 權限 ON 資料庫(或資料表TABLE) TO user@host IDENTIFIED BY '密碼';

資料庫的權限有 15 種：
ALTER	CREATE	DELETE	DROP
FILE	INDEX	INSERT	PROCESS	REFERENCES
RELOAD	SELECT	SHUTDOWN	UPDATE	USAGE
ALL PRIVILEGES(以上全部開放)

host 為主機, 如果架在本機, 就打 localhost
