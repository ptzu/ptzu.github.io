---
title: 藍芽鍵盤於 Windows 和 Ubuntu 雙系統之配對與開機
date: 2018-09-11 21:25:58
draft: false
tags: ["Linux"]
categories: [學習札記, Linux]
---

# 前言
最近組了一台新電腦放到交大宿舍
不過位置有點小
所以採用了無線滑鼠和鍵盤
其中鍵盤是 Logitech K810 藍芽
![相當精緻質感的鍵盤](https://i.imgur.com/xIy0Ek1.jpg)
其實如果都選 2.4GHz 的接收器鍵鼠, 就沒有這篇了ＸＤ
但是 K810 的造型實在太生火了
所以還是買了下去
這篇主要解決了以下問題:
1. 藍芽鍵盤無法在開機選單選擇系統
2. 藍芽在切換系統後需要重新配對
3. 切換系統後時間不一致

# 藍芽於 BIOS
由於藍芽必須在系統啟動後才能配對
所以在 BIOS 階段是無法使用的ＱＱ
在我的搜尋下有兩個解法
一是將 Ubuntu grub 改成可以用滑鼠選擇
這裡我找到了 Gujin bootloader 
號稱 mouse support 的開機選單
不過套件有點久遠, 又一直 build 不起來
只好放棄了
二是使用 EasyBCD 替 Windows 製作多系統開機選單
這個流程是已經載入 Windows 了
所以可以使用藍芽鍵鼠進行選擇

## 系統背景
我的雙系統都是以 MBR 格式安裝的
並用 BIOS(Lagacy) 啟動
若是用 UEFI 的話不確定能否使用

## 還原 Windows bootloader
首先我是先安裝 Windows 再裝 Ubuntu
Windows 的 bootloader 在 MBR 磁區會被蓋掉
所以我們開機的時候會是 grub
但是因為找不到 grub 的解法ＸＤ
所以我們必須再把 grub 蓋掉
這部分我們可以到 Windows 安裝畫面(CD or USB)
然後按修復電腦 -> 使用命令提示字元
接著切換到開機的磁區(C槽)
輸入指令
```
bootsect.exe /nt60 all /mbr
```
當出現 Successfully updated disk bootcode. 時就成功
接著重開機確認是否沒有出現 grub 了

## 使用 EasyBCD
下載好之後
如圖設定:
![](https://i.imgur.com/zeXkc2t.png)
再按加入項目
重新開機後 
應該可以看到 Windows 選單
裡面包含剛剛加入的 Ubuntu 16.04

# 藍芽重新配對
當藍芽和裝置配對時
電腦端會產生一組 key 存下來
鍵盤也會
而切到另一個系統的時候
再配對一次又會產生一組新的 key
導致切回去的時候 key 不相符
所以我們要做的是將兩個系統記載的 key 修改為一樣

首先在 Ubuntu 配對好之後
再到 Windows 配對
這時候鍵盤記錄的 key 是 Windows 的
然後我們想要把 Ubuntu 的 key 改成這個

1. 安裝 chntpw
2. 在 Ubuntu 檔案系統中, 找到 Windows/System32/config 的路徑並開啟終端機
3. 輸入 chntpw -e SYSTEM
4. cd ControlSet001\\Services\\BTHPORT\\Parameters\\Keys
5. ls
6. cd "ls 出來的那個資料夾"
7. ls
8. hex "那個 value 得值"
9. 記好那一段 16 進位的字串(key)
10. 到 /var/lib/bluetooth 連續進入兩個資料夾(16進位樣子的)
11. 以管理員權限編輯 info
12. 找到一段 [LinkKey] Key=0xXXXXXXXXXX 那裡換成剛剛記下來的 key
13. 重新啟動就大功告成

# 系統時間不一致
其實這和藍芽沒啥關係
順便解掉
在 Windows 中時間以 BIOS 顯示的時間為主
而在 Ubuntu 中以 UTC+8 為時間, 而他會把 BIOS 時間改成 UTC+0
這就是為什麼會到 Windows 時時間會慢 8 小時
解決方法是到 Ubuntu 禁用 UTC
```
timedatectl set-local-rtc 1 --adjust-system-clock
```

# 參考
1. https://blog.csdn.net/zbjdonald/article/details/45082399
2. http://katsuno.pixnet.net/blog/post/264422257-%E7%A7%BB%E9%99%A4grub 
3. https://www.zhihu.com/question/46525639
4. 查看 boot mode: http://www.rodsbooks.com/refind/bootmode.html#windows
5. Ubuntu 開機速度: https://kknews.cc/zh-tw/entertainment/8bn8ke.html
