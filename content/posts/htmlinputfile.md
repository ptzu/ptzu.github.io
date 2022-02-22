---
title: HTML, PHP 如何上傳檔案？
date: 2017-05-20 14:31:01
draft: false
tags: ["HTML", "PHP"]
categories: [學習札記, 網頁設計]
---

要在 Html 有上傳檔案的介面
必須利用到
```html
<input type='file' name='upfile' class='upfile'>
```
這樣就會產生大家熟悉的'選擇檔案'按鈕
接著利用表單把檔案送出去
如果表單中有 file 類型的 input
enctype="multipart/form-data" 是必須要加的
```html
<form action="XXX.php" method="post" enctype="multipart/form-data" name="upload">
    <input type='file' name='upfile' class='upfile'>
    <button type="submit" class='btn'> Upload</button>
</form>
```

## PHP處理檔案

我們可以用 $_FILES 陣列來取得上傳的檔案
上傳之後會存在一個暫存資料夾
如果要把檔案存在伺服器, 記得利用 move_uploaded_file 移到你要的資料夾

$_FILES["upload"]["name"]：上傳檔案的原始名稱。
$_FILES["upload"]["type"]：上傳的檔案類型。
$_FILES["upload"]["size"]：上傳的檔案原始大小。
$_FILES["upload"]["tmp_name"]：上傳檔案後的暫存資料夾位置。
$_FILES["upload"]["error"]：如果檔案上傳有錯誤，可以顯示錯誤代碼。

參考資料:
1. http://www.webtech.tw/info.php?tid=24
2. https://www.tad0616.net/modules/tad_book3/html.php?tbdsn=100
3. http://forum.twbts.com/thread-3299-1-1.html
