---
title: "從 Hexo 遷移至 Hugo 上"
date: 2022-02-04T10:44:02+08:00
draft: false
tags: ["Blog"]
---

此 blog 參考了 [Hugo 貼身打造個人部落格](https://ithelp.ithome.com.tw/articles/10252313)  
原先使用 Hexo 需要使用 Node.js, 用久了感覺整個專案有點龐大  
看了一些文章分享 Hugo 的 render 速度蠻快的, 於是來試用看看  
Hugo 也是使用 markdown 來寫作, 所以遷移上沒什麼問題  
Hugo themes 數量蠻多的, 此 blog 使用 "m10c"  

# 自動化佈署
參考了 [將Hugo部落格佈署到Github上](https://yurepo.tw/2021/03/%E5%A6%82%E4%BD%95%E5%B0%87hugo%E9%83%A8%E8%90%BD%E6%A0%BC%E9%83%A8%E7%BD%B2%E5%88%B0github%E4%B8%8A/) 這篇  
使用 Github Action 的功能, 可以在新增文章的時候只需要上傳 markdown, 剩下會自動渲染成靜態網頁  
使用上還蠻方便的