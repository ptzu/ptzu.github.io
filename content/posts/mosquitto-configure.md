---
title: Windows 上安裝 mosquitto
date: 2017-05-15 20:40:31
draft: false
tags: ["IOT"]
categories: [學習札記, 專題]
---

# 所需檔案
1. [Mosquitto for Windows](https://mosquitto.org/download/)
![](http://i.imgur.com/TRxLPbM.png)

2. [Win32 OpenSSL](https://slproweb.com/products/Win32OpenSSL.html)
![](http://i.imgur.com/icvPV92.png)

3. [pthread-win32](ftp://sourceware.org/pub/pthreads-win32)
根據路徑找到檔案下載
![](http://i.imgur.com/QPxv1VQ.png)
![](http://i.imgur.com/TW2E21R.png)

# 安裝 mosquitto

執行 mosquitto 安裝程式

過程選擇安裝路徑
![](http://i.imgur.com/J2b4S4k.png)

勾選下面那個
![](http://i.imgur.com/AJu2mKj.png)

接著按 Next, 應該會出現以下錯誤畫面
因為我們缺少 **libeay32.dll** 和 **ssleay32.dll**

![](http://i.imgur.com/jqaMO77.png)
![](http://i.imgur.com/ktuOWuA.png)

## 安裝需要的元件
回到最上面說的, 在Win32 OpenSSL那裡有下載了安裝檔
我們要安裝它來取得 **libeay32.dll** 和 **ssleay32.dll**

選擇安裝路徑
![](http://i.imgur.com/vmE8fIq.png)

最後面的畫面是要你捐錢, 沒有要捐就把它勾掉吧
![](http://i.imgur.com/jJRhNSo.png)

然後我們到安裝好的資料夾取出 **libeay32.dll** 和 **ssleay32.dll**
![](http://i.imgur.com/2nnqHkJ.png)

以及在 pthread-win32 下載的 **pthreadVC2.dll**
一起放到剛剛未完成安裝的 mosquitto 資料夾

![](http://i.imgur.com/KIHFQmy.png)

到這邊就安裝完成了！

# 啟動 mosquitto 伺服器
到工作管理員/服務
找到 mosquitto 啟動它
![](http://i.imgur.com/uYTgXjH.png)

# 簡單測試 subscribe 和 publish
本機當作伺服器測試
打開cmd
## 訂閱
```
"C:\Program Files (x86)\mosquitto\mosquitto_sub.exe" -d -h 127.0.0.1 -t test
```
## 發送
```
"C:\Program Files (x86)\mosquitto\mosquitto_pub.exe" -d -h 127.0.0.1 -t test -m "test message"
```

參數解釋
-d: 開啟 debug 模式
-h: 主機 IP address
-t: 訂閱主題
-m: 訊息內容

測試結果:
![](http://i.imgur.com/xCVF8uJ.png)

![](http://i.imgur.com/oOexqvU.png)

![](http://i.imgur.com/EWZRPC4.png)
