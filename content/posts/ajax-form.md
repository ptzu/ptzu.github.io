---
title: 不使用 JQuery 達到 Ajax 上傳表單
date: 2017-05-20 13:19:55
draft: false
tags: ["Ajax"]
categories: [學習札記, 網頁設計]
---

這篇不介紹使用 JQuery 的 Ajax
而是利用 XMLHttpRequest(xhr) 來做到相同的效果
首先是 Ajax 的 Javascript 語法
**xmlhttp.open(方法, 請求對象, true)**

## GET 例子

```javascript
    var xmlhttp = new XMLHttpRequest();
    xml.http.open("GET", XXX.php, true);
    xmlhttp.onreadystatechange=function check_user()
    {
        if(xmlhttp.readyState == 4)
        {
            if(xmlhttp.status == 200)
            {
                ...
            }
        }
    }
    xml.send(null);
```

如果想帶參數, 在請求對象那邊可以改成
XXX.php?arg1=val1&&arg2=val2
然後在對象那邊用 $_GET['arg1'], $_GET['arg2']
來取得

## POST 例子
待測試

```javascript
    var xmlhttp = new XMLHttpRequest();
    xml.http.open("POST", XXX.php, true);
    xmlhttp.onreadystatechange=function check_user()
    {
        if(xmlhttp.readyState == 4)
        {
            if(xmlhttp.status == 200)
            {
                ...
            }
        }
    }
    xmlhttp.send("arg1=val1&&arg2=val2");
```

回到主題, 如果我們想像 form 那樣發送 POST 請求呢？
這時候我們需要用到 FormData 這個物件
詳細內容請參閱 [Formdata](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects)

以下是我的例子
```javascript
    <form action="javascript:getPage(this);" method="post" enctype="multipart/form-data" class="upload" name="upload">
        <input type="file" name="upfile" id="upfile"><br>
        <button type="submit" class="btn">Upload</button>
    </form>
    function getPage(opt)
    {
        var files = document.forms.namedItem("upload");
        var xmlhttp = new XMLHttpRequest();
        var fd = new FormData(files);
        xmlhttp.open("POST","../php/insertFilePic.php",true);
        xmlhttp.onreadystatechange=function check_user()
        {
            if(xmlhttp.readyState == 4)
            {
                if(xmlhttp.status == 200)
                {
                    ...
                }
            }
        }
        // console.log(dd);
        xmlhttp.send(fd);
    }
```
如果 input 含有 type file 的
在請求對象那裡一樣是用 $_FILES 去接
另外 console.log(FormData) 是沒東西的
以上就是如何利用 javascript 來做到 Ajax 的 GET 和 POST
