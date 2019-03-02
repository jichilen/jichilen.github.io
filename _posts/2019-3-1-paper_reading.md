---
layout: post
title:  "Synthetically Supervised Feature Learning for Scene Text Recognition"
date:   2019-3-1
desc: "Synthetically Supervised Feature Learning for Scene Text Recognition"
keywords: "python"
categories: [Html]
tags: [reading]
icon: icon-html
---



###    [Synthetically Supervised Feature Learning for Scene Text Recognition](https://link.springer.com/chapter/10.1007%2F978-3-030-01228-1_27)

<p style="font-size: 14px">Yang Liu, Zhaowen Wang, Hailin Jin and Ian Wassell </p>

<p style="font-size: 12px">Computer Laboratory, University of Cambridge, UK</p>

 对于识别过程来说，生成数据集有着很大的作用，即使没有真实图片，用生成数据集也可以取得很好的泛化能力，其主要原因是识别过程的图片相对简单，只有很小的扭曲形变。

 所以一般来说绝大多数的识别算法都是利用大规模生成的图片进行训练，忽略了生成图片的过程，虚拟图片和真实图片的一个很大的不同就在于我们可以得到虚拟图片**生成的参数**，这可以认为是额外的gt，或者说我们得到的图片是没有干扰的

 这篇文章将**图像生成过程**加入到识别网络中来，这样就可以运用**生成判别模型**，在识别的时候利用没有经过扭曲变换的原图也加入训练过程中作为监督信息

<img src="../assets/img/paper-1_1.jpg" style="zoom:100%">

整体的网络结构如上图，可以看到是一个典型的GAN，不过有两个判别器

给定一个`y="ANODIZING"`经过两个不同分支分别产生一个synthetic image和一个clean image，然后通过encoder产生$f$和$\hat f$，$f$分别经过text decoder T和image generator G，生成$\hat x$以及$\hat y$至此整个前向过程结束，网络产生3个loss，分别是$(\hat y,y),(\hat x,\bar x),(\hat f,f)$

对encoder的要求服从两个原则，第一个是不变，这要求E对$(\hat x,\bar x)$产生的特征是一样的，第二个原则是完备，即存在一个生成器能够从特征生成回原图。

这两个原则看起来就很有道理，然后可以很好的把图片生成的过程利用起来，使网络学习到不带特殊变形的参数，同时可以得到一个很好的图像识别模型

这个应该是第一个在文字识别中做GAN的工作（作者强调），并且可以对变形文字有着很好的效果

#### Renderer R

R用于从文字生成带文字的图片，根据参数的不同可以生成带干扰因素的图片以及干净的图片。由于是用于识别的文字图片，只需要一个很小的背景就可以了，所以这个R可以用一个很简单的算法实现。R是不可训练的

#### Encoder and Text Decoder

E和D一起组成了一个常规的文字识别网络，这一部分的实现是按照CRNN来的，简单的来说CRNN是一个利用CNN层提取图像特征然后使用RNN层进行解码的过程，因为RNN的输出结果是一个序列，这正和我们最后需要得到的结果是一致的。但是由于RNN的输出是定长的，而我们的输出是变长的，所以我们需要进行进一步的解码，这个时候一个CTC的结构就显得很重要。

<img src="..\assets\img\paper-1_2.jpg" style="zoom:60%">

CTC的主要思想来源于对未对齐的序列进行监督，主要是寻找相邻序列中的峰值，使得峰值的序列和目标序列匹配的过程。所以CTC并不是寻找理论上最优解的过程，他只需要使得所有可能序列的概率之和达到最大即可。
$$
max_wp(z|y=N_w(x))=p(z|x)=\sum_{B(\pi)=z}P(\pi|x)
$$
CTC的优化目标为上式，z表示目标序列，y表示RNN或者GNN的输出序列，$\pi$表示输出序列，B表示CTC中的变换。所有可能的序列$B(\pi)=z$如下图所示

<img src="..\assets\img\paper-1_3.jpg" style="zoom:60%">

该过程和马尔科夫链非常近似，所以上式的梯度求解也用到了前向后向算法

#### Feature matching and image generator

根据前面所提到的，一个优秀的E和G必须满足不变性和完备性。所以有两个新的监督信息
$$
\min\limits_E L_f=||E(x)-E(\bar x)||_2
\\
\min\limits_{EG} L_g=||G(E(x))-\bar x||_1
$$

#### Adversarial discriminators

同时借助了生成对抗思想来进一步指导$(E(x),E(\bar x)),(G(E(x),\bar x))$的学习
$$
\min\limits_{E,D}\max\limits_{D_I}L_{ga}=\log D_I(\bar x|x)+log(1-D_I(G(E(x))|x))
\\
\min\limits_E\max\limits_{D_F} L_{fa}=\log D_F(E(\bar x))+\log (1-D_F(E(x)))
$$

### 实验

Synth90作为训练集

使用不同种类的clean image作为监督信息

img size ：32 × 100

intensities： [-1,1]

batch size: 32

#### Ablation Study

修改clean image的类型以及网络的组成部分

### 新词汇

|                                          |              |
| :--------------------------------------- | ------------ |
| leverage                                 | 利用         |
| supervision                              | 监督         |
| because of the absence of \|  is free of | 缺少         |
| hand-crafted                             | 手工         |
| manipulate nuisance factors              | 操纵干扰因素 |
| distortion \| distortion                 |              |
| end-to-end fashion                       |              |
| aforementioned                           |              |
| auxiliary                                | 辅助的       |
| deskewed                                 | 歪斜的       |





