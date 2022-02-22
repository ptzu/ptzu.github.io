---
title: Mac OSX 遠端連線 GUI
date: 2018-07-18 22:03:52
draft: false
tags: ["Linux"]
categories: [學習札記, Linux]
---

最近沒 ubuntu 環境可以用
只好遠端連線到實驗室 server
但是要跑一些程式需要 GUI 介面
於是了解了一下如何用 ssh 開啟圖形介面

# 架構
圖形介面呈現由 X window system
主要分為 X server 和 X cilent
詳細請見[鳥哥](http://linux.vbird.org/linux_basic/0590xwindow.php)
架構圖如下:
![](https://i.imgur.com/3YJDhzc.gif)
而我們的 X client 指的就是遠端伺服器(Linux)
必須先確定有裝 X window(Desktop Version)
接著 X server 就是我們的本地端(Mac)
OSX 上有個 X window 的開源實作: XQuartz

# 設定
## Linux
在 /etc/ssh/sshd_config 中加入
```
X11Forwarding yes
X11DisplayOffset 10
```
## MAC
在 /private/etc/ssh/ssh_config 中加入
```
ForwardX11yes
```
不過我對這個保持懷疑
因為我沒修改也可以成功執行

# 執行結果
在 XQuartz 打開終端機輸入
```
ssh -X username@remoteaddress
```
![](https://i.imgur.com/seGXeqh.jpg)
執行 gvim 看看
有成功叫出圖形介面

不過在 docker 環境下就沒辦法
之後再想辦法:
1. https://unix.stackexchange.com/questions/403424/x11-forwarding-from-a-docker-container-in-remote-server
2. https://www.youtube.com/watch?v=PKyj8sbZNYw