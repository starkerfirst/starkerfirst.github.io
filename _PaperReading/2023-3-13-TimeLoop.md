---
title: 'TimeLoop'
date: 2023-3-13
permalink: /PaperReading/2023/3/13/TimeLoop
tags:
  - simulator
---

Sources: NVlabs/Timeloop

tutorial: [TimeLoop/Accelergy](https://accelergy.mit.edu/tutorial.html)

ppt: [TimeLoop_Slide](https://accelergy.mit.edu/ispass2020/2020_08_23_timeloop_accelergy_tutorial_part1.pdf)      [Accelergy_Slide](https://accelergy.mit.edu/ispass2020/2020_08_23_timeloop_accelergy_tutorial_part2.pdf)

  

# Motivation

DNN 程序逻辑可从两个角度来看：数据流与控制流。从程序的状态机语义来说，DNN加速器就是根据DNN程序的特点，从数据流图中选择可并行计算的数据节点，并且用一些高效的控制手段（数据重用，数据预取，分发多播等）完成，即“**divide, distribute, schedule”**，这一整个过程被称为**mapping**。现在更常用的是数据流分析（本质上是和控制流等价的）。

所以在设计mapping过程中，Data Movement和Arithmetic就是两大需要加速的部分。现在很多论文就是在设计domain-specific的存储结构和计算结构，最常见的关键词就是“数据重用”，这是一种减少高延迟高能耗的dram访存的方法，主要有Temporal、Multicast、Forwarding三种。

![pic1](https://starkerfirst.github.io/YangbhPage/images/timeloop_reuse.png)

可以想象到，针对同一个workload可以设计的存算结构空间（包括软硬件栈）是很大的，但是总有比较高效的架构和相应配置（paper给出的在同一个arch上不同mapping的能效两极差达19x）。如何找到这个最优的mapping方式是众多工作的研究对象，本篇paper就提供了一个框架，用来评估一个架构在最佳mapping下的性能（本文不在arch层进行探索，而是一种评估方式）。



innovation：

* *Flexibly describe a wide range of architectures* 

    能描述广泛的架构设计

* *Find optimal mappings for a wide range of workloads onto the architecture*

    每个 DNN 加速器都唯一地公开了许多可配置的硬件设置，并要求设计人员找到一种为每个工作负载调度操作和移动数据的方法，即为每个工作负载找到最佳映射（cost model）

* *Accurately predict energy for a range of accelerator designs*

    准确建模加速器设计空间中涉及的所有组件的能耗（这部分是在Accelergy中）

* *Handle a wide range of technologies*

    基本组件支持新工艺（rram，optic，7nm），以准确反映与技术相关的成本

# Model

建立一个评估模型需要三部分作为输入：**Problem, Architecture, Mapping**

![model](https://starkerfirst.github.io/YangbhPage/images/timeloop_model.png)

## problem

为了向TimeLoop中输入workload，我们需要先根据数据空间建立一个**Operation Space**，简单理解就是将循环展开成一个维度，各个数据空间就是这个Operation Space的一个投影。

![problem](https://starkerfirst.github.io/YangbhPage/images/timeloop_problem.png)

## arch

TimeLoop提供了很多的组件可以供描述架构使用

![arch](https://starkerfirst.github.io/YangbhPage/images/timeloop_arch.png)

## mapping

ppt为了演示是自己输入的mapping spec，但是一般我们直接使用自动mapper完成。mapping需要描述具体的算法执行方式、数据移动方式等，有很多的attribute选项可以精细化模拟。Example1中也体现了在同一个problem和arch上的多种mapping方式。

![mapping](https://starkerfirst.github.io/YangbhPage/images/timeloop_mapping.png)

ppt中提供了很多其他的示例和spec规范。

bypass：在某一层不储存相应数据（节省空间，但是更多外部访存）

spatial block: 空间partition 

## statistics

![stat](https://starkerfirst.github.io/YangbhPage/images/timeloop_stat.png)

## advanced function

这一部分我没有很懂，主要是介绍模拟和统计方法，是比较底层的机制，需要仔细阅读源码。

* Tile analysis: Measure and record **deltas** over all space  and time.

    delta是tile位置增量，比如卷积中平移的步长。在时间上，这个步长与stationarity, sliding window behavior有关；空间上，表征相邻PE重叠的tile部分，这与数据复用方式有关。

* uarch model

* ESTIMATING PERFORMANCE AND ENERGY

# Mapper

人工确定完整的mapping形式会随着任务规模增大而变得更加困难。尽管一些方案可以直觉得到，但是一个自动化的探索工具是更好的选择。

TimeLoop's Mapper需要**Problem, Architecture, Constraints**作为输入，生成探索空间，并在探索空间中找到最优选择。

![mapper](https://starkerfirst.github.io/YangbhPage/images/timeloop_mapper.png)

## map space

Mapping其实是有template的，mapper需要的是在参数空白上填上最优的值，包括Factors, Permutations, Dataspace Bypass等参数。

![mapspace](https://starkerfirst.github.io/YangbhPage/images/timeloop_mapspace.png)

当然，某些架构不能接受一些映射，或者人工通过直觉限制了一些参数，或者在探索过程中发现了一些不合理的空间，就会裁剪掉一部分。

## tuning method

mapper采用了启发式搜索来参数优化，同时可以选择参数的优化优先级。同时TimeLoop还支持空间分解（多线程）。

# Comment

TimeLoop作为一个已经被校准过的评估模拟器，完全可以作为自定义模拟器的校准对象，比如Adyna中我们就用到了这种方式。