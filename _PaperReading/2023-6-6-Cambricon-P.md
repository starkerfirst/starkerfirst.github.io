---
title: 'Cambricon-P'
date: 2023-6-6
permalink: /PaperReading/2023/06/06/Cambricon-P
tags:
  - bit serial architecture
  - scalable accelerator

---

title: Cambricon-P: A Bitflow  Architecture for Arbitrary Precision Computing

authors: Yifan Hao, Yongwei Zhao, Chenxiao Liu, Zidong Du, Shuyao Cheng, Xiaqing Li,  Xing Hu, Qi Guo, Zhiwei Xu, and Tianshi Chen



(PS: This note is written in the writing test for Ph.D. recruitment in IIIS, Tsinghua 2023SU)



## Note: Cambricon-P

#### author: Bohan Yang

This article aims at **arbitrary precision computing** (APC), where the digits of operands vary from tens to millions of bits. APC could be applied in scientific scenarios because some algorithms may be deviation-sensitive. However, the current platforms only have low-bitwidth size-fixed ALUs, which means that operands must be decomposed into small pieces and we should sum up all the intermediates. It comes to the problem of huge on-chip storage and traffic demands, with a long dependency chain between intermediates. Also, we can observe that there are many repetitive and sparse binary computing. How to utilize this redundancy could lead to two opposite efficiency levels. To solve this challenge, the authors propose a **bitstream architecture** to support flexible and monolithic large operations, including **inner-product transformation**, **carry parallel computing mechanism in GUs** and **bit-indexed inner-product processing scheme** to exploit the intra-IPU (inner-product unit) bit redundancy and inter-IPU parallelization. 

In my opinion, the biggest strength of Cambricon-P is the pre-computing scheme for inner-product and carry propagation. As we discussed, the challenge is the binary redundancy in the inner product. But if we could compute all possible outcomes in advance and have a "look-up table", the repetitive computing for the same input operands could be saved by looking up this "table". For the converter in PE, it is designed to set up such a "table". The "table" will be sent to IPUs and indexed by y_i. This solution brilliantly solves the redundancy problem. Besides, a long dependency chain for intermediate carry propagation is another challenge we face. Same to the previous idea, the authors use indexing instead of computing by computing two possible sums with opposite carry-in. The overhead for the dependency chain will be alleviated because we only have to **select** the correct answer by bit index without waiting for the computing chain.

However, Cambricon-P also has some drawbacks to this solution. For example, the exponential expansion of possible outcomes will burden the on-chip wire routing and total area cost. If the operand has hundreds of bits, the number of entries will EXPLODE to an unacceptable degree. But if we try to limit the bitwidth in a signal PE, the system could be compute-bounded because we do not have enough lanes. Besides, we should note that not every entry in this "look-up table" is useful. This may undermine the utilization of on-chip resources. Actually, it is easy to imagine that flip in carry bit is quite rare in possibility space. In most conditions, the carry-out will not be influenced by carry-in. If we can use other carry propagation methods, the dependency chain will be even shorter. 

I think this exhaustive search method is not elegant enough. In fact, the data redundancy can be solved in many ways. I propose the **result record table** as a substitute for the converter in Cambricon-P. Recent results could be reused if operands remain. This will be more efficient when bitwidth grows because the sparsity in the result table will show up. This offers better scalability than exponentially exploded indexes and the area can be even smaller. So IPUs will access the result record table and get their operand. Furthermore, the gather unit can also be optimized by the "**prediction-revision**" method. My idea is to limit the expansion of two possible carry-in numbers and believe the re-flip event is rare. So adjacent adders could form a local computing group and propagate their carry. If the prediction is wrong and may need to have a re-flip, correction can be easily done on the final result, because it is just incremental computing. For sure, my idea needs future research as proof, but the effort to limit exponential expansion is clear.

We have been discussing the above based on the bit-serial architecture. But actually, bit-serial computing will suffer a lot in bandwidth utilization and latency because of the low serial pump-in rate. This is a big challenge if we proceed to use this architecture. In my opinion, due to the balance between the storage pressure and inference latency, it is recommended to use batch computing as an alternative if the workload has the requirement for runtime latency. The moderate scale-out level could lead to more astonishing inference performance, while still ensuring the low on-chip buffer consumption and support for arbitrary precision. And we also can predict that with the batch process, selection redundancy will be exposed and sparsity can be more exploited to prune search space, which will be suitable for APC with large bitwidth.



