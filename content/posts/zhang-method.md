---
title: 相機校正 - 張正友法
date: 2018-09-16 21:17:55
draft: false
tags: ["Computer Vision"]
categories: [學習札記, 視覺SLAM]
---
# 為什麼需要校正
待補

# 計算單應性矩陣 H

先前在相機模型中, 設三維世界點 {%math%}X = [X, Y, Z, 1]^{T}{%endmath%}, 二維相機像素座標 {%math%}m=[u, v, 1]^{T}{%endmath%}
我們知道這樣的轉換關係: {%math%}sm = K[R, T]X{%endmath%}
其中 s 是尺度因子, K 是內參
{%math%}K = \begin{bmatrix}
f_{0} & \gamma  & u\\ 
0 &  f_{1}& v\\ 
0 &  0& 1
\end{bmatrix}{%endmath%}
在棋盤格上我們可以當成是 Z = 0 的平面:
{%math%}s\begin{bmatrix}
u\\ 
v\\ 
1
\end{bmatrix}
= K \begin{bmatrix}
r_{1} & r_{2} & r_{3} & t
\end{bmatrix}
\begin{bmatrix}
X\\ 
Y\\ 
0\\ 
1
\end{bmatrix}
= K \begin{bmatrix}
r_{1} & r_{2} & t
\end{bmatrix}
\begin{bmatrix}
X\\ 
Y\\ 
1
\end{bmatrix}{%endmath%}
將 {%math%} K[r1, r2, t] {%endmath%} 稱為 [單應性矩陣(Homography matrix) H](http://www.cnblogs.com/wangguchangqing/p/8287585.html)
如此可以用 H 來約束內參和外參
而 H 可以透過棋盤和成像平面上的點對應計算出來(尚待學習)
{%math%}s\begin{bmatrix}
u\\ 
v\\ 
1
\end{bmatrix}
= H\begin{bmatrix}
X\\ 
Y\\ 
1
\end{bmatrix}{%endmath%}
{%math%}H = [h_{1}\  h_{2}\ h_{3}] =\lambda K[r_{1}\ r_{2}\ t]{%endmath%}

# 計算內參
我們可以得到
{%math%}\lambda = \frac{1}{s}
\\r_{1} = \frac{1}{\lambda }K^{-1}h_{1}
\\r_{2} = \frac{1}{\lambda }K^{-1}h_{2}{%endmath%}
因為 r 1  和 r 2 為 互相垂直的正交基底(orthogonal basis)且為單範正交基底(orthonormal basis)，所以r 1  和 r 2的內積為 0 且向量長度均相同為 1
{%math%}r_{1}^{T}r_{2} = 0
\\\left \| r1 \right \| = \left \| r2 \right \| = 1{%endmath%}
代入得到:
{%math%}h_{1}^{T}K^{-T}K^{-1}h_{2} = 0
\\h_{1}^{T}K^{-T}K^{-1}h_{1} = h_{2}^{T}K^{-T}K^{-1}h_{2}{%endmath%}
代表每個 H 可以提供兩個方程式, 而內參有五個變數, 所以理論上要三張圖片才能求解(這邊還要再了解一下意思)
為了方便計算, 我們定義 B:
{%math%}K^{-1} = \frac{1}{f_{0}f_{1}}\begin{bmatrix}
f_{1} &  0& 0\\ 
\gamma  &  f_{0}& 0\\ 
\gamma v-f_{1}u &  v& 1
\end{bmatrix}{%endmath%}
{%math%}B = K^{-T}K^{-1} = \begin{bmatrix}
B_{11} & B_{12} &B_{13} \\ 
B_{21} & B_{22} & B_{23}\\ 
B_{31} & B_{32} & B_{33}
\end{bmatrix} = 
\begin{bmatrix}
\frac{1}{f_{0}^{2}} & -\frac{\gamma }{f_{0}^{2}f_{1}} & \frac{\gamma v_{0} - f_{1}u}{f_{0}^{2}f_{1}}\\ 
-\frac{\gamma }{f_{0}^{2}f_{1}} &  \frac{\gamma ^{2}}{f_{0}^{2}f_{1}^{2}}& -\frac {\gamma (\gamma v-f_{1}u)}{f_{0}^{2}f_{1}^{2}} - \frac{v}{f_{1}^{2}}\\ 
\frac{\gamma v_{0} - f_{1}u}{f_{0}^{2}f_{1}} &  -\frac {\gamma (\gamma v-f_{1}u)}{f_{0}^{2}f_{1}^{2}} - \frac{v}{f_{1}^{2}}& \frac {(\gamma v-f_{1}u)^{2}}{f_{0}^{2}f_{1}^{2}} + \frac{v^{2}}{f_{1}^{2}} + 1
\end{bmatrix}{%endmath%}
發現 B 是斜對稱矩陣, 所以有效元素 6 個, 寫成 b:
{%math%}b = [B_{11}\ B_{12}\ B_{22}\ B_{13}\ B_{23}\ B_{33}]^{T}{%endmath%}
展開推導可得到:
{%math%}h_{i}^{T}Bh_{j} = \begin{bmatrix}
h_{i1}h_{j1}\\ 
h_{i1}h_{j2} + h_{i2}h_{j1}\\ 
h_{i2}h_{j2}\\ 
h_{i3}h_{j1} + h_{i1}h_{j3}\\ 
h_{i3}h_{j2} + h_{i2}h_{j3}\\ 
h_{i3}h_{j3}
\end{bmatrix} ^{T}
\begin{bmatrix}
B_{11}\\ 
B_{12}\\ 
B_{22}\\ 
B_{13}\\ 
B_{23}\\ 
B_{33}
\end{bmatrix}^{T}
= v_{ij}^{T}b
\\ v_{ij}^{T} = [h_{i1}h_{j1}\ h_{i1}h_{j2} + h_{i2}h_{j1}\ h_{i2}h_{j2}\ h_{i3}h_{j1} + h_{i1}h_{j3}\ h_{i3}h_{j2} + h_{i2}h_{j3}\ h_{i3}h_{j3}]{%endmath%}
回到我們一開始得到的約束式:
{%math%}\left\{\begin{matrix}
h_{1}^{T}K^{-T}K^{-1}h_{2} = 0\\ 
\\h_{1}^{T}K^{-T}K^{-1}h_{1} = h_{2}^{T}K^{-T}K^{-1}h_{2}
\end{matrix}\right.
\Rightarrow \left\{\begin{matrix}
v_{12}^{T} b= 0\\
v_{11}b = v_{22}b 

\end{matrix}\right.{%endmath%}
寫成矩陣:
{%math%}\begin{bmatrix}
v_{12}^{T}\\ 
(v_{11}-v_{22})^{T}
\end{bmatrix}b = 0
{%endmath%}
根據每個校正板, 都可以得到一個 {%math%} Vb = 0{%endmath%} 的式子
我們就能得到內參的各個參數

# 計算外參
待補

# 參考
1. [Camera Calibration(相機校正)](http://hackpad.g0v.link/YYvM1CE8lv2.html)
2. [张正友标定算法原理详解](https://blog.csdn.net/u010128736/article/details/52860364)
3. [SLAM入门之视觉里程计(6)：相机标定 张正友经典标定法详解](https://www.cnblogs.com/wangguchangqing/p/8335131.html)
4. [内参、外参、畸变参数三种参数与相机的标定方法与相机坐标系的理解](https://blog.csdn.net/yangdashi888/article/details/51356385)
5. [张正友相机标定Opencv实现以及标定流程&&标定结果评价&&图像矫正流程解析（附标定程序和棋盘图](https://blog.csdn.net/dcrmg/article/details/52939318)
6. [攝像頭校正 camera calibration - part 2 calibration](http://wycwang.blogspot.com/2012/10/camera-calibration-part-2-calibration.html)
7. [机器视觉的相机标定到底是什么？](https://www.zhihu.com/question/29448299)
8. [相机标定（Camera calibration）原理、步骤](https://blog.csdn.net/lql0716/article/details/71973318)