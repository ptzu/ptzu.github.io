---
title: 對極幾何 Epipolar Geometry
date: 2018-09-24 10:40:17
draft: false
tags: ["Codeforces", "Computer Vision"]
categories: [學習札記, 視覺SLAM]
---
對於一個場景, 透過兩個位置不同的相機觀看
這兩幅圖像之間會有一些點對應關係
整張圖所形成的幾何關係稱為 Epipolar Gemometry
如下圖: 
![](https://i.imgur.com/Z61FHfJ.png)

兩個相機中心分別為 {%math%} O_{1},\ O_{2} {%endmath%}
三維世界座標點 {%math%} P {%endmath%} 
在兩個像平面 {%math%} A_{1},\ A_{2} {%endmath%} 投影的點為 {%math%} p_{1},\ p_{2} {%endmath%}
而 {%math%} P,\ O_{1}\ O_{2} {%endmath%} 三點所形成的平面稱為極平面(Epipolar plane)
{%math%} O_{1}O_{2} {%endmath%}稱為基線(Baseline), 穿越兩個像平面的點分別為 {%math%} e_{1},\ e_{2} {%endmath%} 稱為極點(Epipoles)
投影點與極點連成的線稱為極線(Epipolar Line): {%math%} l_{1},\ l_{2} {%endmath%}

我們可以透過兩幀圖像之間的特徵點對應, 進而求出相機的運動

# 推導
設點三維做標點
{%math%} P=[X,Y,Z]^{T} {%endmath%}
對於兩個投影點
{%math%} s_{1}p_{1}=KP\\s_{2}p_{2}=K(RP+t) {%endmath%}
K 是內參矩陣
若是用齊次座標, 可以將 s 去除, 因為常數項不影響
接著我們覺得 K 有點麻煩, 所以我們用[歸一化座標](https://zhuanlan.zhihu.com/p/33583981)
{%math%} x_{1}=K^{-1}p_{1},\ x_{2}=K^{-1}p_{2} {%endmath%}
看公式可以知道這東西其實就是 P
而我們不想透過 K 來得到投影點座標
反而是直接假設有一個距離相機 z = 1 的平面
投影在上面的座標為:
{%math%} P=\begin{bmatrix}
\frac{X}{Z}\\ 
\frac{Y}{Z}\\ 
1
\end{bmatrix} {%endmath%}
來得到二維座標
我們可以得到這樣的關係:
{%math%} x_{2}=Rx_{1} + t {%endmath%}
接下來就是數學的一些變換:
兩邊同時乘上 {%math%} \hat{t} {%endmath%}, 相當於與 t 做外積
{%math%} \hat{t}x_{2}=\hat{t}Rx_{1} {%endmath%}
上面的 t 因為和自己做外積, 所以消失了
接著左乘 {%math%} x_{2}^{T} {%endmath%}
因為 {%math%} \hat{t}x_{2} {%endmath%} 是一個和 {%math%} t,\ x_{2} {%endmath%} 垂直的向量
再和{%math%} x_{2} {%endmath%} 做內積就會是 0
得到以下式子:
{%math%} x_{2}^{T}\hat{t}Rx_{1} = 0 {%endmath%}
此式就稱為 對極約束(Epipolar Constraint)
代入 {%math%} p_{1}, p_{2} {%endmath%}
{%math%} p_{2}^{T}K^{-T}\hat{t}RK^{-1}p_{1} = 0 {%endmath%}
幾何意義是 {%math%} P,\ O_{1},\ O_{2} {%endmath%} 共平面
將 {%math%} \hat{t}R {%endmath%} 令成 E, 這東西就稱為本質矩陣(Essential Matrix)
若是有包含內參, 就是基本矩陣(Fundamental Matrix)
{%math%} E=\hat{t}R,\ F=K^{-T}EK^{-1} {%endmath%}
進一步簡化約束式:
{%math%} x_{2}^{T}Ex_{1}=p_{2}^{T}Fp_{1}=0 {%endmath%}

到這邊我們可以得到一個和 x 有關的式子
可以藉由配對點對來求得 E 或 F
再根據 E 或 F 取得 R, t
以上推導來自高翔.

# 延伸
由於約束式的幾何意義是三點共面
所以有這樣的式子:
{%math%} (P - p_{1})[(O_{1}-O_{2})\times (P - p_{2})]=0 {%endmath%}
我想從這個推回去約束式, 不過一直沒有成功QQ
時間關係只好先擱著

而基本矩陣有一些基本特性:
1. rank = 2
2. 自由度為 5

可以參考[這首歌](https://www.youtube.com/watch?v=DgGV3l82NTk)ＸＤ

# 參考
1. [相机位姿求解问题？](https://www.zhihu.com/question/51510464)
2. [本质矩阵和基础矩阵的区别是什么？](https://www.zhihu.com/question/27581884)
3. [Pinhole Camera: Epipolar Geometry(感覺推導有些問題)](http://silverwind1982.pixnet.net/blog/post/153221294-pinhole-camera%3A-epipolar-geometry)
4. [Epipolar Geometry](http://www.cs.toronto.edu/~jepson/csc420/notes/epiPolarGeom.pdf)
