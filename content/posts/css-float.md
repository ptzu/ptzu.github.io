---
title: 利用 CSS 達到 div 併排
date: 2017-05-19 11:44:58
draft: false
tags: ["Codeforces", "CSS"]
categories: [學習札記, 網頁設計]
---
假設我們現在有3個區塊想要並列一起
```html
<div class="wrap">
    <div class="a"></div>
    <div class="b"></div>
    <div class="c"></div>
</div>
```
我們可以在 Css 中利用 float 來達成併排效果
```css
.wrap{
    background-color: green;
}
.a, .b, .c{
    float: left;
}
```
這樣就可以達到效果
但這時候你可能會發現, 父階層的 div 怎麼沒有高度？
原因是 float 造成元素沒有撐起 div 高度

有三種方法解決
1. 對父階層設定高度
2. 使用 css clear
在結束標籤前加一個 div(注意不是直接作用在父階層哦)
```html
<div class="wrap">
    <div class="a"></div>
    <div class="b"></div>
    <div class="c"></div>
    <div class="clear"></div>
</div>
```css
.clear{
    clear:both;
}
```
3. 對父階層使用 css overflow
```css
.wrap{
    overflow: hidden;
}
```
