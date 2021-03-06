---
title: nlp 词嵌入 word2vec 与相关应用
toc: true
date: 2018-08-21 18:16:23
---
# nlp 词嵌入/word2vec 与相关应用



# 缘由：


对NLP进行总结：






# NLP常见任务：



* 自动摘要 ：搜索引擎，会想找到一些文字来作为对应网页的摘要。
* 指代消解：小明放学了，妈妈去接**他  **
* 机器翻译：小心地滑-> Slide carefully
* 词性标注：heat(v.) water(n.) in(p.) a (det.) pot(n.)
* 分词（中文，日文等）：大水沟/很/难过  看起来是一个基础的任务，实际上会影响很多后续的NLP环节的准确度，比如分词对于翻译是有很大影响的
* 主题识别
* 文本分类
* ..等等


**看来上面的每个都要自己实践一遍，对相应的代码和使用的库以及原理都要清楚。**








# NLP 处理方法

* 传统：基于规则  rule based
* 现代：基于统计学习，“规则”隐含在模型参数里
    * HMM，CRF，SVM，LDA，CNN，...



# 词编码需要保证词的相似性


最细粒度的需要知道词是什么样的含义，这样就需要对词进行编码，编码成一个向量，你需要这些向量能够保持住词的一些信息，比如我希望它能保持住一些词的一些similarity，一些相似性，相关性。比如，下面的青蛙：这些词，实际上是有一些相关性的，我们希望这个在表示成最后的word vector的时候呢，希望它们能够保持这些相关性。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/DK2DGl8lhB.png?imageslim)




## 简单 词/短语 翻译


想要使向量空间分布具有相似性，能呈现对应的一个关系。**这么厉害？真的有这种相似性吗？**

左：英语                                                                              右：西班牙语


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/6Ji2fEIF73.png?imageslim)




## 向量空间子结构


而且我们想达到这样的一个功能：希望能捕捉到低层的一些similarity

注意，这个地方的每个向量的维度是一致的，因为最后把它压缩成一个dense vector。**这种向量是怎么构建的？**

\[V_{King}-V_{Queen}+V_{Women}=V_{Man}\]

\[V_{Paris}-V_{France}+V_{German}=V_{Berlin}\]

所以，我们的最终目标是：词向量表示作为机器学习，特别是深度学习的输入和表示空间。**怎么构建的？人工？**

在计算机中表示一个词：
<table style="width: 291.466px;" >
<tbody >
<tr >

<td style="width: 135px;" >猫
</td>

<td style="width: 136.466px;" >美丽
</td>
</tr>
<tr >

<td style="width: 135px;" >




  * 动物


  * 胎生


  * 有毛


  * 弹跳性好


  * 体型小


  * 宠物


  * 吃肉


  * 会发声



</td>

<td style="width: 136.466px;" >




  * 漂亮


  * 多姿


  * 靓丽


  * 好看


  * 俊俏


  * 迷人


  * 标志


  * 端庄



</td>
</tr>
<tr >

<td style="width: 135px;" >上位词
</td>

<td style="width: 136.466px;" >同义词
</td>
</tr>
</tbody>
</table>
如果用人来标注的话：




  1. 不能分辨细节的差别


  2. 需要大量人为劳动


  3. 主观


  4. 无法发现新词


  5. 难以精确计算词之间的相似度




#




#




# 离散表示




## 离散表示：One-hot表示


语料库：




  * John likes to watch movies. Mary likes too.


  * John also likes to watch football games.


词典

词典包含10个单词，每个单词有唯一索引，在词典中的顺序和在句子中的顺序没有关联，词典就是自己随便定的一个包含这些单词的字典。

{"John": 1, "likes": 2, "to": 3, "watch": 4, "movies": 5, "also":6, "football": 7, "games": 8, "Mary": 9, "too": 10}

One-hot表示：有多少个词就开多大的一个向量空间，只有这个词出现的编号为下标的那个位置为1，其他的地方为0。


  * John: [1, 0, 0, 0, 0, 0, 0, 0, 0, 0]


  * likes: [0, 1, 0, 0, 0, 0, 0, 0, 0, 0]


  * …


  * too : [0, 0, 0, 0, 0, 0, 0, 0, 0, 1]


one-hot 有好处也有坏处，one-hot 会产生一种对句子的离散的表示：bag of words


## one-hot 对句子的离散表示：Bag of Words  词袋


**词袋与词权重到底是什么关系？从属？**

一种表示方法：文档的向量表示可以直接将各词的词向量表示加和。**不错的方法，但是这样肯定会丢失信息把？次序信息？**




  * John likes to watch movies. Mary likes too.-> [1, 2, 1, 1, 1, 0, 0, 0, 1, 1]


  * John also likes to watch football games.->[1, 1, 1, 1, 0, 1, 1, 1, 0, 0]


但是，上面的方法存在一个问题，比如说John和likes它们的权重好像是一致的，看不出它们的差别。现在希望有个方式能体现出词与词之间的差异：即词权重

词权重有两个方法：


  * TF-IDF (Term Frequency - Inverse Document Frequency)  信息检索。一般用来考察分好的词与文章的相关度。词 t 的IDF 权重为： \(log(1+\frac{N}{n_t})\)  其中 N: 文档总数， \(n_t\) : 含有词t的文档数。所以，刚才看不出词与词之间的差别，但是现在能看到不同的词的权重的差别。 \(n_t=0\)怎么办？这说明了你的语料的容纳度是不够的，一般不会为0。  **TF-IDF的计算公式是什么？这里只列了IDF的计算公式。**


    * [0.693, 1.386, 0.693, 0.693, 1.099, 0, 0, 0, 0.693, 0.693] **这个正确吗？怎么算的？**





  * Binary weighting  短文本相似性，Bernoulli Naive Bayes  这个是上面的Bag of Words的一种简化形式，出现就是1，没有出现就是0 ，对于短文本的相似性用的比较多，尤其是你的词汇比较几种的时候。


    * [1, 1, 1, 1, 1, 0, 0, 0, 1, 1]  **什么意思？**





上面这些无论是Bag of words 还是TF-IDF 还是 Binary weighting ，词在文档中的顺序order都没有被考虑。因此会出现：John likes mary 和Mary likes john 这两个是一样的。

那么怎么将顺序保留下来呢？


## 考虑顺序 的 N元组 Bi-gram和N-gram


为2-gram建索引：把两个两个的词作为一个word：




  * "John likes”: 1,


  * "likes to”: 2,


  * "to watch”: 3,


  * "watch movies”: 4,


  * "Mary likes”: 5,


  * "likes too”: 6,


  * "John also”: 7,


  * "also likes”: 8,


  * “watch football”: 9,


  * "football games": 10,


则根据这样得到如下：


  * John likes to watch movies. Mary likes too.->[1, 1, 1, 1, 1, 1, 0, 0, 0, 0]


  * John also likes to watch football games.->[0, 1, 1, 0, 0, 0, 1, 1, 1, 1]


这个东西就叫做 Bi-gram 即二元 即2-gram 。

这种方式的优缺点如下：


  * 优点：考虑了词的顺序，顺序的问题得到一定程度的缓解


  * 缺点：词表的膨胀，维度极度爆炸


而且模型参数数量与 n 的关系如下：如果是一篇20万词的文本：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/8K5A5h3leF.png?imageslim)

可见，维度不仅极度增长，而且增长之后是一个非常sparse 稀疏的一个情况。那么有没有更好的办法呢？


## 语言模型：


**语言模型与前面的N元组是什么关系？应用的时候是怎么与N元组配合使用的？还是独立使用的？**

后一个词的出现是基于之前的已经出现的词的，因此整个句子出现的概率可以这样认为：


\[P(w_1,\cdots ,w_m)=\prod_{i=1}^{m}P(w_i|w_1,\cdots ,w_{i_1})\]


Unigram/1-gram 即后面出现的词并不依赖于之前出现的词：

\[\begin{align*}P(Mary\,likes\,too) &= P(too | Mark, likes) * P(likes | Mary) * P(Mary)\\
&= P(too) * P(likes) * P(Mary)\end{align*}\]

Bi-gram/2-gram 即只与它前面的这个词有依赖关系

\[\begin{align*}P(Mary\,likes\,too) &= P(too | Mark, likes) * P(likes | Mary) * P(Mary)\\
&= P(too | likes) * P(likes | Marry) * P(Mary)\end{align*}\]

这个就是语言模型，将词与词之间的顺序信息转嫁到出现的概率信息上。


## 上面的离散表示存在的问题






  * 无法衡量词向量之间的关系  各种度量（与或非，距离）都不合适 太稀疏，很难捕捉文本的含义


    * 酒店[0, 1, 0, 0, 0, 0, 0, 0, 0, 0]


    * 宾馆[0, 0, 0, 0, 1, 0, 0, 0, 0, 0]


    * 旅舍[0, 0, 0, 0, 0, 0, 0, 0, 1, 0]





  * 词表维度随着语料库增长膨胀，这样的话模型也就只能不断修改。


  * n-gram词序列随语料库膨胀更快 **是呀**


  * 数据稀疏问题


那么这些问题怎么解决呢？


## 新的表示方式 分布式表示 (Distributed representation)


**这一节不是很明白？什么是分布式表示？怎么就分布了？**

如果直接记忆的话：




  * 红色的大型卡车   memory unit 1   [1, 0, 0, 0, 0, 0, 0, 0, 0, 0]


  * 黄色的中型SUV   memory unit 2   [0, 1, 0, 0, 0, 0, 0, 0, 0, 0]


  * 紫色的小型电动车   memory unit 3   [0, 0, 1, 0, 0, 0, 0, 0, 0, 0]


但是可以分析出句子有一些 patten，把模型相同的拿出来，那么句子相当于是这写模式的组合。那么需要对这个东西进行存储时，就可以压缩到三个维度。做一个笛卡儿组合的：

需要的记忆单元数=(颜色20)*(型号3)*(车型30)

那么这个时候的分布式表示： **什么意思？**

(颜色记忆单元20)+(型号记忆单元3)+(车型记忆单元30)

那么对于一个词来说，怎么表达这个词呢？可以用你附近的一些词来表示。


## 用一个词附近的其他词来表示该词


现代统计自然语言处理中最有创见的想法之一。

banking附近的词将会代表banking的含义：


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/D32BD2Fdg5.png?imageslim)

比如banking 就会牵涉到 government 等。


## 共现矩阵 (Cocurrence matrix)


共现：指的是通过你周边的一些词汇可以来了解你这个词汇，然后到底想通过多少个词汇来了解你呢？这个就是window length。比如windows length=2，那么左边看两个词，右边看两个词。

Word-Document 的共现矩阵主要用于发现主题(topic)，用于主题模型，如LSA (Latent Semantic Analysis)。**什么是LSA？**

局域窗中的Word - Word 共现矩阵可以挖掘语法和语义信息




  * I  like  deep  learning.


  * I  like  NLP.


  * I  enjoy  flying.


下图为 window  length设为1（一般设为5～10），使用对称的窗函数（左右window  length都为1)。

如I的左右两边分别看1个词，左边没有，右边是有2个like，有1个enjoy。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/A63A0h77lg.png?imageslim)

可以看出，这个是一个对称的矩阵。

而且，这个比较重要了：从上表可以认为，比如 I 这个单词，就可以使用对 I 应行或列的向量来表示。**利害**

所以，我们可以吧共现矩阵的行或列作为词向量。

**那么生成的这个矩阵怎么使用呢？在真实的算法中？而且感觉文章很长的话这个也会很大吧？**


## 共现矩阵这种方法存在的问题






  * 向量维数随着词典大小线性增长 **是的**


  * 存储整个词典的空间消耗非常大  **是呀**


  * 一些模型如文本分类模型会面临稀疏性问题。 即使它相对于one-hot的表示形式而言，已经有些dense了，但是还是比较sparse


  * 模型会欠稳定 。除非你的语料库非常大，不然，你新加一些语料的话，频次可能就会有变化。而频次的变化会导致表示的变化。


所以，我需要去构造低维度稠密的向量作为词的分布式表示！ 一般是 25~1000 维 。**为什么是 25~1000 维 ？**

那么怎么变成低维度，稠密的向量呢？


## 那么怎么去构造低维度稠密的向量：最简单的，用SVD


最直接的想法：用SVD对共现矩阵向量做降维


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/EHmhjEBD5l.png?imageslim)

SVD分解的公式：**没明白？之前的SVD的分解没有学到。学了之后这里补充下。**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/77lAmEhi5a.png?imageslim)

然后分解后做一个可视化，为了可视化，对于每个词而言，我只取了前两个维度的信息。**没明白？U是什么？前两个维度的信息指的是什么？**


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/mDjfl64J36.png?imageslim)

从上面的图可发现：I与enjoy和like的距离好像是差不多的，而且距离like最近的好像就是enjoy。所以只是通过这样的简单的梳理，我就可以捕获到like和enjoy之间的similarity。**利害。但是看到这里，我就想知道那个window length 到底取多少比较好？一般取多少？而且，对于中文来说怎么处理？是分次之后像这样进行处理吗？**


## 那么SVD降维有没有什么问题呢？

* 计算量随语料库和词典增长膨胀太快，对X(n,n)维的矩阵，计算量O(n^3)。 而对大型的语料库，n~400k，语料库大小1~60B token。**SVD降维的算法还是要自己实现的，而且要清楚计算量O。**

* 难以为词典中新加入的词分配词向量。**为什么？是降维后的不能直接添加吗？**

* 与其他深度学习模型框架差异大。为什么呢？因为我们希望DL的模型是一个end-to-end的，即数据一喂进去就能拿到想要的结果，也就是说不想要额外的一些东西，一般来说是一个层级的结构。如果与DL的模型框架差异太大，就只能先抽出一些东西，再放到这个模型中去做。


那么还有没有更好的方法呢？


# NNLM (Neural Network Language model)




## NNLM：介绍


这个模型的本质不是用来产生词向量的。**本质是什么？了解的不够**

直接从语言模型出发，将模型最优化过程转化为求词向量表示的过程。首先，语言模型做的事情就是：我要去判断那些词之后出现那些词的概率是最高的。如果我们假设t对应的词只与前面的n-1个词相关：那么目标函数就是：

\[L(\theta )=\sum_{t}^{ }logP(w_t|w_{t-n+1},\cdots ,w_{t-1})\]

解释下上面的式子：对log进行求和，其实就是对P进行连乘。**还有些没理解，n是多少？整个意思是不是每个词基于之前的所有词的概率的乘积？**

使用了非对称的前向窗函数，窗长度为n-1。**为什么使用的是非对称的？为什么窗口长度为n-1？**滑动窗口遍历整个语料库求和，计算量正比于语料库大小。概率P满足归一化条件，这样不同位置t处的概率才能相加，即：之所以满足归一化条件，是因为第n个词无非是从字典里面取的，所有字典里面的词加在一起等于1 。**没有很理解？**

\[\sum_{w\in \{vocabulary\} }^{ }P(w|w_{t-n+1},\cdots ,w_{t-1})=1\]

用神经网络来实现概率P。


## NNLM：结构


是做什么的：我想用一个长度为N的滑动窗口在我的语料中滑动，当我每取出来N个连在一起的词的时候，我用前N-1个词去预测后面的一个词。所以如果文章很长，我就有很多的这种N个词的串，每个都可以进行一次训练。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/gK221ffF00.png?imageslim)

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/K81j1g92cj.png?imageslim)

look-up Table 是一个查找表

先说一下，对于每个输入的词的one-hot表示来说，是怎么压缩到D维的？




  * 假设字典有8万个词，每个都是one-hot表示，我输入的一个字就是 8万*1 的列向量，而我的权重矩阵是D*8万 的，那么 D*8万 的矩阵和输入的 8万*1 相乘之后的就得到D*1 的矩阵，这个矩阵就是压缩后的信息。这里面要注意的一点：因为输入的 8万*1 的列向量实际上只有一个位置是1，其余全是0，所以这个过程实际上就是相当于把D*8万的列向量中的其中一列取出来了。


对每次输入的词压缩到D维之后做什么呢？


  * 在连接层对这些 D*1 的vector做了一个concat 拼接，比如说 ，我输入“我”，“是”，“中国”，“人” 这个句子，在每次得到这个字对应的D*1的矩阵之后，我依次把它拼接起来，这样就得到了一个4D*1的一个列向量。


然后做什么呢？


  * 它有一个hidden layer ，它假如说有100个神经元，然后把刚刚生成的4D*1的矩阵与这些神经元做全连接。全连接以后用tanh激活。这100个神经元输出的100个结果与8万个结果做一个softmax，我最后拿到的是一个8万*1的一个概率向量，而标准答案这个时候只有一个，所以我可以用一个损失函数来衡量好还是不好，这个损失函数叫做交叉熵损失。那么我就可以通过最小化这个交叉熵损失，来通过BP和SGD来更新我的矩阵C，训练这个神经网络。


也就是说：


  * 这个神经网络的程序本意是做语言模型 language model ，这个模型做好之后，我就可以根据 N-1 个词来预测后面的词。但是它有个中间产物：矩阵C，也就是稠密词向量表示。对于每个词而言，把 C 中的对应的那一列取出来就是它的词向量，而且它的维度是有限的，比如说 300 维。而不再是像刚才那样，随着语料的增加，维度上升的那种表示。**利害**


总结一下：


  * (N-1)个前向词:one-hot表示


  * 采用线性映射将one-hot表示投影到稠密D维表示


  * 输出层:Softmax


  * 各层权重最优化:BP＋SGD




## NNLM的计算复杂度


基本上是对上面的结构又介绍了下。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/Kc2ki4e0bD.png?imageslim)

每个训练样本的计算复杂度：\(N * D + N * D * H + H * V\)

一个简单模型在大数据量上的表现比复杂模型在少数据量上的表现会好




# word2vec: CBOW(连续词袋)  **从这里开始没有看**


在NNLM的基础上提出了word2vec，即用一个简化的模型去做上面的训练出词向量这种工作。又CBOW和CBOgrama 可以看一下源码，里面有各种各样的小技巧来帮助它们加速这个。使得这个东西最后的速度非常快。

先理解一下原理，使用的话，可以直接调用。


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/IBb4Il04kA.png?imageslim)


* 无隐层
* 使用双向上下文窗口
* 上下文词序无关 (BoW)
* 输入层直接使用低维稠密表示
* 投影层简化为求和(平均)


目标函数：

$$J=\sum_{w\in corpus}^{ }P(w|context(w))$$

$$J=\sum_{i\in corpus}^{ }log(\frac{exp(w_i^T\widetilde{w_I})}{\sum_{k=1}^{V}exp(w_i^T\widetilde{w_k})})$$

$$J=\sum_{i\in corpus,j\in context(i)}^{ }log(\frac{exp(w_i^T\widetilde{w_I})}{\sum_{k=1}^{V}exp(w_i^T\widetilde{w_k})})$$


## CBOW：层次Softmax


W=“足球”时的相关记号示意图


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/AF70FBDJ91.png?imageslim)






  * 使用Huffman Tree来编码输出层的词典


  * 只需要计算路径上所有非叶子节点词向量的贡献即可


  * 计算量降为树的深度  \(V=>log_2(V)\)




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/9EGhlK668l.png?imageslim)






  * Sigmoid函数\(\sigma (x)=\frac{1}{1+e^{-x} }\)


  * n(w,j)：Huffman数内部第j层的节点


  * ch(n(w,j))：n节点的child节点


  * [[n(w,j+1)=ch(n(w,j)]] 是选择函数，表明只选择从根节点到目标叶节点路径上的内部节点


\[\sum_{w=1}^{W}p(w|w_I)=1\]

**没明白**


## CBOW 负例采样


P(w|context(w))： 一个正样本，V-1个负样本，对负样本做采样

\[g(w)=\sigma (X_w^T\theta^w)\prod_{u\in N \,EG(w)}^{ }[1-\sigma(x_w^T\theta^u)]\]




  * \(X_w\)是context(w)中词向量的和


  * \(\theta^u\)是词u对应的一个（辅助）


  * 向量 \(NEG(w)\)是w的负样本采样子集




损失函数：对语料库中所有词w求和：


\[L=\sum_{w\in C}^{ }log(g(w))\]

\[sum_{w\in C}^{ }\{log[\sigma(x_w^T\theta^w)]+\sum_{w\in NEG(w)}^{ }log[\sigma(-x_w^T\theta^u)]\}\]

词典中的每一个词对应一条线段，所有词组成了[0，1]间的剖分

\(len(w)=\frac{counter(w)}{\sum_{u\in D}^{ }counter(u)}\)，实际使用中取counter(w)^(3/4)效果最好。

\(l_1,l_2,\cdots ,l_N\)组成了[0,1]间的剖分


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/aIB2Ial3dK.png?imageslim)

将[0,1]划分为M=10^8等分，每次随机生成一个[1,M-1]间的整数，看落在那个词对应的剖分上。


## Word2Vec:Skip-Gram模型




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/3B25hLK90F.png?imageslim)


* 无隐层
* 投影层也可省略
* 每个词向量作为log-linear模型的输入


目标函数：

$$\frac{1}{T}\sum_{t=1}^{T}\sum_{-c\leq j\leq c,j\neq 0}^{ }log(p(w_{t+j}|w_t)$$

概率密度由Softmax给出：

$$p(w_k|w_t)=\frac{exp(\widetilde{w}_k^Tw_t)}{\sum_{m=1}^{V}exp(\widetilde{w}_m^Tw_t)}$$


## Word2Vec：存在的问题：



* 对每个local context window单独训练，没有利用包含在global co-currence矩阵中的统计信息
* 对多义词无法很好的表示和处理，因为使用了唯一的词向量


### 词嵌入可视化: 公司 — CEO

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/a4eEL8e23m.png?imageslim)


### 词嵌入可视化: 词向

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/95IhII0HEg.png?imageslim)




### 词嵌入可视化: 比较级和最高级




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/2HCLadkEmC.png?imageslim)




### 词嵌入效果评估: 词类比任务

![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/9fFjak9cAi.png?imageslim)

19544个类比问题




  * “Athens is to Greece as Berlin is to __?”


  * “Bigger is to Big as Greater is to __?”




### 词嵌入效果评估: 词相似度任务




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/60KihC5hD3.png?imageslim)


* SVD：只保留出现次数最大的1万个词，记为\(X_{trunc}\)
* SVD-S：\(\sqrt{X_{trunc} }\)
* SVD-L：\(log(1+X_{trunc})\)

## 词嵌入效果评估: 作为特征用于CRF实体识别


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/cIlL7kfG62.png?imageslim)

NER任务




  * 437,905个离散特征


  * 额外的50维连续特征


  * 使用CRF模型训练




## GloVe与Word2Vec对比




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/J1mKbHJdlc.png?imageslim)

GloVe随着迭代次数增加，精度提升

Word2Vec未使用类似迭代次数的Epoch，用Negative Samples模拟




# 总结






  * 离散表示


    * One-hot representation, Bag Of Words Unigram语言模型


    * N-gram词向量表示和语言模型


    * Co-currence矩阵的行(列)向量作为词向量





  * 分布式连续表示


    * Co-currence矩阵的SVD降维的低维词向量表示


    * Word2Vec: Continuous Bag of Words Model


    * Word2Vec: Skip-Gram Model








# 工具google word2vec






  * 地址


    * https://code.google.com/archive/p/word2vec/


    * 墙内用户请戳https://github.com/dav/word2vec





  * 安装步骤


    * git clone https://github.com/dav/word2vec


    * cd word2vec/src


    * Make


    * 试试./demo-word.sh 和./demo-phrases.sh







# 工具gensim




![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/H2f6ak8Jja.png?imageslim)



![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/HhjHimKjiL.png?imageslim)



![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/C43dj1f98k.png?imageslim)




## 工具gensim中文处理案例


数据下载，训练与测试的代码详情见
https://www.zybuluo.com/hanxiaoyang/note/472184


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/g9JiGabKKB.png?imageslim)

## Word2vec+CNN做文本分类


  * 论文详见《Convolutional Neural Networks for Sentence Classification》
    * http://arxiv.org/abs/1408.5882
  * Theano完成的代码版本：
    * https://github.com/yoonkim/CNN_sentence
  * TensorFlow改写的代码版本：
    * https://github.com/dennybritz/cnn-text-classification-tf
  * 添加 分词和 中文词向量映射之后，可用于中文文本分类(情感分析)


# 作业


仿照以下案例 ， 构建新闻数据集上的新闻词向量

https://www.zybuluo.com/hanxiaoyang/note/472184


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180727/j2B3IK4Am3.png?imageslim)

搜狗全网新闻语料地址 ：
http://www.sogou.com/labs/resource/ca.php




# COMMENT：


**从 word2vec: CBOW  开始没有看，等先看完机器学习的和RNN的再看这个。**

还是很厉害的，感觉，很nice。要补充好。

**。只看了PPT，感觉非常混乱，好像没有什么主次，没有什么分块，当然绝大部分是我的水平不够，看过视频之后进行补充和修正和分割。**

**而且感觉NLP应该是一个很系统的东西，还是要拆分下。**



文本映射乘一个dense vector 这种思想不仅在词嵌入中用到，而且在很多地方会用到，one-hot 的表示形式有时是很难处理的。



# REF：

  1. 七月在线 深度学习
