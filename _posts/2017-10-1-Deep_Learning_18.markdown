---
layout: post
title:  深度学习（十八）——YOLOv2, 语义分割
category: DL 
---

# YOLOv2（续）

### Direct location prediction

使用anchor boxes的另一个问题是模型不稳定，尤其是在早期迭代的时候。大部分的不稳定现象出现在预测box的(x,y)坐标时。

究其原因在于，虽然RPN会预测坐标的修正值$$(\Delta x, \Delta y)$$，然而却未对$$\Delta x, \Delta y$$的取值范围做限定。因此，可能会出现anchor检测很远的目标box的情况，效率比较低。

正确做法应该是每一个anchor只负责检测周围正负一个单位以内的目标box。

### Fine-Grained Features

修改后的网络最终在13x13的特征图上进行预测，虽然这足以胜任大尺度物体的检测，但如果用上细粒度特征的话可能对小尺度的物体检测有帮助。

Faser R-CNN和SSD都在不同层次的特征图上产生区域建议以获得多尺度的适应性。

YOLOv2使用了一种不同的方法，简单添加一个passthrough layer，把浅层特征图（分辨率为26x26）连接到深层特征图。

具体操作如下：

1.叠加相邻空间位置的特征到不同通道，将26x26x512的特征图叠加成13x13x2048的特征图。

2.将浅层特征图（13x13x2048）和深层特征图（13x13x1024）合并为一个（13x13x3072）tensor。

### Multi-Scale Training

为了让YOLOv2对不同尺寸图片具有鲁棒性，在训练的时候就要考虑这一点。

每经过10批训练（10 batches）就会随机选择新的图片尺寸。网络使用的降采样参数为32，于是使用32的倍数{320,352，…，608}，最小的尺寸为320x320，最大的尺寸为608x608。调整网络到相应维度然后继续进行训练。

>一般来说，神经网络各层的tensor尺寸在训练过程中是不能随意修改的，因为修改之后参数的个数会发生变化，那就不是同一个model了。然而，有些操作是例外的。比如pooling，这个操作只有规则没有参数，因此可以很方便适应不同的tensor尺寸。再比如卷积，不同尺寸的图片，实际上也可以用相同大小的kernel进行卷积操作，只要kernel的参数可以在训练过程中共享，这个训练过程就是合理的。而YOLOv2正好只有卷积和pooling两种操作。

## Faster

### Darknet-19

YOLOv2使用了一个新的分类网络作为特征提取部分，参考了前人的先进经验，比如类似于VGG，作者使用了较多的3x3卷积核，在每一次池化操作后把通道数翻倍。

借鉴了network in network的思想，网络使用了全局平均池化（global average pooling），把1x1的卷积核置于3x3的卷积核之间，用来压缩特征。也用了batch normalization（前面介绍过）稳定模型训练。

最终得出的基础模型就是Darknet-19，其包含19个卷积层、5个最大值池化层（maxpooling layers ）。如下图：

![](/images/article/Darknet_19.png)

Darknet-19的运算量为55.8亿次浮点数运算。VGG-16为306.9亿次，而YOLO为85.2亿次。

## Stronger

### 联合训练

作者提出了一种在分类数据集和检测数据集上联合训练的机制：

1.使用检测数据集的图片去学习检测相关的信息，例如bounding box坐标预测，是否包含物体以及属于各个物体的概率。

2.使用仅有类别标签的分类数据集图片去扩展可以检测的种类。

这种方法有一些难点需要解决。检测数据集只有常见物体和抽象标签（不具体），例如 “狗”，“船”。分类数据集拥有广而深的标签范围（例如ImageNet就有一百多类狗的品种，包括 “Norfolk terrier”, “Yorkshire terrier”, and “Bedlington terrier”等. ）。必须按照某种一致的方式来整合两类标签。

大多数分类的方法采用softmax层，考虑所有可能的种类计算最终的概率分布。但是softmax假设类别之间互不包含，但是整合之后的数据是类别是有包含关系的，例如 “Norfolk terrier” 和 “dog”。所以整合数据集没法使用这种方式（softmax 模型），

作者最后采用一种不要求互不包含的多标签模型（multi-label model）来整合数据集。

### Hierarchical classiﬁcation（层次式分类）

ImageNet的标签参考WordNet（一种结构化概念及概念之间关系的语言数据库）。例如：

![](/images/article/WordNet.png)

很多分类数据集采用扁平化的标签。而整合数据集则需要结构化标签。

WordNet是一个有向图结构（而非树结构），因为语言是复杂的（例如“dog”既是“canine”又是“domestic animal”），为了简化问题，作者从ImageNet的概念中构建了一个层次树结构（hierarchical tree）来代替图结构方案。这也就是作者论文中提到的WordTree。

WordTree的细节，更偏NLP一些，这里不再赘述。

## 参考

https://zhuanlan.zhihu.com/p/25167153

YOLO2

http://blog.csdn.net/jesse_mx/article/details/53925356

YOLOv2论文笔记

http://lanbing510.info/2017/09/04/YOLOV2.html

目标检测之YOLOv2

https://mp.weixin.qq.com/s/r2mfq0UkljKce8_dYrCNEw

YOLOv2原理与实现

# YOLOv3

https://zhuanlan.zhihu.com/p/34945787

YOLOv3：An Incremental Improvement全文翻译

https://mp.weixin.qq.com/s/UWuCiV6dBk9Z0XusEBS6-g

物体检测经典模型YOLO新升级，就看一眼，速度提升3倍！

https://mp.weixin.qq.com/s/BF7mj-_D303cLiD5e6oy6w

期待已久的—YOLO V3

https://mp.weixin.qq.com/s/-Ixqxx6QGJDTM4g50y6LyA

进击的YOLOv3，目标检测网络的巅峰之作

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## R-FCN

FCN在目标检测领域的应用。

http://blog.csdn.net/zijin0802034/article/details/53411041

R-FCN: Object Detection via Region-based Fully Convolutional Networks

https://blog.csdn.net/App_12062011/article/details/79737363

R-FCN

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

# 语义分割

Semantic segmentation是图像理解的基石性技术，在自动驾驶系统（具体为街景识别与理解）、无人机应用（着陆点判断）以及穿戴式设备应用中举足轻重。

我们都知道，图像是由许多像素（Pixel）组成，而“语义分割”顾名思义就是将像素按照图像中表达语义含义的不同进行分组（Grouping）/分割（Segmentation）。

![](/images/article/image_enet.png)

上图是语义分割网络ENet的实际效果图。其中，左图为原始图像，右图为分割任务的真实标记（Ground truth）。

显然，在图像语义分割任务中，其输入为一张HxWx3的三通道彩色图像，输出则是对应的一个HxW矩阵，矩阵的每一个元素表明了原图中对应位置像素所表示的语义类别（Semantic label）。

因此，图像语义分割也称为“图像语义标注”（Image semantic labeling）、“像素语义标注”（Semantic pixel labeling）或“像素语义分组”（Semantic pixel grouping）。

由于图像语义分割不仅要识别出对象，还要标出每个对象的边界。因此，与分类目的不同，相关模型要具有像素级的密集预测能力。

目前用于语义分割研究的两个最重要数据集是PASCAL VOC和MSCOCO。

参考：

https://zhuanlan.zhihu.com/p/21824299

从特斯拉到计算机视觉之“图像语义分割”

https://zhuanlan.zhihu.com/SemanticSegmentation

一个语义分割的专栏

https://zhuanlan.zhihu.com/p/22308032

图像语义分割之FCN和CRF

https://zhuanlan.zhihu.com/p/25515361

图像语义分割之特征整合和结构预测

https://zhuanlan.zhihu.com/p/27794982

语义分割中的深度学习方法全解：从FCN、SegNet到各代DeepLab

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://mp.weixin.qq.com/s/4BvvwV11f9MrrYyLwUrX9w

还在用ps抠图抠瞎眼？机器学习通用背景去除产品诞生记

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/mQqEe4LC0VHBH2ZAtFanWQ

基于深度学习的图像语义分割方法回顾

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/M1Oo4ST2aspgZF8UeSUDww

如何妙笔勾檀妆：像素级语义理解

https://mp.weixin.qq.com/s/xalo2XtKtzR5tA_dPFzaJw

一文介绍3篇无需Proposal的实例分割论文

https://mp.weixin.qq.com/s/BL1xZ_YuuPe9frIc9E1fkA

南开大学提出新物体分割评价指标

https://mp.weixin.qq.com/s/3rfZUhio4Bk1RUGkEk5xoQ

ETH Zurich提出新型网络“ROAD-Net”，解决语义分割域适配问题

https://mp.weixin.qq.com/s/qMLCi-CghxvTcwyPnvFxnQ

ConvCRF：一种结合条件随机场与CNN的高效语义分割方法

https://mp.weixin.qq.com/s/JbdwtpA3iRXReyerO4HYIg

一文了解什么是语义分割及常用的语义分割方法有哪些

https://mp.weixin.qq.com/s/jCv259hI0vl7st80Obfrcg

图像语义分割的工作原理和CNN架构变迁

https://mp.weixin.qq.com/s/deepxMWCpIEe3jk_kanfMg

金字塔注意力网络：一种利用底层像素与高级特征的语义分割网络

# 前DL时代的语义分割

从最简单的像素级别“阈值法”（Thresholding methods）、基于像素聚类的分割方法（Clustering-based segmentation methods）到“图划分”的分割方法（Graph partitioning segmentation methods），在DL“一统江湖”之前，图像语义分割方面的工作可谓“百花齐放”。在此，我们仅以“Normalized cut”和“Grab cut”这两个基于图划分的经典分割方法为例，介绍一下前DL时代语义分割方面的研究。

## Normalized cut

Normalized cut （N-cut）方法是基于图划分（Graph partitioning）的语义分割方法中最著名的方法之一，于2000年Jianbo Shi和Jitendra Malik发表于相关领域顶级期刊TPAMI。

通常，传统基于图划分的语义分割方法都是将图像抽象为图（Graph）的形式$$\bf{G}=(\bf{V},\bf{E})$$（$$\bf{V}$$为图节点，$$\bf{E}$$为图的边），然后借助图理论（Graph theory）中的理论和算法进行图像的语义分割。

常用的方法为经典的最小割算法（Min-cut algorithm）。不过，在边的权重计算时，经典min-cut算法只考虑了局部信息。如下图所示，以二分图为例（将$$\bf{G}$$分为不相交的$$\bf{A},\bf{B}$$两部分），若只考虑局部信息，那么分离出一个点显然是一个min-cut，因此图划分的结果便是类似$$n_1$$或$$n_2$$这样离群点，而从全局来看，实际想分成的组却是左右两大部分。

![](/images/article/N_cut.jpg)
