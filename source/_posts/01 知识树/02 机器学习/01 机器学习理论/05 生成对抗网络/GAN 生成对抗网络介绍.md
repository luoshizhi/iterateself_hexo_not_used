---
title: GAN 生成对抗网络介绍
toc: true
date: 2018-07-27 15:03:17
---
# GAN 生成对抗网络介绍

# ORIGINAL






  1. [GAN原理，优缺点、应用总结](https://blog.csdn.net/qq_25737169/article/details/78857724) 这个人写的挺实在的。要总结下


  2. [机器之心GitHub项目：GAN完整理论推导与实现，Perfect！](https://www.jiqizhixin.com/articles/2017-10-1-1)


  3. [生成式对抗网络（GAN）如何快速理解？这里有一篇最直观的解读](https://blog.csdn.net/Ksf3kg7dU95rn0XL/article/details/79015258)


  4. [RL-GAN For NLP: 强化学习在生成对抗网络文本生成中扮演的角色](https://cloud.tencent.com/developer/article/1087101)


  5. [ML-Tutorial-Experiment](https://github.com/jiqizhixin/ML-Tutorial-Experiment)


  6. [Ian Goodfellow：生成对抗网络 GAN 就是强化学习（超全资料）](http://www.g.com.cn/tech/34807729/)


  7. [最简单易懂的GAN（生成对抗网络）教程：从理论到实践（附代码）](https://www.leiphone.com/news/201706/ty7H504cn7l6EVLd.html)


  8. [Generative-Adversarial-Network-Tutorial](https://github.com/uclaacmai/Generative-Adversarial-Network-Tutorial)


  9. [Generative Adversarial Networks for beginners](https://www.oreilly.com/learning/generative-adversarial-networks-for-beginners)


  10. [GAN的最大贡献（V(G,D) 公式）是怎么想出来的？](https://www.zhihu.com/question/63793084)


  11. [【李宏毅2017 深度学习GAN课程】Generative Adversarial Network 中文 【授权搬运】](https://www.bilibili.com/video/av18603573/)


  12. [Ian Goodfellow Presentations](http://www.iangoodfellow.com/slides/)


  13. [Generative Adversarial Networks](https://arxiv.org/abs/1701.00160)


  14. [生成对抗网络 wiki](https://zh.wikipedia.org/zh-hans/%E7%94%9F%E6%88%90%E5%AF%B9%E6%8A%97%E7%BD%91%E7%BB%9C)


  15. [GAN学习指南：从原理入门到制作生成Demo](https://zhuanlan.zhihu.com/p/24767059)


  16. [到底什么是生成式对抗网络GAN？](https://www.msra.cn/zh-cn/news/features/gan-20170511)


  17. [PyTorch GAN 教程](https://morvanzhou.github.io/tutorials/machine-learning/torch/4-06-GAN/)


  18. [Tensorflow 50行 GAN 代码](https://github.com/MorvanZhou/Tensorflow-Tutorial/blob/master/tutorial-contents/406_GAN.py)


  19. [论文 Generative Adversarial Networks](https://arxiv.org/abs/1406.2661)


  20.



# TODO






  * **需要整理一下**





* * *





# INTRODUCTION






  * aaa





  * 下文介绍GAN本身的部分，包括GAN的特点，优缺点总结，常用的训练tricks，以及GAN的一些改进成果，有基础的可以直接跳过这一部分。


  * 本文的第三部分会介绍一些GAN的变种以及复现很好的GitHub代码链接，感兴趣的可以看一下。


  * 在本文的第四部分，我会列举一些GAN的应用，介绍其原理，同时附有github代码链接。





# GAN 介绍




## 介绍一下 GAN？


生成对抗网络 GAN（Generative adversarial nets），是一种生成式模型，也是一种无监督学习模型。**什么叫生成式模型？**

它最大的特点是为深度网络提供了一种对抗训练的方式，此方式有助于解决一些普通训练方式不容易解决的问题。**解决了什么问题？**


## GAN 到底是什么？


GAN的主要灵感来源于博弈论中**零和博弈**的思想。

应用到深度学习神经网络上来说，就是通过生成网络G（Generator）和判别网络D（Discriminator）不断博弈，进而使 G 学习到数据的分布。

举个例子：用在图片生成上，我们想让最后的 G 可以从一段随机数中生成逼真的图像：

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/k410K0HBcC.png?imageslim)

上图中：




* G是一个生成式的网络，它接收一个随机的噪声 z（随机数），然后通过这个噪声生成图像。**怎么生成的？**

* D是一个判别网络，判别一张图片是不是 “真实的”。它的输入是一张图片，输出的 D(x) 代表 x 为真实图片的概率，如果为 1，就代表 100% 是真实的图片，而输出为 0，就代表不可能是真实的图片。


那么这个训练的过程是什么样子的呢？在训练中：


* G 的目标就是尽量生成真实的图片去欺骗判别网络 D。

* D的目标就是尽量辨别出G生成的假图像和真实的图像。


这样，G 和 D 就构成了一个动态的“博弈过程”，最终的平衡点即纳什均衡点。 **什么叫纳什均衡点？为什么这个点是纳什均衡点？这个纳什均衡点有什么用处吗？**



OK，上面我们大概了解了下 GAN 到底是什么，那么他有那些特点呢？


## GAN的特点



* 相比较传统的模型，他存在两个不同的网络，而不是单一的网络，并且训练方式采用的是对抗训练方式。**嗯，是的**

* GAN中 G 的梯度更新信息来自判别器 D ，而不是来自数据样本。**嗯，是的，D 传过来的错误率就是 G 调整的依据。**





# GAN 的优缺点




## GAN 的优点






  * GAN是一种生成式模型，相比较其他生成模型（玻尔兹曼机和GSNs）只用到了反向传播,而不需要复杂的马尔科夫链。**什么是波尔兹曼机？ 什么是GSNs？**


  * 相比其他所有模型，GAN可以产生更加清晰，真实的样本。


  * GAN采用的是一种无监督的学习方式训练，可以被广泛用在无监督学习和半监督学习领域。嗯，是的


  * 相比于变分自编码器， GANs没有引入任何决定性偏置( deterministic bias)，变分方法引入决定性偏置，因为他们优化对数似然的下界，而不是似然度本身，这看起来导致了 VAEs 生成的实例比 GANs 更模糊。**没看懂，****什么意思？什么是决定性偏置？什么是变分自编码器？**


  * 相比VAE， GANs没有变分下界，如果鉴别器训练良好，那么生成器可以完美的学习到训练样本的分布。换句话说，GANs是渐进一致的，但是VAE是有偏差的。**什么意思？**


  * GAN应用到很多场景上，比如图片风格迁移，超分辨率，图像补全，去噪，避免了损失函数设计的困难，不管三七二十一，只要有一个的基准，直接上判别器，剩下的就交给对抗训练了。**厉害了。**





##  GAN 的缺点






  * 训练 GAN 需要达到纳什均衡，有时候可以用梯度下降法做到，有时候做不到。我们还没有找到很好的达到纳什均衡的方法，所以训练 GAN 相比 VAE 或者 PixelRNN 是不稳定的，但我认为在实践中它还是比训练玻尔兹曼机稳定的多。**什么是VAE？什么是PixelRNN？什么是波尔兹曼机？为什么有的时候使用梯度下降方法做不到？现在有什么找到纳什均衡的方法吗？现在有什么进展吗？**


  * GAN 不适合处理离散形式的数据，比如文本。**为什么？现在还是这样吗？**


  * GAN 存在训练不稳定、梯度消失、模式崩溃的问题（目前已解决）。模式崩溃是什么意思？GAN也会梯度消失吗？







# GAN 与强化学习有什么关系？




GAN是使用RL来解决生成建模问题的一种方式。GAN的不同之处在于，奖励函数对行为是完全已知和可微分的，奖励是非固定的，以及奖励是agent的策略的一个函数。但我认为GAN基本上可以说就是RL。**GAN与强化学习（RL）原则之间有什么相似之处（如果有的话）？GAN的 “generator - discriminator”的想法和RL的“agent - environment interaction” 有紧密的联系吗？**











# GAN 的变种


自从GAN出世后，得到了广泛研究，先后几百篇不同的GANpaper横空出世，国外有大神整理了一个GAN zoo（GAN动物园），链接如下，感兴趣的可以参考一下：

https://github.com/hindupuravinash/the-gan-zoo

由于GAN的变种实在太多，这里我只简单介绍几种比较常常用的成果，包括DCGAN,, WGAN, improved-WGAN，BEGAN，并附有详细的代码github链接。




# **GAN的广泛应用**


这些都要总结一下。




  1. GAN本身是一种生成式模型，所以在数据生成上用的是最普遍的，最常见的是图片生成，常用的有DCGAN WGAN，BEGAN，个人感觉在BEGAN的效果最好而且最简单。


  2. GAN本身也是一种无监督学习的典范，因此它在无监督学习，半监督学习领域都有广泛的应用，比较好的论文有 Improved Techniques for Training GANs、Bayesian GAN（最新）、Good Semi-supervised Learning


  3. 不仅在生成领域，GAN在分类领域也占有一席之地，简单来说，就是替换判别器为一个分类器，做多分类任务，而生成器仍然做生成任务，辅助分类器训练。


  4. GAN可以和强化学习结合，目前一个比较好的例子就是seq-GAN


  5. 目前比较有意思的应用就是GAN用在图像风格迁移，图像降噪修复，图像超分辨率了，都有比较好的结果，详见pix-2-pix GAN 和cycle GAN。但是GAN目前在视频生成上和预测上还不是很好。


  6. 目前也有研究者将GAN用在对抗性攻击上，具体就是训练GAN生成对抗文本，有针对或者无针对的欺骗分类器或者检测系统等等，但是目前没有见到很典范的文章。






























* * *





# COMMENT
