---
title: ROC
toc: true
date: 2018-08-12 20:04:57
---


[第7章 集成方法 ensemble method](http://ml.apachecn.org/mlia/ensemble-random-tree-adaboost/#_9) **这里面有些是关于ROC的，要总结进来**



TODO

* **比如到底是什么意义？与别的指标的关系？正常情况下大概是多少？对模型的参数有什么指导意义？怎么画？怎么编程实现求这个指标？对于不同的模型这个指标分别应该怎么求？等等，还是有很多要清楚的写的**




# MOTIVE

* 在看机器学习实战的时候，看到ROC，感觉之前的对于模型评估的总结实际上非常不清晰


到底是什么意义？与别的指标的关系？正常情况下大概是多少？对模型的参数有什么指导意义？怎么画？怎么编程实现求这个指标？对于不同的模型这个指标分别应该怎么求？等等



上面的文章中说到：

可以基于代价函数的分类器决策控制：TP*(-5)+FN*1+FP*50+TN*0


![mark](http://pacdb2bfr.bkt.clouddn.com/blog/image/180728/Ikh5bb33LL.png?imageslim)

**实际中真的有这么做的吗？**
