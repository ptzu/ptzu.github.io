---
title: 相機座標系、世界座標系
date: 2018-08-08 23:15:18
draft: false
tags: ["Computer Vision"]
categories: [學習札記, 視覺SLAM]
---
最近在讀 SLAM 相關 paper 時
常看到 camera/world frame
我原本想說是和 keyframe 類似的東西
結果是完全不同的東西呀～
這邊指的是座標系(雖然我還是不知道為什麼要用frame)

順手整理一下相機的相關座標轉換
相機模型中主要有四個平面座標系:
1. 畫素平面座標系（u,v): 圖像在像素中的位置
2. 像平面座標系 (影像物理座標（x,y)): 以相機主點為原點，場景點在圖像平面上的投影座標。 
3. 相機座標系（Xc,Yc,Zc): 以相機為中心的三維座標。
4. 世界座標系（Xw,Yw,Zw): 絕對座標系統中的三維座標。

當我們能將這四個座標軸作轉換, 那麼就能從二維影像得知在三維世界中的位置

# 畫素平面 <=> 像平面
因為畫素不能反映圖像真實尺寸
像平面可以想成是一張二維影像
只是被縮小成像素大小(物理層面)
畫素單位是 pixel(微米大小), 而像平面單位是 mm

假設畫素座標為 {%math%}(u, v, 1)^{T}{%endmath%} (這邊用的是齊次座標, 當成是(u, v)就好了)
像平面座標 {%math%}(x, y){%endmath%}
每個像素的真實尺寸為 dx * dy

我們以圖像中心當作像平面座標的原點 {%math%}(u_{0}, v_{0}){%endmath%}
所以: 
{%math%}\left\{\begin{matrix}
u=\frac{x}{dx}+u_{0}\\ 
v=\frac{y}{dy}+v_{0}
\end{matrix}\right.{%endmath%}
{%math%}\left\{\begin{matrix}
x=udx-u_{0}dx\\ 
y=udy-u_{0}dy
\end{matrix}\right.{%endmath%}

若以矩陣來表示: 
{%math%}\begin{bmatrix}
u\\ 
v\\ 
1
\end{bmatrix}
=
\begin{bmatrix}
\frac{1}{dx} & 0 & u_{0}\\ 
0& \frac{1}{dy} & v_{0}\\ 
0& 0 & 1
\end{bmatrix}\begin{bmatrix}
x\\ 
y\\ 
1
\end{bmatrix}{%endmath%}

# 像平面 <=> 相機
相機座標系是一個三維空間
假設三維坐標中的一點 P, 投影到平面一點 p
可以先參考這張圖:
![](https://i.imgur.com/xRh9odS.jpg)

f 是焦距, P 為在 camera frame 上的一點, 投影到像平面上一點 p 
根據相似三角形: {%math%}\frac{f}{Z} = \frac{x}{X} = \frac{y}{Y}{%endmath%}
得到像平面座標(x, y):
{%math%}
\left\{\begin{matrix}
x=\frac{fX}{Z}\\ 
y=\frac{fY}{Z}
\end{matrix}\right.
{%endmath%}
以矩陣表示:
{%math%}
Z_{c}\begin{bmatrix}
x\\ 
y\\ 
1
\end{bmatrix}
=
\begin{bmatrix}
f & 0 & 0\\ 
0 &  f& 0\\ 
0 & 0 & 1
\end{bmatrix}\begin{bmatrix}
X_{c}\\ 
Y_{c}\\
Z_{c}
\end{bmatrix}
{%endmath%}

# 相機 <=> 世界
三維世界之間的座標轉換
可以用一個旋轉和平移矩陣表示
故:
{%math%}
\begin{bmatrix}
X_{c}\\ 
Y_{c}\\ 
Z_{c}\\ 
1
\end{bmatrix}
=
\begin{bmatrix}
R & T\\ 
0 & 1
\end{bmatrix}
\begin{bmatrix}
X_{w}\\ 
Y_{w}\\ 
Z_{w}\\ 
1
\end{bmatrix}
{%endmath%}

到這邊我們就可以完成相素到世界座標的轉移
在相機校正的時候會看到所謂的內參和外參數
相素到相機的轉移矩陣就稱為內參數矩陣
而相機到世界的轉移就稱為外參數矩陣
以上式子整理一下就可以得到

# 參考
1. [机器视觉相机标定？](https://www.zhihu.com/question/58489965)
2. [世界座標系和相機座標系,影象座標系的關係](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/415506/)
3. [相机参数标定（camera calibration）及标定结果如何使用](https://blog.csdn.net/Aoulun/article/details/78768570)
4. http://silverwind1982.pixnet.net/blog/post/134551091
5. https://blog.csdn.net/liulina603/article/details/52953414
6. https://blog.techbridge.cc/2018/04/22/intro-to-pinhole-camera-model/