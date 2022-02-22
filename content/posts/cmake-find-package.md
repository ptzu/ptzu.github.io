---
title: CMake 搜尋函式庫 find_package()
date: 2018-09-14 10:29:17
draft: false
tags: ["Codeforces", "Develop"]
categories: [學習札記, Linux]
---

最近使用 CMake 時
使用到了 OpenCV 這個庫
不過因為我電腦裡面有兩個版本
一個是 ROS 自裝的, 一個是自己編譯好的
他一直找到舊版本的函式庫
於是自己就好奇他是如何找到的

當我們呼叫 find_package(套件) 時, 他會從以下步驟搜尋 Find<Name>.cmake 檔
1. 從 ${CMAKE_MODULE_PATH} 這個變數中的路徑去搜尋
通常這個是 project 中的 cmake 資料夾
2. 如果找不到, 會到 cmake 中的模組搜尋, 路徑通常是 /usr/share/cmake-x.y/Modules
3. 最後一種是套件安裝時, 會在他自己的資料夾產生 .cmake 檔讓別人搜尋, 稱為配置模式, 尋找 <NAME>Config.cmake

當找到 .cmake 檔時
會配置以下變數
```
<NAME>_FOUND
<NAME>_INCLUDE_DIRS or <NAME>_INCLUDES
<NAME>_LIBRARIES or <NAME>_LIBRARIES or <NAME>_LIBS
<NAME>_DEFINITIONS
```

以 OpenCV 為例, 就會先在模組找 FindOpenCV.cmake
找不到再去找 OpenCVConfig.cmake

# 參考
1. https://blog.csdn.net/u011092188/article/details/61425924
2. https://blog.csdn.net/haluoluo211/article/details/80559341
