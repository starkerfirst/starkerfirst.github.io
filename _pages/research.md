---
layout: archive
title: "Research"
permalink: /Research/
author_profile: true
---

## Christina

### Background and Motivation

With the birth of GPT-4, an era of foundation model has already come to a reality. Recent research found that the bigger models get, the better emergent abilities they show. However, the explosion of parameters is a nightmare for hardware designers because of incompatible growth rates between models and 

Recent advances in foundation models \cite{foundation} have demonstrated that a model's representative power and transfer learning ability for multi-modality multi-task (MMMT) work is rising linearly with more parameters, which shows that the bigger models get, the better they achieve \cite{megatron,bigbird}. But with the fast increase of parameters, computation and storage explosion issues arose quickly. Sparse-activated models have been proposed to gain orders of magnitude fewer inference FLOPs with only input-related parameters utilized in computation while providing equal model quality to dense networks \cite{m3vit,switchtransformer,hydranet,gshard}. Their architecture is adaptive to its input tokens and may change its topology on run-time based on input features. Mixture of Experts (MoE) is one of the most popular implementations for sparse-activated neural networks derived from transformer model \cite{moe}. Its efficiency in the deployment of foundation models such as Large Language Model (LLM) and Vision Transformer (ViT) has been proved by more and more commercial products and academic research \cite{jiuzhang,yi2023edgemoe,cong2023enhancing,vmoe,shen2023mixtureofexperts,softmoe}.

However, the computation demand from foundation models including MoE has already grown to an unaccepted level with a giant incompatibility between hardware and model. Memory on only one commercial GPU is no longer adequate for inference on bulk parameters, and a multi-chip system is necessary for MoE inference to fit the model size. Distributed systems that contain multiple GPUs with dedicated huge HBM dies and inter-chip links with ultimate bandwidth are proposed with better system scalability and overall computing power, which respectively propels the further explosion of MoE parameters \cite{h100,switchtransformer}. However, because of the imbalance in communication and computation demands between existing hardware and models, previous works show that GPU hardware utilization of LLM inference falls below 20\% without extra model modification, which is caused directly by heavy data access and communication latency \cite{flexmoe, unleashing}. On current computing platforms, GPU scaling out may be not compelling due to low resource utilization and high fabrication costs. 

Also, MoE has quite low arithmetic intensity because of coarse-grained sparsity compared to traditional transformer models that today's GPU and static accelerator with or without sparse cores are inefficient to handle. They are designed to deploy static operator graphs without supporting operator sparsity. Unused experts are considered bubbles and cause idleness in hardware unless receiving control signals from remote CPUs to reconfigure task mapping, which is proposed in recent MoE inference platforms but increases MoE inference latency by a round-trip \cite{flexmoe}. In batched inference, this will lead to an unbalanced workload distribution among experts and GPUs, forcing latency as the maximum of all GPUs \cite{fastermoe,smartmoe,tutel}. 

Although dedicated adaptive accelerators like Adyna and DNA have been proposed, they target general medium-sized dynamic NNs and leave blanks in scale-out methods. When parameters of LLM rise at a shocking rate, new accelerator architecture must have sufficient support to elastically expand new resources into the system and face diverse demands from scalable billion-level models to efficient distilled models \cite{amd,nvidia}. However, we should notice that scalable architecture may lead to more complex scheduling problems, especially handling operator sparsity of MoE and reducing idleness. How to hand out computation tasks efficiently on an elastic system is the priority of deploying MoE.

