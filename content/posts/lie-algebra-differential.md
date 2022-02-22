---
title: 李代數 SO(3), SE(3) 微分推導
date: 2018-09-15 22:03:57
draft: false
tags: ["Lie Algebra"]
categories: [學習札記, 視覺SLAM]
---
# SO(3)
當我們考慮到無限小的運動時
就會需要到微分
對於旋轉矩陣 R(t)
我們知道:
{%math%}R(t)R^{T}(t) = I{%endmath%}
事實上旋轉矩陣的反矩陣, 即為他的轉置矩陣
接著我們對這個式子兩邊做微分:
{%math%}\frac{d}{dt}(RR^{T}) = \dot{R}R^{T} + R\dot{R^{T}} = 0{%endmath%}
移項後得到:
{%math%}\dot{R}R^{T} = -(\dot{R}R^{T})^{T}{%endmath%}
此時可以發現到 {%math%} \dot{R}R^{T} {%endmath%} 就是斜對稱矩陣！
令 {%math%} \hat{w}(t) = -(\dot{R}R^{T})^{T} {%endmath%}
右乘 {%math%} R(t) {%endmath%} 得到:
{%math%}\dot{R}(t) = \hat(w)R(t){%endmath%}
如此就得到 R 微分的關係式

# SE(3)
接著考慮到剛體運動
對於 {%math%}g = \begin{bmatrix}
R & T\\ 
0 & 1
\end{bmatrix}{%endmath%}
這邊我想了一整天, 才發現很根本的錯誤
原本我想照著 SO(3) 一樣乘上轉置矩陣去求
可是怎麼算都不對
原因就是 SE(3) 的轉置不是他的反矩陣啊啊啊！
所以回到原本
我們從 {%math%}\dot{g}g^{-1}{%endmath%} 考慮
等等右乘上 g 就可以得出 g 微分的關係式
可以得到:
{%math%}\dot{g}g^{-1}=\begin{bmatrix}
\dot{R} & \dot{T}\\ 
0 & 0
\end{bmatrix}
\begin{bmatrix}
R^{T} & -R^{T}T\\ 
0 & 1
\end{bmatrix}
=\begin{bmatrix}
\dot{R}R^{T} & \dot{T}-\dot{R}R^{T}T\\ 
0 & 0
\end{bmatrix}{%endmath%}

(註: 我也不知道 g 的反矩陣怎麼求的, 總之湊一湊應該可以)

轉換一下:
{%math%}\dot{g}g^{-1}=\begin{bmatrix}
\hat{w}(t) & v(t)\\ 
0 & 0
\end{bmatrix}
\equiv \hat{\xi }(t){%endmath%}
{%math%}\hat{g}=\hat{\xi }g{%endmath%}
其中 {%math%}\hat{\xi }{%endmath%} 稱為 twist, 剛體運動 g(t) 曲線的正切向量
這些 twist 的集合就是李代數 se(3)

# 參考
1. [lie group and computer vision : 李群、李代数在计算机视觉中的应用](https://blog.csdn.net/heyijia0327/article/details/50446140)