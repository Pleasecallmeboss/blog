---
title: doc2vec与聚类
date: 2022-04-08 19:14:38
tags:
---

## Word2vec

### 1.Word2Vec是用来生成词向量的工具

### 2词向量（https://zhuanlan.zhihu.com/p/81032021）

#### 	2.1字典序

​	含义：词典中的第几个字

​	![img](https://pic1.zhimg.com/80/v2-3c9a38cc5722f46beffcc10470237768_720w.jpg)

#### 	2.2one-hot编码

​	构建一个由 n个0组成的序列 (n是字典总数)，其中将第字典序个0 换成 1

​	例如：n =3，则所有的字典序对应one-hot 编码有

```text
1: 1 , 0 , 0
2: 0 , 1 , 0
3: 0 , 0 , 1
```

![img](https://pic1.zhimg.com/80/v2-e1a97fadf10aaf6e62a5f2ed807800d4_720w.jpg)

#### 	2.3深度学习的本质

​	给定大量级样本及模型（负杂函数），求解模型参数（函数解析式）![img](https://pic2.zhimg.com/80/v2-c79467c3bc8e519b67f720924d3874f1_720w.jpg)

​	简单来说深度学习，就是通过待定系数法、求导更新，最小化模型和真实世界模型的误差。其中模型就是：一大堆负杂的矩阵乘法、加法及变换

#### 	2.4embedding

​	既然已经有了 one-hot 来表达文本，那么为什么还需要词向量呢？

​	让我们做一个假设，通过 one-hot 我们可以对APP上的外卖店都编了一个号。仅通过这个编号我们可以知道每家店的口味吗？答案是否定的，因为单独的one-hot 编码仅仅只代表了一个无意义的编码。

​	人类为了对未知的事物进行描述，会通过事物的其他方向来进行类比。就像我们会将外卖按照：口味、速度、价格、好评等等维度进行打分，以此希望能够通过这些维度的评分来体现这家外卖的好坏。

​	因此，embedding可以抽象的理解为：为了让one-hot 具有更强的语义特征而设计的描述one-hot不同维度的特征。只不过这个维度和特征是通过某种形式学习得到。

#### 	2.5目标任务

​	首先，看看nlp embedding 的鼻祖之一：word2vec (2013)

![img](https://pic1.zhimg.com/80/v2-3d7e19c00358605c893e51d666327284_720w.png)

​	直观的来讲，word2vec 就是进一步在语言的算法表达上，做了一个映射任务：

​	语言 ——> 函数 ——> 目标任务

​	目标任务有两种做法 :

1. skip-gram : 给定句子中的当前词，预测周围的词
2. cbow : 给定句子中的周围词，预测当前词

![img](https://pic2.zhimg.com/80/v2-3677468e93f3ed0b1b15244a39373e51_720w.jpg)

​	其核心观点是在one-hot 的文本表征基础上，乘以一个参数矩阵，得到一个dim 长度的参数，这个就是embedding

​	因为该矩阵通过Skip-gram 或 CBOW 方式在大量语料中训练得到 ， 所以，通过w2c训练得到的embedding 也就带有了相似词相近这么一个特点。

​	因为同义词、近义词可能在大量语料上有着相同的上下文，因此，训练后embedding 也相近

> w2c 提供了一种有效的文本数值化方法，为深度学习发展提供了标准范式





w2c 的本质是通过embedding 的方式，将原有的one-hot 矩阵通过大量的语料扩展出了dim 维度的特征信息。

![img](https://pic4.zhimg.com/80/v2-564acd6b926af536a9abcc2739b5b85f_720w.jpg)

就像我们给大众店家打分一样，本来店家可能只是 某地区、第几家 店铺。

通过5个维度，给店家打了一个评分，这个评分一定意义上就能代表这个店家。



## Doc2vec

## （https://zhuanlan.zhihu.com/p/136096645）

Doc2vec可以获得句子、段落和文档的向量表达，是Word2Vec的拓展。

代码



## 层次聚类

### 1聚类（https://zhuanlan.zhihu.com/p/104355127）

`聚类(Clustering)`是按照某个特定标准(如距离)把一个数据集分割成不同的类或簇，使得**同一个簇内的数据对象的相似性尽可能大，同时不在同一个簇中的数据对象的差异性也尽可能地大**。也即聚类后同一类的数据尽可能聚集到一起，不同类数据尽量分离。



#### **1.1 聚类的一般过程**

1. 数据准备：特征标准化和降维
2. 特征选择：从最初的特征中选择最有效的特征，并将其存储在向量中
3. 特征提取：通过对选择的特征进行转换形成新的突出特征
4. 聚类：基于某种距离函数进行相似度度量，获取簇
5. 聚类结果评估：分析聚类结果，如`距离误差和(SSE)`等

#### **1.2 数据对象间的相似度度量**

对于数值型数据，可以使用下表中的相似度度量方法。

![img](https://pic3.zhimg.com/80/v2-921d5ff0b665e67d5df54047b9d6531a_720w.jpg)

`Minkowski`距离就是![[公式]](https://www.zhihu.com/equation?tex=+Lp+)范数（![[公式]](https://www.zhihu.com/equation?tex=p%E2%89%A51))，而 `Manhattan` 距离、`Euclidean`距离、`Chebyshev`距离分别对应 ![[公式]](https://www.zhihu.com/equation?tex=p%3D1%2C2%2C%E2%88%9E+)时的情形。

#### **1.3 cluster之间的相似度度量**

除了需要衡量对象之间的距离之外，有些聚类算法（如层次聚类）还需要衡量`cluster`之间的距离 ，假设![[公式]](https://www.zhihu.com/equation?tex=+C_i+)和![[公式]](https://www.zhihu.com/equation?tex=+C_j) 为两个 `cluster`，则前四种方法定义的 ![[公式]](https://www.zhihu.com/equation?tex=C_i+)和 ![[公式]](https://www.zhihu.com/equation?tex=C_j) 之间的距离如下表所示。

![img](https://pic4.zhimg.com/80/v2-efc243a0fd595089412bd617eaa0d78b_720w.jpg)

- `Single-link`定义两个`cluster`之间的距离为两个`cluster`之间距离最近的两个点之间的距离，这种方法会在聚类的过程中产生`链式效应`，即有可能会出现非常大的`cluster`
- `Complete-link`定义的是两个`cluster`之间的距离为两个``cluster`之间距离最远的两个点之间的距离，这种方法可以避免`链式效应`,对异常样本点（不符合数据集的整体分布的噪声点）却非常敏感，容易产生不合理的聚类
- `UPGMA`正好是`Single-link`和`Complete-link`方法的折中，他定义两个`cluster`之间的距离为两个`cluster`之间所有点距离的平均值
- 最后一种`WPGMA`方法计算的是两个 `cluster` 之间两个对象之间的距离的加权平均值，加权的目的是为了使两个 `cluster` 对距离的计算的影响在同一层次上，而不受 `cluster` 大小的影响，具体公式和采用的权重方案有关。



### 层次聚类

前面介绍的几种算法确实可以在较小的复杂度内获取较好的结果，但是这几种算法却存在一个`链式效应`的现象，比如：A与B相似，B与C相似，那么在聚类的时候便会将A、B、C聚合到一起，但是如果A与C不相似，就会造成聚类误差，严重的时候这个误差可以一直传递下去。为了降低`链式效应`，这时候层次聚类就该发挥作用了。

![img](https://pic3.zhimg.com/80/v2-0141f21130b60f7ac23fa05d7e417cee_720w.jpg)

**层次聚类算法 (hierarchical clustering)** 将数据集划分为一层一层的 `clusters`，后面一层生成的 `clusters` 基于前面一层的结果。层次聚类算法一般分为两类：

- **Agglomerative 层次聚类**：又称自底向上（bottom-up）的层次聚类，每一个对象最开始都是一个 `cluster`，每次按一定的准则将最相近的两个 `cluster` 合并生成一个新的 `cluster`，如此往复，直至最终所有的对象都属于一个 `cluster`。这里主要关注此类算法。
- **Divisive 层次聚类**： 又称自顶向下（top-down）的层次聚类，最开始所有的对象均属于一个 `cluster`，每次按一定的准则将某个 `cluster` 划分为多个 `cluster`，如此往复，直至每个对象均是一个 `cluster`。

![img](https://pic2.zhimg.com/80/v2-be2a7cf798fdc983e6521a68b1eb952d_720w.jpg)

另外，需指出的是，层次聚类算法是一种贪心算法（greedy algorithm），因其每一次合并或划分都是基于某种局部最优的选择。



## 层次聚类代码

（https://blog.csdn.net/elaine_bao/article/details/50242867）

scipy cluster库简介
scipy.cluster是scipy下的一个做聚类的package, 共包含了两类聚类方法:
1. 矢量量化(scipy.cluster.vq):支持vector quantization 和 k-means 聚类方法
2. 层次聚类(scipy.cluster.hierarchy):支持hierarchical clustering 和 agglomerative clustering(凝聚聚类)





# [轮廓系数](https://so.csdn.net/so/search?q=轮廓系数&spm=1001.2101.3001.7020)分析

- 对于第i i*i*个对象，计算它到所属簇中所有其他元素的平均距离，记作a i a_i*a**i*(体现凝聚度)
- 对于第 i i*i* 个对象和不包含该对象的任意簇，计算该对象到给定簇中所有对象的平均距离，记 b i b_i*b**i* （体现分离度）
- 第 i 个对象的轮廓系数为 s i = ( b i − a i ) m a x ( a i , b i ) s_i = \frac{(bi-ai)}{max(ai, bi)}*s**i*=*m**a**x*(*a**i*,*b**i*)(*b**i*−*a**i*)
