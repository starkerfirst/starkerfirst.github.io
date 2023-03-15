---
layout: archive
title: "Personal Statement"
permalink: /Intro/
author_profile: true
redirect_from:
  - /PS
---

My name is Bohan Yang. I am a junior majoring in microelectronics at the School of the Gifted Young, University of Science and Technology of China (USTC). Right now, I am seeking a Ph.D. position in computer architecture or AI chip design, since I have a strong interest in AI chip computing dataflow optimization and reconfigurability, especially for hardware acceleration of mechanisms such as attention and computing architecture in application scenarios such as cloud computing.

I first became interested in computer architecture while reading *Computer Organization and Design: The Hardware/Software Interface* as my undergraduate textbook. I was fascinated by the way that the computer was built up and facilitated the binary process inside that tiny chip. Looking at this sophisticated gadget, I wanted to understand how a human could build this up. So after that, I began learning related courses like *digital logical circuits*, *digital integrated circuits,* and *digital chip design* and gradually developed a systematic mindset. I started to consider the system from both a software and hardware perspective. Then I had my first chance to implement a RISC-V CPU in RTL format and drive it to do some tricks. My eagerness to try new methods to speed up the clock, cut down the area, and save energy saw me quickly progress through the project. As I moved from a simple trial to the more complicated task to design a chip and generate a complete GDS layout, my passion for exploring grew.

With my passion, I have participated in many research projects,  such as CUDA computing, TCP/IP packet catching,  tiny OS kernel building, IP core implementation, chip design, and architecture simulation. I studied such varied fields because I wanted to give myself a solid foundation and an overview of computer architecture, and at the same time figure out which field I would eventually focus on. 

Last summer, as an intern, I joined Polar Bear Tech for my summer internship. Since Polar Bear Tech is a startup company for chiplet design and integration, I learned a lot about the industrial workflow and the problems and solutions the industry cares about. As a member of the AI compiling research group, I gain a deeper understanding of how AI frameworks operate. There, I wrote domain-specific operators and tried to optimize them in C++. Also, I wrote Python scripts to verify the functionality of operators while ensuring the coverage rate. Watching the passing of such a large-scale operator verification is a great achievement and I was motivated to work on bigger projects.

Now, I work as a research intern under the guidance of Prof. Mingyu Gao at IIIS, Tsinghua University to seek a better accelerator architecture for dynamic neural networks. Reading papers with brilliant ideas to relieve all kinds of bottlenecks broadens my horizon. I am inspired by these thoughts and try to make use of them in my recent research. It is a hard task to implement my ideas at first because of the complexity, but I am doing my best to learn the tools to build up the whole system from nothing.  

In my observation, most of the research papers and frontier research hotspots tend to focus on relieving the incompatibility between computing and storage demands. For example, kinds of dataflows have been proposed to fully utilize data reuse, like ykp_os, kcp_ws (NVDLA), xp_ws, and rs (Eyeriss). Many data-moving strategies have also been invented like forwarding, buffer sharing, and multicast. Until today, this is still a hotspot. Also, with the rapid growth of AI applications and computing power demands, the current computing diagram is no longer suitable and efficient for the future. GPU, for instance, has been proven to be the most practical DNN training and inference platform so far, because of its tensor-friendly architecture (SIMD) compared to traditional CPU. However, it is still not perfect since there are growing special demands such as sparse computing, mixed precision, and dynamic branching. My current research is to facilitate dynamic neural networks by building multi-level computing blocks which can manipulate and hand out works on runtime, which fits this application better than GPUs. I quickly realized that I am excited to optimize applications from their underlying principles and platforms. And that is why architecture research is exactly the field I’m looking for.

My thoughts of the future are now crystal clear. I am determined to pursue my Ph.D. career in computer architecture, and I have the confidence to use my knowledge to facilitate computing-dense scenarios that will drive the next wave of the AI revolution. Also, I hope that my design will be applied to commercial use one day, which means my research is helpful to industry development. 

Right now, I am interested in the acceleration of attention mechanisms and pruning methods, especially those used for self-driving and edge computing. I believe both applications will become dazzling new stars that everyone knows and uses in the future. So high-efficiency, low-latency chips will have the critical position because the embedded system cannot tolerate high energy consumption and response latency. And I also believe universal AI accelerators like the Nvidia H100 are promising since "universal" indicates a huge development community and flexible application scenarios. In my opinion, commercial chips cannot be popular and successful until they can meet the needs of various production situations. So will some new platform take the place of traditional GPU in this Golden Age? That is what I am going to explore in my Ph.D. life.

Here is the end of my statement. Thank you very much for your time. If you are interested in the details of my projects, you can check them out in my GitHub repo or on my website.





