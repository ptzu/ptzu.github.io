---
title: IIS Express 遠端(內網)存取
date: 2017-09-22 10:12:28
draft: false
tags: ["Network"]
categories: [學習札記, 專題]
---
在測試 Web API 的時候, localhost 測都沒有問題
但是在不同電腦上測卻出現了 400 Bad Request
找了蠻多資料才解決
因為預設只開放 localhost 連線
所以我們必須做一些設定才可遠端連線
整理如下:

# 設定IP
在 cmd 輸入 ipconfig 取得本機IP
## 取得專案 port
![](https://i.imgur.com/ExVVpLu.png)
![](https://i.imgur.com/7UhnTKp.png)
## 使用 netsh 將本機 IP 及 port 加入 URL 保留區
![](https://i.imgur.com/u0ZBaWd.png)
## 檢查設定
```
netsh http show urlacl
```
## 刪除設定
測試完再做刪除就好
```
netsh http delete urlacl url=http://192.168.1.101:51320/
```
# 修改 IIS Express 設定
在"專案"資料夾下找到
```
.vs/config 資料夾中的 applicationhost.config
```
打開之後在裡頭添加 binding
註: VS2013是採用集中管理, VS2015之後將設定檔改成分散式管理, 也就是在各個專案之中, 注意版本, 修改錯設定檔會沒有效果
![](https://i.imgur.com/PK0EHXQ.png)
# 修改防火牆規則
在輸入規則添加專案所使用的 port
添加方法不再贅述
![](https://i.imgur.com/uJK6KC7.png)
# 測試結果
打開 IIS Express 應該要有 2 個站台
![](https://i.imgur.com/rkrwnC0.png)
# 可能遇到的問題
## 打不開網頁
請用管理員權限打開 Visual Studio
![](https://i.imgur.com/CJa83np.png)
## 出現 503 Service Unvailable
binding 設定沒設定好
注意是否修改錯檔案

# 參考資料
1. http://ithelp.ithome.com.tw/articles/10185364
2. https://stackoverflow.com/questions/5442551/iisexpress-returns-a-503-error-from-remote-machines
