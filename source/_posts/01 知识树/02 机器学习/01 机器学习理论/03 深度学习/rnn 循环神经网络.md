---
title: rnn 循环神经网络
toc: true
date: 2018-08-12 13:02:54
---

# RNN 循环神经网络

TODO

- **双向RNN没有听明白，要再查找下相关的资料看下。最好有个例子能实现一下。**
- **实际上老师讲的还是很清楚的，但是还是有几个问题，要解决。而且，中间的演示代码好像没有找到。**
- **而且，每个算法，我都想实际实现下，不然总是感觉空落落的。**



## 从神经网络到循环神经网络

为什么会有 RNN？

因为传统的神经网络（包括CNN），输入和输出都是互相独立的。比如图像上的猫和狗是分开的，但有些任务，后续的输出和之前的内容是相关的。比如：我是中国人，我的母语是____。

而 RNN 引入了 "记忆"的概念，循环指的是每个元素都执行相同的任务。但是输出不仅依赖于输入还依赖于之前的 “记忆”。



## 循环神经网络的应用


RNN 一般在自然语言处理中用的比较多。其实只要是序列到序列的都可以用 RNN 来学习，比如

- 在通过搜索引擎查询的时候，可以把网页内容作为一个文本序列，查询的字段看作为一个文本序列，这时就可以用 RNN 来学习这两个文本序列的对应关系。这样搜索的时候，就可以按照对应关系展示出来。<span style="color:red;">老师说百度可能使用这个的，不知道是不是，感觉不大可能</span>

下面我们看下 RNN 的一些应用场景：

### 文本的模仿

可以模仿论文（连公式都格式很正确）

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/1hE8H1BeCG.png?imageslim)

<span style="color:red;">对于现在自动写新闻的还是要了解下。想知道具体是怎么实现的。</span>

可以模仿linux内核代码 “写程序”，它不能学到逻辑，但是能学到排布：

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/fj7bL3iH4D.png?imageslim)

<span style="color:red;">不知道现在的自动程序开发到什么程度了。要了解下。</span>


模仿郭敬明的小说：可以模仿出小说的语言风格。

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/5Ef85F8h4F.png?imageslim)

<span style="color:red;">语言风格到底是什么呢？</span>

### 机器翻译


以前的翻译模型是基于统计的 SMT（统计机器翻译），SMT相对而言复杂一些，需要存大量的映射关系在本地，比如英文的 china，对应中文的中国。

而 NMT （neural machine translation）就不用把映射关系存放下来。<span style="color:red;">关于 NMT 的介绍还是要补充下的，在工程实践中把 NMT 添加进去。</span>


SMT 不是说完全作废了，实际上 SMT 囊括了 NLP 中大量的问题，做 NLP 遇到的大量问题都会在 SMT 中出现，其实它是一个很好的学习领域。

bing 用的就是 SMT，google 用的就是 NMT。<span style="color:red;">不知道现在是什么情况了。</span>

NMT 就是 RNN 最擅长的事情：序列到序列的学习。

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/GJDgb05A3B.png?imageslim)




### 看图说话：

会根据图片的内容来用文字进行描述。也可以对一个图片进行提问，然后会给出答案。

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/HdlhIleF67.png?imageslim)

<span style="color:red;">不知道现在的进展是什么样了？对图片进行提问真的可以吗？是什么原理？这样的网络是怎么训练出来的？图像和问题同时作为输入？答案作为label？涉及图片的时候也用RNN吗？</span>


OK，下面我们详细讲一下循环神经网络的结构：




## 循环神经网络的结构

language model 指的是，有一个序列，我们知道了前n-1个element是什么之后推断下面一个element是什么。

简单来看，把序列按照时间展开

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/K56eAIjEG6.png?imageslim)

解释如下：**还是很清楚的**

  * \(X_t\) 是时间t处的输入。
  * \(S_t\) 是时间t处的记忆， \(S_t=f(UX_t+WS_{t-1})\) ，f 可以是 \(tanh\) 等。
  * \(O_T\) 是时间t处的出书，比如是预测下一个词的话，可能是 softmax 输出的属于每个候选词的概率， \(O_t=softmax(VS_t)\)。


## 结构细节：


可以把隐状态 \(S_t\) 视作记忆体，捕捉了之前时间点上的信息。而输出 \(O_t\) 由当前时间以及之前所有的记忆共同计算得到。即只关于\(S_t\)。

理论上来说，我们的 \(S_t\) 能保留之前的所有的信息的，但是我的矩阵的维度是有限的，也就是说我能存储的信息量是有限的，因此 实际的时候，\(S_t\)并不能捕捉和保留之前的所有信息（记忆有限？）

不同于CNN，这里的RNN其实整个神经网络都共享一组参数（U，V，W），极大减小了需要训练和预估的参数量。**嗯**

图中的 \(O_t\) 在有些任务下是不存在的，比如文本情感分析，判断一篇文章的情感，因为在读一篇文章的时候，读到每一个位置我都可以输出一个结果，当然，这个结果不一定是准确的，所以，我可以等文章读完之后，得到的最终的结果作为文本的情感结果就可以。也就是说，对于文本情感分析这个任务来说，我只需要最后的那个 \(O_t\) ，中间的都不需要。**那这样的话，这个文本情感分析怎么训练？只把最后这个位置的损失球出来就可以，然后用BPTT去反向传播就行。**


## 一个例子：


举一个经典的例子：序列到序列的学习，一个language model 。

有一个经典的例子是char-rnn，上面的文本的模仿的三个例子都是这个模型生成的。**这个就是char-rnn 吗？想看一下char-rnn。**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/f7Kkib6199.png?imageslim)

RNN生成模型模仿语言风格例子：




  * \(s_t=tanh(Ux_t+Ws_{t-1})\)


  * \(o_t=softmax(Vs_t)\)


  * \(x_t\in \mathbb{R}^8000\)


  * \(o_t\in \mathbb{R}^8000\)


  * \(s_t\in \mathbb{R}^100\)


  * \(U\in \mathbb{R}^{100\times 8000}\)


  * \(V\in \mathbb{R}^{8000\times 100}\)


  * \(W\in \mathbb{R}^{100\times 100}\)


对照代码看下。

**注意：这个地方的代码没有找到，实际上视频中对照代码讲了一些，再找找，应该是有的，找到之后补充进来。而且对应的视频也没有看要参照看下。 这个还是值得找一下的，因为里面是他自己手写的RNN代码，而不是调用的库。**

现在一般不用Caffe做RNN，因为它的输入比较麻烦。






# 不同类型的RNN


这部分很重要，因为实际中单向的LSTM用的还真不太多，大部分用的都是双向的深层的RNN


## 双向RNN


在有些情况下，当前的输入不只依赖于之前的序列元素，还可能依赖于之后的序列元素，比如说从一段话踢掉部分词，让你补全。那么这个时候，只用之前的RNN感觉就有点不足了，那么怎么办呢？可以用双向的RNN。当然，单向的RNN可以做的事情也可以用双向来做，可能会捕捉到更多的信息


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/L77Bf8IllF.png?imageslim)

上面的h是hidden的意思，对应于之前的RNN中的S。

一个是从左往右做迭代的，一个是从右往左做迭代的，然后对h做一个拼接。前向和后向的W和V是不一样的。因为一个是从前往后的，一个是从后往前的。**实际上这个地方有些不明白，是怎么在从前往后训练的时候而且从后往前训练的？而且这个时候的输出的 y 是什么？这个时候的损失函数是什么？实际的代码是怎么实现的呢？还是说，先前向训练一遍，再反向训练一遍这样循环？**

Tensorflow中是有双向的RNN的。 MxNet里面也是有的。


## 深层双向RNN


和双向RNN的区别是每一步/每个时间点我们设定多层结构


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/J1A76D0k80.png?imageslim)

**说实话这个没听明白。为什么有这些隐层？训练的时候是什么样的？看一下相关的资料。**


##




# RNN和BPTT算法 （重要）


在CNN中，我们用BP+SGD，在RNN中我们使用BPTT ，back propagation through time 。

MLP（DNN）与CNN用BP算法求偏导，BPTT和BP是一个思路。**MLP（DNN）是什么？**


## 举个例子：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/6GbL3BGHj3.png?imageslim)

比如说，我依次输入的是："我"，“爱”，“北京“，”天安门“，”广场“，然后想要的输出是：“爱”，“北京“，”天安门“，”广场“。假设我的词典中有4万个词。即，每个输出都是一个4万*1的一个概率向量。


## 损失函数


我们到每个时间点的时候都又一个输出，都会计算一个loss， 交叉熵损失 softmax ，**交叉熵损失函数是这样的吗？**：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/fm92AGjEfh.png?imageslim)

现在我要计算所有的loss，因此，沿着时间轴把所有的loss加在一起：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/KmdJ02FH4B.png?imageslim)




## 那么这个时候怎么求偏导呢？


我的loss有了，我们想找到 W 使 loss function 最小，我们有随机梯度下降的方法。随机梯度下降要求我们求loss function对于W的偏导，求偏导的这个过程就是我们这个地方的核心，在RNN中，求偏导不能使用BP，而要使用BPTT。为什么这么说呢？

首先我们知道要求这么一个偏导：**为什么是针对W的？U和V不用管吗？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/1LA4IAFG4F.png?imageslim)

我们单独看一个位置的：**是的**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/8BCcD76JBJ.png?imageslim)

但是呢：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/mlcJkI58L8.png?imageslim)

我们发现，s3与s2还有关系，即，s3对于W的偏导我们没有办法直接求出，因此我们只能沿着时间轴把s2往前展开：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/2DDiKkIImJ.png?imageslim)



如下图所示：




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/CG1Hi0BHKL.png?imageslim)

即可以写成：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/hAKA7LmhkL.png?imageslim)

那么这个就是BPTT，可见，与BP还是有本质的区别的，就是要涉及之前的各个状态的偏导。






# RNN与图片描述




## 怎么去做呢？


有一张图片，我希望我用文本对它进行描述。那么怎么做呢？

我们找了一个Alexnet，输进去一张图片之后，会得到一个4096*1的向量。这个向量含有图片的很多的信息，相当于对这个图片抽取了特征信息。然后，我把这个向量也添加到我们的RNN中。如下图所示：即把我的4096*1的向量以一个新的权重\(W_{ih}\)叠加到我的RNN的公式中。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/J6b3CA9ECH.png?imageslim)

实际上，上面这个公式写的有些不清楚，后面把图修一下，公式实际上就是：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/Kl2dCihjih.png?imageslim)

现在在括号中添加了 \(W_{ih}*v\) 这一项。

这个地方要强调下，**这个添加 \(W_{ih}*v\) 这一项只在第一步做**，后面每一次的更新再也没有这一项了，也就是说后面的部分还是老样子，如下图：

**但是我没有很明白，为什么这样直接的添加是有用的？为什么只在第一步的时候添加这一项？而且，起头的东西是什么？也就是说这个时候的\(x_0\)是什么？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/lh6B56BIgK.png?imageslim)

这个是简单的一种image caption ，有更厉害的attention model，这个后面会讲到。**讲到之后我这个地方提一下。对比一下。**


## 数据集


有图片描述数据集 http://mscoco.org   里面有12w张图片，5句话描述每张图片。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/l1maJh0LGd.png?imageslim)

下面是一些attention model 产生的结果：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/fAhc8CA7l2.png?imageslim)

**最好自己试一下，最起码要找一个已经实现的代码看一遍。**










# REF：
1. 七月在线 深度学习
