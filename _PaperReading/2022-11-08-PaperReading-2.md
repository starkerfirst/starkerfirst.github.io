---
title: 'Tangram'
date: 2022-11-08
permalink: /PaperReading/2022/11/08/Tangram
tags:
  - spatial accelerator
  - dataflow opt
---
### Authors: **Mingyu Gao**, Xuan Yang, Jing Pu, Mark Horowitz, and Christos Kozyrakis

## Intro

基于tile的架构是面对日益复杂且庞大的DNN的提高加速器capabilities和efficiency的重要方式，因为它使网络不仅放得下，还能充分挖掘无依赖数据之间的并行性。它支持两大粗粒度并行：**层内并行**和**层间流水**，然而目前细粒度并行上仍有挖掘空间。

本文提出三个novelty：

* 层内buffer sharing： 将不同tile的分布buffer联合成shared buffer，减少冗余和相同数据的多次存取
* 层间alternate layer loop ordering：更加细粒度地前递数据，减少buffer需求量和流水线延时
* 复杂DAG的流水计算单元的拓扑分配，最小化off-chip mem读取

## Background

本文采用eyeriss（MIT的深度学习处理器）作为baseline，在不改变baseline构成的前提下应用三种优化方法。eyeriss的架构是tile加速器的典型，整体为两级tile分割。一个engine由2d spatial pe array组成，公用一个buffer，挖掘细粒度并行。

![pic1](http://starkerfirst.github.io/YangbhPage/images/tangram_pic1.png)

上层chip是由2d spatial engine array组成，NOC负责连接和与off-chip mem的互联。

![pic2](http://starkerfirst.github.io/YangbhPage/images/tangram_pic2.png)

从数据流的角度来看，对于CONV运算，一个加速器的效果取决于nested loop如何被有效展开、变换（交换）、调度（空间分配）。因为on-chip和off-chip之间的交换次数会直接影响性能，所以需要最大化数据重用。

为什么不直接增大每个engine里的PE数呢？这会有四点问题：

* 较小的层不能充分利用，并且一个engine里有多个CONV运算会干扰。
* 启动延时和耗能增大
* PE与Gbuf之间的最大距离成为瓶颈
* 不能支持层间有效流水（连接方式僵硬）

## Proposed Dataflow

### Buffer sharing

![pic3](http://starkerfirst.github.io/YangbhPage/images/tangram_pic3.png)

![pic4](http://starkerfirst.github.io/YangbhPage/images/tangram_pic4.png)
TBD
