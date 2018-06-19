layout:     post
title:      deconvolution networks 
subtitle:   反卷积
date:       2018-06-01
author:     Ezra
catalog: true
tags:
    - Blog

作者：张萌
链接：https://www.zhihu.com/question/43609045/answer/120266511
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

一句话解释：逆卷积相对于卷积在神经网络结构的正向和反向传播中做相反的运算。
逆卷积(Deconvolution)比较容易引起误会，转置卷积(Transposed Convolution)是一个更为合适的叫法.
举个栗子：
4x4的输入，卷积Kernel为3x3, 没有Padding / Stride, 则输出为2x2。

输入矩阵可展开为16维向量，记作
输出矩阵可展开为4维向量，记作
卷积运算可表示为
不难想象其实就是如下的稀疏阵:

平时神经网络中的正向传播就是转换成了如上矩阵运算。
那么当反向传播时又会如何呢？首先我们已经有从更深层的网络中得到的.

![1529395223885](C:\Users\Ezra\AppData\Local\Temp\1529395223885.png)

回想第一句话，你猜的没错，所谓逆卷积其实就是正向时左乘，而反向时左乘，即的运算。
逆卷积的一个很有趣的应用是GAN(Generative Adversarial Network)里用来生成图片：Generative Models

![1529395234075](C:\Users\Ezra\AppData\Local\Temp\1529395234075.png)

----
[1603.07285] A guide to convolution arithmetic for deep learning
GitHub - vdumoulin/conv_arithmetic: A technical report on convolution arithmetic in the context of deep learning



正常卷积运算：

如上图：4x4的输入，卷积Kernel为3x3, ,输出为2x2。其计算可以理解为： 
输入矩阵展开为4*4=16维向量，记作x 
输出矩阵展开为2*2=4维向量，记作y 
卷积核C为如下矩阵： 

卷积运算可表示为y = Cx（可以对照动图理解），而卷积的反向传播可以如下，相当于乘以C^T 
反卷积运算：

如上图：2x2的输入，卷积Kernel为3x3, ,输出为4x4。如果按照正常卷积，生成的feature map尺寸应该减小，但是我们现在想要生成尺寸变大，就可以对输入进行padding。但与正常卷积padding不同的是：反卷积卷积运算的起点不一定在原feature map上，而是从padding区域开始。仔细看上面动图，可以看到第一个运算卷积核的中点在padding区域上。 
反卷积相对于卷积在神经网络结构的正向和反向传播中做相反的运算。 