---
title: 'Dynamic Neural Networks A Survey'
date: 2022-11-01
permalink: /PaperReading/2022/11/01/Dynamic_Neural_Networks_A_Survey
tags:
  - Dynamic Neural Networks
  - Survey
---
### Authors: Yizeng Han, Gao Huang, Shiji Song, Le Yang, Honghui Wang, and Yulin Wang

[paper download](http://starkerfirst.github.io/YangbhPage/files/Dynamic_Neural_Networks_A_Survey.pdf)

![pic2](http://starkerfirst.github.io/YangbhPage/images/Dynamic_Neural_Networks_A_Survey_pic2.png)

## intro

大多数网络都是静态结构和参数（没有runtime适应），可能会影响表达能力（representation power）, 效率（efficiency）和可解释性（interpretability）。

### efficiency

动态神经网络可以在推理时分配计算资源，根据输入是否平凡和有用信息区块大小，在不同粒度（layers，channels，sub-networks，spatial areas，temporal location）上丢弃一些多余的信息内容，减少计算。

![pic1](http://starkerfirst.github.io/YangbhPage/images/Dynamic_Neural_Networks_A_Survey_pic1.png)

### representation power

显然，动态神经网络大幅提高了参数空间，并有选择的调用空间，有点类似大脑分区工作。不仅保证了推理时间，还获得了更强适应力和应用场景。soft attention mechanism也可以算作动态神经网络一种。

### Adaptiveness

动态神经网络做出了实时的trade-off between accuracy and efficiency。 因此相比于静态网络，更能适应不同的硬件平台和软件环境。（comment：这个很难说，，，）

### Compatibility

动态神经网络是在普通静态网络的基础上得到的，经过一些连接控制部件得到的（并联MOE，串联Skipnet，以及其他拓扑），继承了所有的优点和新进展。

### Generality

不限于一些特殊场景，可以无缝衔接任何应用场景。

### Interpretability

通过施加输入来观察哪一部分起了作用，可以揭示模型和大脑底层机制的类似性，为DNN模型的可解释性工作提出有效方法。(SNN也是类脑的一种尝试）

## Text Summary

本文接下来分粒度的介绍了**Sample-wise，spatial-wise，temporal-wise** 三种动态类别，每一种动态类别都可以在不同方面上进行。在文章后半段还讲述了判定准则和训练方式。具体方式不作列出。

## comment

动态神经网络的优势是尝试在accuracy, computational efficiency, adaptiveness上有所突破。直观上来看，这种tradeoff在理想上确实很诱人，但是从实验中可以发现仍存在一些和理论上的gap。

* Budgeted prediction

  不是所有应用场景都是可以动态的，一些边缘/移动计算场景要求以稳定速率流水线处理数据，此时latency和computation budget是预定义的，显然动态会给调度带来极大麻烦。
* Streaming prediction

  想要节省计算，就一定不能batch并发，否则必然每一个分支都需要运行。即使最后可以mask掉结果，但是计算是省不了的。所以要求batch size恒为1，这是对计算资源的浪费。
* hardware platform limitation

  现有的通用计算平台很难在控制流上给出很好的优化性能，比如GPU基于SIMD运行，Spatial accelerator则是Static dataflow，这二位是极其缺乏branching的控制的，所以要么全算完，要么中间插入CPU控制介入。(之后Adyna的工作就是在这个基础上开展的)   此外，还有稀疏性的计算问题，这都不是通用平台能很好处理的。
