---
bg: "photo10.jpg"
layout: post
title: "相机标定"
crawlertitle: Camera Calibration #"page title"
summary: Camera Calibration #"post description"
date: 2019-05-28 14:00:00 +0800
tags : 'matrix theory'
author: "vpromise"
categories: "tech"
---


*相机标定的方法，介绍世界坐标系、相机坐标系、成像平面坐标系和图像像素坐标系之间的转换关系*

---
# 相机标定中矩阵的应用

## 摘要
矩阵理论作为一门重要的数学分支，其在数值分析、最优化方法、控制理论、数学模型等工程领域有着重要的应用。特别的，在近几年热门并快速发展的人工智能领域，矩阵理论更是机器学习和深度学习的基石。在我所从事的基于深度学习的三维人体姿态估计的研究中，在对视频和图片数据的预处理以及神经网络的搭建中运用到了大量的矩阵理论知识。本文我将围绕数据处理中涉及到的相机标定以及成像坐标变换等相关知识展开，介绍矩阵理论在我的科研研究中的应用。

研究基于视频的深度学习方法估计人体姿态，我们的数据来源于CAREN康复分析平台，数据包括围绕目标检测人员的三个角度拍摄的视频文件和记录的人体关节点上标记的21个标记点的三维空间位置坐标信息，我们的研究就是通过深度学习的方法基于视频直接输出关节点的坐标信息，而不需要借助在关节点上贴标记点的方法，如果达到足够的精度需求，便可以省去不必要的浪费在贴标记点上的时间。其中，建立视频中像素点的坐标信息与世界坐标之间的对应关系成为数据预处理部分的一项重要工作，因此我们需要完成相机的标定以及基于像素坐标的三维信息重建工作。

## 第一章 坐标转换

相机成像的过程中，涉及到了世界坐标系[Xw,Yw,Zw]、相机坐标系[Xc,Yc,Zc]、成像平面坐标系[x,y]和图像像素坐标系[u,v]之间的转换。世界坐标系，为相机定位而引入的，描述相机本身所处的位置信息；相机坐标系，以摄像机光心为原点，z轴与光轴重合即垂直与成像平面；成像平面坐标系，也称图像坐标系，坐标原点为摄像机光轴与图像物理坐标系的交点位置；像素坐标系，以像素为单位，坐标原点在左上角。下面将介绍成像过程中各个坐标之间的转换关系，了解成像的原理。

### 1.1 世界坐标系到相机坐标系

从世界坐标到相机坐标系的变换可以表示坐标系的旋转变换加上平移变换，首先考虑旋转变换，如图1-1所示，当坐标系仅绕z轴旋转θ角时，通过几何关系可得坐标变换关系：
![1.png](https://i.loli.net/2019/05/28/5cece0a78f99785531.png)

转换成矩阵表示形式，可得旋转矩阵R1，如下所示：
![2.png](https://i.loli.net/2019/05/28/5cece0a7a9d4562294.png)

![1-1.jpg](https://i.loli.net/2019/05/28/5cece2bdb444326067.jpg)  
<center>图1-1 坐标系绕z轴旋转θ角</center>

同理，可得绕x轴旋转$\phi$角和绕y轴旋转ω角的旋转矩阵R2和R3，如下所示：
![3.png](https://i.loli.net/2019/05/28/5cece0a7b3b2865129.png)

![1-2.jpg](https://i.loli.net/2019/05/28/5cece2be6a9c779701.jpg)
<center>图1-2 世界坐标系到相机坐标系的变换</center>

于是可得旋转矩阵R=R<sub>1</sub>R<sub>2</sub>R<sub>3</sub>，再考虑上平移变换矩阵T，如图1-2所示，可得到从世界坐标系到相机坐标系的变换矩阵如下：
![4.png](https://i.loli.net/2019/05/28/5cece0a7c8eb075288.png)

为方便计算，我们将其改写成如下形式：
![5.png](https://i.loli.net/2019/05/28/5cece0a7c87a562556.png)

### 1.2 相机坐标系至图像坐标系

如图1-3所示，从相机坐标系到图像坐标系，属于透视投影关系，从3D转换到2D，其中几何关系满足三角形的相似定理。
![1-3.png](https://i.loli.net/2019/05/28/5cece2bec85c625069.png)
<center>图1-3 相机坐标系到图像坐标系变换</center>

根据三角形的相似定理，得到如下变换关系：
![6.png](https://i.loli.net/2019/05/28/5cece0a7c86e055478.png)

式（7）中f为相机的焦距，Zc为相机焦点与成像平面之间的距离，将变换关系转换成矩阵表达形式，如下所示：
![7.png](https://i.loli.net/2019/05/28/5cece0a7c862490302.png)

### 1.3 图像坐标系至像素坐标系

像素在轴上的物理尺寸为dx和dy，若两坐标轴互相垂直，如图1-1所示，此时有如下变换关系：
![8.png](https://i.loli.net/2019/05/28/5cece0a7c8fe565818.png)
![1-4.jpg](https://i.loli.net/2019/05/28/5cece2bedeebf88174.jpg)
<center>图1-4 图像坐标系与像素坐标系坐标轴垂直</center>

将（9）中转换关系变成矩阵的表达形式，如下所示：
![9.png](https://i.loli.net/2019/05/28/5cece0a7c972764061.png)

![1-5.jpg](https://i.loli.net/2019/05/28/5cece2c2526f671595.jpg)
<center>图1-5 图像坐标系与像素坐标系坐标轴不垂直</center>

一般情况下，图像坐标系与像素坐标系两轴不互相垂直，如图1-2所示，此时的坐标变换如下所示：
![10.png](https://i.loli.net/2019/05/28/5cece0a7e930a96921.png)
将（11）中转换关系改写成矩阵的表达形式，如下所示：
![11.png](https://i.loli.net/2019/05/28/5cece152a8ba662688.png)
式（12）中，![111.jpg](https://i.loli.net/2019/05/28/5cece58d5f8fe28031.jpg)

### 1.4 坐标变换小结
综上所述，可知在相机成像的过程中涉及到了四个坐标系之间的坐标变换，如图1-6所示：

![1-6.jpg](https://i.loli.net/2019/05/28/5cece2c25061672497.jpg)
<center>图1-6 相机成像坐标变换关系</center>

成像过程中的坐标变换可整理成公式（13）的表达形式，如下所示：
![12.png](https://i.loli.net/2019/05/28/5cece152c39d915273.png)

通过最终的转换关系可知，一个三维空间中的坐标点，可以通过变换在图像中找到一个对应的像素点。变换矩阵中的参数可分为相机的内参和外参，得到这些参数，便可以通过已知的坐标信息求解其他坐标信息，而获取相机参数就需要完成相机标定的工作。

## 第二章 相机标定

在图像测量过程以及机器视觉应用中，为确定空间物体表面某点的三维几何位置与其在图像中对应点之间的相互关系，必须建立相机成像的几何模型，这些几何模型参数就是相机参数。在大多数条件下这些参数必须通过实验与计算才能得到，这个求解参数的过程就称之为相机标定。无论是在图像测量或者机器视觉应用中，相机参数的标定都是非常关键的环节，其标定结果的精度及算法的稳定性直接影响相机工作产生结果的准确性。因此，做好相机标定是做好后续工作的前提，提高标定精度是科研工作的重点所在。

### 2.1 相机标定原理

首先规定二维点用m=[u,v]<sup>T</sup>，三维点用M=[X,Y,Z]<sup>T</sup>表示，一般情况下，假设模型平面在世界坐标系中的Z坐标为0，根据公式（13）可知：
![13.png](https://i.loli.net/2019/05/28/5cece152bac0222544.png)

其中s为任意的比例因子，A为相机内参矩阵，ri为旋转矩阵R的第i列，t为平移矩阵。当Z为0时，点M和它在图像上的映射点m之间变换可以用单应矩阵H表示：
![14.png](https://i.loli.net/2019/05/28/5cece152aad9b66717.png)

其中m=[u,v,1]<sup>T</sup>，M=[X,Y,1]<sup>T</sup>，H=A[r1,r2,t]，显然H为3行3列的矩阵，对于标定平面的一张图片，它的单应矩阵可以估计出来，令H=[h1,h2,h3]，则有：
![15.png](https://i.loli.net/2019/05/28/5cece152b294249657.png)

式中λ是任意标量。根据旋转向量R为正交矩阵，所以有以下的性质：
![16.png](https://i.loli.net/2019/05/28/5cece152cb80430261.png)

由式（16）和式（17），便可以求解得到：
![17.png](https://i.loli.net/2019/05/28/5cece152c7b1671050.png)

式（18）中的h1，h2通过求解单应性矩阵H求解，所以未知量只剩下内参矩阵A，A中含有5个参数，如果需要完全解出来这5个未知量，则需要3个不同的单应性矩阵H，那就是需要3张不同的标定平面的照片，可以通过改变相机与标定板间的相对位置来获得不同的标定照片，从而完成内参矩阵的求解。得到内参矩阵后，便可通过公式，求解外参矩阵，得到：
![18.png](https://i.loli.net/2019/05/28/5cece152bf8b120908.png)
综上，我们便完成了相机内参与外参求解的理论部分。

### 2.2 完成相机标定

为了标定相机参数，我们需要采集相机拍摄的标准标定板视频或图片，如图2-1所示，是我手持标定板在卡伦平台上采集到的图片，分别有三个角度的摄像头采集的图像，每个角度采集不同位置下标定板的图像序列。
![2-1.jpg](https://i.loli.net/2019/05/28/5cece2c2b495159521.jpg)
<center>图2-1 卡伦平台相机标定采集图像</center>

完成图像采集后，根据相机标定的原理，我完成了相机参数的标定，具体标定是通过自己编程完成，当然Matlab和OpenCV也都提供了内置的工具箱和函数，综合了多种标定方法，能够快速且准且地完成标定。通过本次相机标定工作，我既掌握了相关的知识，同时也加深了对矩阵理论知识的理解。

## 参考文献
[1] Zhang Z . A Flexible New Technique for Camera Calibration[M]. IEEE Computer Society, 2000.