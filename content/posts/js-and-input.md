---
title: 用 Javascript 取得 Html input
date: 2017-04-13 23:36:53
draft: false
tags: ["Javascript"]
categories: [學習札記, 網頁設計]
---
寫作業遇到要用 JS 取得 input tag 送出的值
在 StackOverflow 查到有六種方法:

以
```
<input name="searchTxt" type="text" maxlength="512" id="searchTxt" class="searchField"/>
```
為例


1. document.getElementById('id').value
2. document.getElementsByClassName('class_name')[].value
3. document.getElementsByTagName('tag_name')[].value
4. document.getElemenstByName('name')[].value
5. document.querySelector('any').value
6. document.querySelectorAll('any')[].value
---
2, 3, 4, 6都是返回一群元素

5, 6是利用 CSS selector
