---
layout: post
title:  Machine Learning之Python篇（一）
category: AI 
---

# Machine Learning之Python篇

## 概述

![](/images/article/python_for_big_data.jpg)

## 教程

https://ljalphabeta.gitbooks.io/python-/content/

《Python机器学习》中文版

https://github.com/lawlite19/MachineLearning_Python

东南大学某研究生的github，包含大量ML算法示例。

https://github.com/lawlite19/DeepLearning_Python

上个哥们的DL示例

http://www.jianshu.com/p/eb9e3be977ed

Python数据分析之武林秘籍。这里包括了大量ML或DL的python工具包。

https://chrisalbon.com/

Chris Albon的个人主页。包含大量ML/DL相关的note。

>Chris Albon, UC Davis博士。在肯尼亚工作多年，援非的IT人。著有《Machine Learning with Python Cookbook: Practical Solutions from Preprocessing to Deep Learning》。

https://store.chrisalbon.com/

Chris Albon写的ML卡片书。

## Anaconda

Anaconda是一个科学计算方面的python发行版，下面提到的所有工具都可以通过Anaconda一站式安装。

官网：

https://www.anaconda.com/download/

基本命令：

`conda update --all`

`conda update conda`

`conda update anaconda`

Anaconda同时也支持多个Python版本的并存和切换。它的底层用到了virtualenv。这个功能在Ubuntu下意义不太大，因为后者的apt工具已经维护好了2.x和3.x两个分支，除非你想同时支持两个不同的3.x分支。但在Windows下，由于包管理器的缺位，这个问题是很难解决的。

创建一个virtualenv：

`conda create -n python2 python=2.7`

这条命令会在Anaconda/envs下创建一个python2文件夹。

将命令行环境切换到该版本：

`activate python2`

参考：

http://www.cnblogs.com/zhusleep/p/5616099.html

Anaconda安装更新库

https://segmentfault.com/a/1190000004020387

Python多版本切换工具-Pyenv\virtualenv及Anaconda科学计算环境的配置

http://www.jianshu.com/p/d2e15200ee9b

Anaconda多环境多版本python配置指导

## NumPy

NumPy是python语言所有数学计算库的基础。它主要提供了矩阵运算的功能。

官网：

http://www.numpy.org/

教程：

https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

API参考：

https://docs.scipy.org/doc/numpy/reference/

quickstart中文版：

https://mp.weixin.qq.com/s/q0jSrxRRQAB7FUUqgD5H6A

### 文件存取

原始二进制文件：tofile()和fromfile()

NumPy专用的格式文件（.npy或.npz，它和原始二进制文件的区别在于：前者包含维度和类型信息，而后者只有数据本身）：save()和load()

文本文件：savetxt()和loadtxt()

参考：

http://www.cnblogs.com/dmir/p/5009075.html

python:numpy（文件存取）

### 数据类型转换

类型转换：`c = b.astype(int)`

把A类型看做B类型，比如将一个float64的数，看做8个单字节的数：`a.dtype = 'int8'`

参考：

http://www.cnblogs.com/hhh5460/p/5129032.html

numpy数据类型dtype转换

### 参考

https://mp.weixin.qq.com/s/FVI3zEp4it-fd99-3MU9vA

从数组到矩阵的迹，NumPy常见使用大总结

https://www.machinelearningplus.com/101-numpy-exercises-python/

101 NumPy Exercises for Data Analysis。这里包含了101个和numpy有关的问题，并附有答案。

https://mp.weixin.qq.com/s/DiFTV29gvMlosoy2HaPQjw

用Python做图像处理（3）

## SciPy

SciPy提供了一些更高阶的数学运算库，比如：积分、插值、信号处理、傅立叶变换、矩阵特征值、统计计算等。

SciPy提供的功能主要仍局限于数学运算，而并未提升到算法的层面。这也是它和scikit-learn或其他高级库的差别所在。

官网：

http://www.scipy.org/

API参考：

https://docs.scipy.org/doc/scipy/reference/

## Scikit-learn

Scikit-learn提供了常见的机器学习算法的实现。

官网：

http://scikit-learn.org/stable/index.html

教程：

http://scikit-learn.org/stable/user_guide.html

API参考：

http://scikit-learn.org/stable/tutorial/index.html

http://scikit-learn.org/stable/modules/classes.html

中文文档：

http://sklearn.apachecn.org

## Matplotlib

Matplotlib是一个高阶的图形库，主要提供生成图表等数据可视化方面的功能。

官网：

http://matplotlib.org/

API参考：

http://matplotlib.org/1.5.3/api/index.html

参考：

https://mp.weixin.qq.com/s/mpj1QpWpnGm8117p3cEWZw

如何优雅而高效地使用Matplotlib实现数据可视化

https://mp.weixin.qq.com/s/LBrlXEhGYOx1aPFZzQcyTQ

5种快速易用的Python Matplotlib数据可视化方法

https://mp.weixin.qq.com/s/aBi1PTEumRs0frUpb_uYrA

用Python做图像处理（2）

https://mp.weixin.qq.com/s/3VgFKiUOFvtWmqg1BO9xGw

matplotlib--python的数据可视化

## Pandas

Pandas是一个数据分析方面的工具库。它提供的Series(1-dimensional)和DataFrame(2-dimensional)数据结构，可以提供类似sql的数据操作和查询的功能。

官网：

http://pandas.pydata.org/

文档：

http://pandas.pydata.org/pandas-docs/stable/

API参考：

http://pandas.pydata.org/pandas-docs/stable/api.html

参考：

http://www.cnblogs.com/chaosimple/p/4153083.html

十分钟搞定pandas

http://pandas.pydata.org/pandas-docs/stable/comparison_with_sql.html

Pandas和SQL的比较

https://mp.weixin.qq.com/s/XIQ5EpQcYLxmRBuaTCZFzg

一行代码，Pandas秒变分布式，快速处理TB级数据

https://mp.weixin.qq.com/s/fXI5suCVna6fBxPnVyKevw

浅谈NumPy和Pandas库

https://mp.weixin.qq.com/s/1xHeXs9gM3LzEzCUz0jhXA

简单实用的pandas技巧：如何将内存占用降低90%

http://wiki.jikexueyuan.com/project/start-learning-python/311.html

python科学计算之Pandas使用

## mysql

http://www.runoob.com/python/python-mysql.html

python操作mysql数据库

## chainer

chainer是一个日本公司Preferred Networks写的基于python的深度学习框架。

官网：

https://chainer.org/

代码：

https://github.com/chainer/chainer

Preferred Networks是日本目前最强的AI创业公司，估值已经超过20亿美元。在工业机器人领域具有很强的实力。

它推出的PaintsChainer是一个给黑白线稿上色的App。

官网：

https://github.com/pfnet/PaintsChainer

## iPython

ipython是一个python的交互式 shell，比默认的python shell 好用得多，支持变量自动补全，自动缩进，支持 bash shell 命令，内置了许多很有用的功能和函数。

在较新的ipython版本中，添加了ipython notebook的功能，弥补了ipython shell下代码不易保存等缺点，并且在使用--pylab inline选项后，可以在代码执行后立即显示运行结果（包括图片，数据表格等），因此在数据分析中运用十分广泛。

`sudo apt-get install ipython ipython-notebook`

## Jupyter

Jupyter是iPython的后继项目，它不仅支持python语言，还支持其他50多种交互式语言。成为目前最流行的交互式shell和数据文本交换格式。

官网：

https://jupyter.org/

`pip install jupyter`

参见：

https://mp.weixin.qq.com/s/UXlPhX3Vb2yqocpUH_3W5w

Jupyter项目的前世今生

https://mp.weixin.qq.com/s/aJRVh7BWOMq4KCoBMtLGGw

快速学习Jupyter Notebook

## TuShare

TuShare是一个免费、开源的python财经数据接口包。主要实现对股票等金融数据从数据采集、清洗加工 到 数据存储的过程，能够为金融分析人员提供快速、整洁、和多样的便于分析的数据，为他们在数据获取方面极大地减轻工作量，使他们更加专注于策略和模型的研究与实现上。

官网：

http://tushare.org/

## skimage

skimage相当于python版本的OpenCV。

官网：

http://scikit-image.org/

参考：

https://buptldy.github.io/2016/03/31/2016-03-31-Skimage%20hog/

Compute the HOG descriptor by skimage

## Numba

NumPy的一个高速版本，能完成前者大部分的功能。

官网：

http://numba.pydata.org/

参考：

https://mp.weixin.qq.com/s/5peMiGXNnviAjMP76tV3pw

Python高性能计算库——Numba

## Face Recognition

这是一个人脸识别的软件包。

代码：

https://github.com/ageitgey/face_recognition

参考：

https://mp.weixin.qq.com/s/QcMsSsZY7TcNQ3G57p8eyQ

3行Python代码完成人脸识别

## Luminoth

Luminoth是一个开源的计算机视觉工具包，目前支持目标探测和图像分类，但以后会有更多的扩展。该工具包在TensorFlow和Sonnet上用Python搭建而成。

代码：

https://github.com/tryolabs/luminoth

## OpenFace

OpenFace是一款开源的人脸识别软件。它的原理基于CVPR 2015年的论文：FaceNet。由于采用了深度学习技术，OpenFace对人脸识别的准确率，大大超过了OpenCV。

OpenFace是用Python和Torch编写的。

官网：

https://cmusatyalab.github.io/openface/

## Tangent

Tangent是一个用于自动微分的源到源Python库。

官网：

https://github.com/google/tangent

参考：

https://mp.weixin.qq.com/s/CExdIdtUxZi4c2B39xdYJA

谷歌开源Tangent

## LibROSA

LibROSA是一个分析音乐和语音的库。

官网：

http://librosa.github.io/

## PIL

PIL：Python Imaging Library，已经是Python平台事实上的图像处理标准库了。PIL功能非常强大，但API却非常简单易用。

官网：

http://pythonware.com/products/pil/

安装：

`sudo apt install python-imaging`

文档：

http://www.effbot.org/imagingbook/pil-index.htm

参考：

https://mp.weixin.qq.com/s/lJYzxehwo3K2APB2l_WeTA

用Python做图像处理（1）

## VisPy

VisPy可以算的上是Matplotlib的威力加强版，它添加了对GPU、3D和大数据的支持。

官网：

http://vispy.org/

参考：

https://mp.weixin.qq.com/s/yduK_XKQ-qhiYTNl-YZMFg

利用Python实现卷积神经网络的可视化

## Seaborn

Seaborn是另一个非常棒的Matplotlib的威力加强版，专注于统计绘图，并可无缝对接Pandas库。

官网：

https://seaborn.pydata.org

参考：

https://mp.weixin.qq.com/s/hPbTZHR2iDH4t1eQghNvUQ

数据可视化详解+代码演练

## mlpy

mlpy是一个开源的ML库。只是它最近的一次更新，已经是2012年的事情了。

官网：

http://mlpy.sourceforge.net

## Pyecharts

pyecharts是一个用于生成Echarts图表的类库。Echarts是百度开源的一个数据可视化JS库。

官网：

http://pyecharts.org/

参考：

https://mp.weixin.qq.com/s/3T594c4DzuRmPW5A4zu8Dg

Pyecharts：极其强大的Python数据可视化模块

## ImageAI

ImageAI是一个CV方面的库，集成了大量的DL模型，其目标是使用数十行代码完成一个CV任务。

代码：

https://github.com/OlafenwaMoses/ImageAI

## 参考

https://mp.weixin.qq.com/s/S7gnAdekPFcyd4ni1m9phg

GitHub最著名的20个Python机器学习项目，值得收藏！

https://github.com/neozhaoliang/pywonderland/tree/master/

如何用Python画各种著名数学图案

http://old.sebug.net/paper/books/scipydoc/index.html

用Python做科学计算

https://pan.baidu.com/s/1qYPUHNQ

小抄合集

https://mp.weixin.qq.com/s/1--i1OhxRNPvNnmP9bFo3g

全机器学习和Python的27个速查表

https://mp.weixin.qq.com/s/8wI49YBAs7ckkYdc4B_WNQ

14张思维导图读懂Python编程核心知识体系

https://mp.weixin.qq.com/s/HKHLiCqEAtoCAqf4CHv86w

python机器学习算法代码实现

