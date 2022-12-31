---
title: 'Adyna'
date: 2022-11-10
permalink: /posts/2022/11/10/Adyna
tags:
  - accelerator
  - simulator
  - gem5
---
[github repo](https://github.com/starkerfirst/adyna)







# Systolic Array Model in Gem5

gem5目前大型simobject的开源代码非常少，唯一能找到的比较完整的是gem5-aladdin中的systolic array model，这个模型也是脉动阵列在github上唯一gem5实现，非常具有学习意义。

[harvard-acc/gem5-aladdin: End-to-end SoC simulation: integrating the gem5 system simulator with the Aladdin accelerator simulator. (github.com)](https://github.com/harvard-acc/gem5-aladdin)

唯一问题也是最大问题是代码的doc几乎没有，复杂而难懂。我大概摸索总结出了一个如下的代码框架(/src/systolic_array)

## system
* **adyna_connection.h**        functions to invoke adyna
* **sys_connection.h**          addition functions to support adyna_connection
* **adyna_params.h**            definitions of params data class
* **adyna.h**                   systematic definition of Adyna (c++ edge) 
* **adyna.py**                  systematic definition of Adyna (Python edge)
* **SConscript**                scons connection file


## microarchitecture
* **dataflow.h**                a complete picture of the whole SA dataflow
* **register.h**                model of double buffer, using timebuf.h
* **pe.h**                      definitions of MAC, PE and IOreg
* **fetch.h**                   definitions of fetch unit
* **scratchpad.h**              definitions of scratchpads and slaveports
* **local_spad_interface**      definitions of masterports to spm 

## data
* **datatype.h**                describe the definition of pixel data (in MAC)
* **tensor.h**                  describe the definition of tensor and its params

## Useless yet
* **Gem5Datapath.h**            systematic definition virtual class (including interface and params)



Adyna architecture
==================
Adyna的spec正在搭建，相关代码已经开始编写，整体架构图如下：

![github repo](http://starkerfirst.github.io/YangbhPage/images/adyna_schematic.png)

## Acc

加速器部分采用systolic_array的方式，不过会简化接口加强建模，大概的框架如下图。

![github repo](http://starkerfirst.github.io/YangbhPage/images/adyna_acc.png)

## NSR

这应该是Adyna最复杂的部分，因为其承载了很多核心的功能，包括**storage, routing, manipulation and reconfiguration**。相关spec仍在制定。

## NoC

NoC部分采用Garnet Network来建模。

![github repo](http://starkerfirst.github.io/YangbhPage/images/adyna_noc.png)
