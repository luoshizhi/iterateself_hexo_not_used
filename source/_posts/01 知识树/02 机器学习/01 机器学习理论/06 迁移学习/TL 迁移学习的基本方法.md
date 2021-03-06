---
title: TL 迁移学习的基本方法
toc: true
date: 2018-07-27 15:08:28
---
---
author: evo
comments: true
date: 2018-05-20 15:10:10+00:00
layout: post
link: http://106.15.37.116/2018/05/20/tl-%e8%bf%81%e7%a7%bb%e5%ad%a6%e4%b9%a0%e7%9a%84%e5%9f%ba%e6%9c%ac%e6%96%b9%e6%b3%95/
slug: tl-%e8%bf%81%e7%a7%bb%e5%ad%a6%e4%b9%a0%e7%9a%84%e5%9f%ba%e6%9c%ac%e6%96%b9%e6%b3%95
title: TL 迁移学习的基本方法
wordpress_id: 6126
categories:
- 人工智能学习
tags:
- Transfer Learning
---

<!-- more -->

[mathjax]

**注：非原创，只是按照自己的思路做了整合，修改。推荐直接看 ORIGINAL 中所列的原文。**


# ORIGINAL






  1. [迁移学习简明手册](https://github.com/jindongwang/transferlearning-tutorial)  [王晋东](https://zhuanlan.zhihu.com/p/35352154)




# TODO






  * aaa





* * *





# INTRODUCTION






  * aaa





# 迁移学习的基本方法


迁移学习的基本方法可以分为四种：




  * 基于样本的迁移


  * 基于模型的迁移


  * 基于特征的迁移


  * 基于关系的迁移


本部分简要叙述各种方法的基本原理和代表性相关工作。基于特征和模型的迁移方法是我们的重点。后续将会更加深入地讨论和分析。


## 基于样本的迁移  ( Instance based Transfer Learning )


基于样本的迁移学习方法根据一定的权重生成规则，对数据样本进行重用，来进行迁移学习。

下图形象地表示了基于样本迁移方法的思想：

源域中存在不同种类的动物，如狗、鸟、猫等，目标域只有狗这一种类别。在迁移时，为了最大限度地和目标域相似，我们可以人为地提高 源域 中属于狗这个类别的样本权重。**嗯，刚想写个为什么，想了下，感觉的确是这么回事。提高样本权重指的是什么？提高狗的样本个数吗？提升到多少合适？把目标领域全部包括进去？这个真的可行吗？不会使模型不平衡吗？而且目标域只有狗是什么意思？是分类狗的品种吗？如果权重是一个值的话怎么放到训练模型中的？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/1B5Hl3780c.png?imageslim)

在迁移学习中，对于源域 \(\mathcal{D}_s\) 和目标域 \(\mathcal{D}_t\) ，通常假定产生它们的概率分布是不同且未知的( \(P(\mathbf{x}_s) \ne P(\mathbf{x}_t)\) )。

由于实例的维度和数量通常都非常大，因此，直接对 \(P(\mathbf{x}_s)\) 和 \(P(\mathbf{x}_t)\) 进行估计是不可行的。**什么叫进行估计？**

因而，大量的研究工作都着眼于对源域和目标域的分布比值进行估计( \(P(\mathbf{x}_t)/P(\mathbf{x}_s)\) )。所估计得到的比值即为样本的权重。**什么是分布比值进行估计？怎么作为样本的权重的？ **这些方法通常都假设 \(\frac{P(\mathbf{x}_t)}{P(\mathbf{x}_s)}<\infty\) 并且源域和目标域的条件概率分布相同( \(P(y|\mathbf{x}_s)=P(y|\mathbf{x}_t)\) )。特别的，有：




  * TrAdaboost 方法，将 AdaBoost 的思想应用于迁移学习中，提高有利于目标分类任务的实例权重、降低不利于目标分类任务的实例权重，并基于PAC 理论推导了模型的泛化误差上界。TrAdaBoost 方法是此方面的经典研究之一。[note] Boosting for transfer learning [/note] **后面有介绍这个吗？**


  * 核均值匹配方法 (Kernel Mean Matching, KMM) 对于概率分布进行估计，目标是使得加权后的源域和目标域的概率分布尽可能相近。[note] Correcting sample selection bias by unlabeled data  [/note] **后面有介绍吗？**


  * 传递迁移学习方法 (Transitive Transfer Learning, TTL) 和远域迁移学习 (Distant Domain Transfer Learning, DDTL)，扩展了实例迁移学习方法的应用场景，利用联合矩阵分解和深度神经网络，将迁移学习应用于多个不相似的领域之间的知识共享，取得了良好的效果。[note] Transitive transfer learning [/note] [note] Distant domain transfer learning  [/note] **这两个后面有介绍吗？**


虽然实例权重法具有这些优点：


  * 较好的理论支撑


  * 容易推导泛化误差上界 **什么叫容易推导泛化误差上界？怎么推导？**


但是他们缺点比较大：


  * 这类方法通常只在领域间分布差异较小时有效，因此对自然语言处理、计算机视觉等任务效果并不理想。


而对于这些任务，基于特征表示的迁移学习方法效果更好，因此基于特征迁移是我们的重点：


## 基于特征迁移


基于特征的迁移方法(Feature based Transfer Learning)是指将




  * 通过特征变换的方式互相迁移，来减少源域和目标域之间的差距  **什么意思？**


  * 或者将源域和目标域的数据特征变换到统一特征空间中，然后利用传统的机器学习方法进行分类识别。 **怎么变换到统一的特征空间中的？**


根据特征的同构和异构性，又可以分为同构和异构迁移学习。下图很形象地表示了两种基于特征的迁移学习方法：**什么是特征的同构与异构性？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/K3cEj9GDDm.png?imageslim)

基于特征的迁移学习方法是迁移学习领域中最热门的研究方法，这类方法通常假设源域和目标域间有一些交叉的特征。举几个例子：**什么是交叉的特征？ 这几个例子所讲的方法都要总结下。**




  * 香港科技大学的Pan等人~\cite{pan2011domain}提出的迁移成分分析方法(Transfer Component Analysis, TCA)是其中较为典型的一个方法。该方法的核心内容是以最大均值差异(Maximum Mean Discrepancy, MMD)~\cite{borgwardt2006integrating}作为度量准则，将不同数据领域中的分布差异最小化。


  * 加州大学伯克利分校的Blitzer等人~\cite{blitzer2006domain}提出了一种基于结构对应的学习方法(Structural Corresponding Learning, SCL)，该算法可以通过映射将一个空间中独有的一些特征变换到其他所有空间中的轴特征上，然后在该特征上使用机器学习的算法进行分类预测。


  * 清华大学龙明盛等人~\cite{long2014transfer}提出在最小化分布距离的同时，加入实例选择的迁移联合匹配(Tranfer Joint Matching, TJM)方法，将实例和特征迁移学习方法进行了有机的结合。


  * 澳大利亚卧龙岗大学的Jing Zhang等人~\cite{zhang2017joint}提出对于源域和目标域各自训练不同的变换矩阵，从而达到迁移学习的目标。


近年来，基于特征的迁移学习方法大多与神经网络进行结合，在神经网络的训练中进行学习特征和模型的迁移。后面我们会系统的介绍。


## 基于模型迁移


基于模型的迁移方法 (Parameter/Model based Transfer Learning) 是指从源域和目标域中找到他们之间共享的参数信息，以实现迁移的方法。这种迁移方式要求的假设条件是：源域中的数据与目标域中的数据可以共享一些模型的参数。

下图形象地表示了基于模型的迁移学习方法的基本思想：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/eH8lfKh3cG.png?imageslim)

下面介绍几种代表性的工作：**都要总结下**




  * 中科院计算所的Zhao等人 ~\cite{zhao2011cross} 提出了 TransEMDT 方法。该方法首先针对已有标记的数据，利用决策树构建鲁棒性的行为识别模型，然后针对无标定数据，利用 K-Means 聚类方法寻找最优化的标定参数。


  * 西安邮电大学的Deng等人 ~\cite{deng2014cross} 也用超限学习机做了类似的工作。


  * 香港科技大学的Pan等人 ~\cite{pan2008transfer} 利用 HMM ，针对 Wifi 室内定位在不同设备、不同时间和不同空间下动态变化的特点，进行不同分布下的室内定位研究。


  * 另一部分研究人员对支持向量机 SVM 进行了改进研究~\cite{nater2011transferring,li2012cross}。这些方法假定SVM中的权重向量 \(\mathbf{w}\) 可以分成两个部分： \(\mathbf{w}=\mathbf{w_o}+\mathbf{v}\) ，其中 \(\mathbf{w}_0\) 代表源域和目标域的共享部分， \(\mathbf{v}\) 代表了对于不同领域的特定处理。


  * 在最新的研究成果中，香港科技大学的Wei等人~\cite{wei2016instilling}将社交信息加入迁移学习方法的正则项中，对方法进行了改进。


  * 清华大学龙明盛等人~\cite{long2015learning,long2016deep,long2017deep}改进了深度网络结构，通过在网络中加入概率分布适配层，进一步提高了深度迁移学习网络对于大数据的泛化能力。


通过对现有工作的调研可以发现，目前绝大多数基于模型的迁移学习方法都与深度神经网络进行结合。这些方法对现有的一些神经网络结构进行修改，在网络中加入领域适配层，然后联合进行训练。因此，这些方法也可以看作是基于模型、特征的方法的结合。**什么是领域适配层？**


## 基于关系迁移


基于关系的迁移学习方法 (Relation Based Transfer Learning) 与上述三种方法具有截然不同的思路。这种方法比较关注源域和目标域的样本之间的关系。

下图表示了不同领域之间相似的关系。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/afGccJcgcl.png?imageslim)

就目前来说，基于关系的迁移学习方法的相关研究工作非常少，仅有几篇连贯式的文章讨论。这些文章都借助于马尔科夫逻辑网络 (Markov Logic Net) 来挖掘不同领域之间的关系相似性。**什么意思？**

基于马尔科夫网络的关系迁移：**没看懂？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/3gH9E4idHH.png?imageslim)





我们将重点讨论基于特征和基于模型的迁移学习方法，这也是目前绝大多数研究工作的热点。



















* * *





# COMMENT


迁移学习领域权威综述文章A survey on transfer learning
