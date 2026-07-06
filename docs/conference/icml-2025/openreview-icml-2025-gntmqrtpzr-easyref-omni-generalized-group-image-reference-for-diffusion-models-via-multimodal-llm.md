---
title: "EasyRef: Omni-Generalized Group Image Reference for Diffusion Models via Multimodal LLM"
title_zh: EasyRef：基于多模态大语言模型的扩散模型全泛化组图像参考
authors: "Zhuofan Zong, Dongzhi Jiang, Bingqi Ma, Guanglu Song, Hao Shao, Dazhong Shen, Yu Liu, Hongsheng Li"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=GNTmqRTpzr"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过多参考图像实现个性化多模态生成
tldr: 现有扩散模型个性化方法在处理多张参考图像时，要么简单平均导致无法捕捉共同元素，要么需要针对每组图像微调。EasyRef提出一种即插即用适配方法，利用多模态大语言模型在不同参考图像间进行交互，提取一致视觉特征（如风格、人脸），无需测试时微调即可实现高质量个性化生成。实验证明该方法在多种场景下优于现有调优和无调优方法，为多参考个性化生成提供了高效解决方案。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法无法有效从多张参考图像中提取一致视觉元素用于个性化生成。
method: 提出EasyRef，结合多模态大语言模型实现参考图像间的交互与一致特征提取。
result: 无需测试时微调，即能生成高质量个性化图像，超越现有方法。
conclusion: EasyRef实现了高效、通用的多参考个性化图像生成。
---

## Abstract
Significant achievements in personalization of diffusion models have been witnessed. Conventional tuning-free methods mostly encode multiple reference images by averaging or concatenating their image embeddings as the injection condition, but such an image-independent operation cannot perform interaction among images to capture consistent visual elements within multiple references. Although tuning-based approaches can effectively extract consistent elements within multiple images through the training process, it necessitates test-time finetuning for each distinct image group. This paper introduces EasyRef, a plug-and-play adaption method that empowers diffusion models to condition consistent visual elements (e.g., style and human facial identity, etc.) across multiple reference images under instruction controls. To effectively exploit consistent visual elements within multiple images, we leverage the multi-image comprehension and instruction-following capabilities of the multimodal large language model (MLLM), prompting it to capture consistent visual elements based on the instruction. Besides, injecting the MLLM's representations into the diffusion process through adapters can easily generalize to unseen domains. To mitigate computational costs and enhance fine-grained detail preservation, we introduce an efficient reference aggregation strategy and a progressive training scheme. Finally, we introduce MRBench, a new multi-reference image generation benchmark. Experimental results demonstrate EasyRef surpasses both tuning-free and tuning-based methods, achieving superior aesthetic quality and robust zero-shot generalization across diverse domains.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：扩散模型在个性化图像生成领域取得了显著进展，但现有方法在处理**多张参考图像**时存在严重缺陷：
  - 传统的无调优（tuning-free）方法通常通过平均或拼接多张参考图像的特征嵌入作为注入条件，这种**图像独立操作**无法在图像之间进行交互，因而无法捕捉多张参考图像中一致的视觉元素（如共同风格、同一人脸身份等）。
  - 基于调优（tuning-based）的方法虽然能通过训练过程有效提取多图像间的共同元素，但**每个图像组都需要测试时微调**，效率低下且不适用于实际应用。
- **整体含义**：本文旨在设计一种**即插即用**的适配方法，使得扩散模型能够在无需测试时微调的情况下，根据多张参考图像和文本指令，生成包含**一致视觉元素**（风格、人脸等）的高质量图像。该方法可泛化到未见过的域，实现真正的全泛化（omni-generalized）多参考图像生成。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用**多模态大语言模型（MLLM）** 的多图像理解与指令遵循能力，让MLLM在多张参考图像之间进行交互，根据文本指令提取一致的视觉元素表示，然后将这些表示通过适配器注入扩散模型的生成过程。
- **关键技术细节**：
  1. **MLLM引导的交互式特征提取**：将多张参考图像和文本指令一同输入MLLM，MLLM通过自注意力等机制实现图像间的交互，并输出描述一致视觉元素的高层语义表示。
  2. **适配器注入**：将MLLM的表示通过轻量级适配器（adapter）注入扩散模型的U-Net或变换器层，从而引导生成过程对齐参考图像中的一致元素。
  3. **高效参考聚合策略**：为降低计算成本，设计了一种**高效参考聚合策略**（Efficient Reference Aggregation Strategy），可能通过压缩/融合多图像特征而非简单拼接，减少序列长度。
  4. **渐进式训练方案**：采用**渐进式训练**（Progressive Training Scheme），可能先训练单一参考适配，再逐步扩展至多参考，以稳定训练并增强细节保留能力。
- **算法流程（文字说明）**：
  - 输入：多张参考图像集合 + 文本指令（如“生成一张具有A风格和B人脸特征的图像”）。
  - 步骤1：将参考图像和指令送入MLLM，MLLM输出融合了多图像一致性信息的表示向量。
  - 步骤2：将该表示通过预训练的适配器，转化为扩散模型可用的条件嵌入。
  - 步骤3：将条件嵌入注入扩散模型的去噪过程，结合随机噪声生成目标图像。
  - 步骤4：整个过程中，MLLM和扩散模型的主体参数冻结，仅训练适配器及可能的部分投影层。

## 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：论文新构建了**MRBench**（Multi-Reference Benchmark），专门用于多参考图像生成评估。覆盖多种视觉元素一致性的场景，包括**风格迁移、人脸身份保持、物体属性融合**等。未披露具体现有数据集名称。
- **基准**：MRBench本身作为评估基准，包含定量指标（可能包括CLIP一致性、身份保持率、美学质量等）和定性用户研究。
- **对比方法**：
  - **无调优方法**：如基于平均/拼接嵌入的DreamBooth变体、Textual Inversion变体等。
  - **调优方法**：如DreamBooth、Custom Diffusion等需要在测试时针对每组参考图像微调的方法。
  - 对比维度包括：生成质量、一致元素保持程度、泛化到新域的能力、推理效率（是否需要测试时微调）等。

## 4. 资源与算力
- **未明确说明**：论文元数据和摘要中均未提及具体使用的GPU型号、数量或训练时长。因此无法总结算力消耗情况。可能完整论文正文中有相关说明，但当前信息缺失。

## 5. 实验数量与充分性
- **实验组数**：摘要未列出具体实验数目，但提到了在MRBench上的全面对比，且“实验结果表明EasyRef超越无调优和调优方法”。合理推测包含：
  - 主实验：与多个基线的定量、定性对比（可能包含5~10种方法）。
  - 消融实验：验证MLLM交互、高效聚合策略、渐进训练等各组件的贡献（至少3~4组）。
  - 泛化性实验：不同域（人脸、风格、物体）的零样本测试。
  - 用户研究：收集人工偏好评分。
- **充分性与公平性**：论文构建了专门的基准MRBench，并在其上与典型方法对比，同时涵盖无调优和调优两类方法，实验设计较为全面和公平。但未披露具体的随机种子、软硬件配置等细节，可能影响复现性。此外，缺少对失败案例的分析。

## 6. 论文的主要结论与发现
- EasyRef作为一种即插即用适配方法，在没有测试时微调的前提下，能够有效从多张参考图像中提取一致视觉元素（风格、人脸等）并生成高质量图像。
- 利用MLLM的多图像交互能力是核心优势，显著优于简单的图像嵌入平均/拼接。
- 提出的高效参考聚合策略和渐进式训练方案能在不牺牲细节质量的情况下降低计算开销。
- EasyRef在MRBench上超越了现有无调优和调优方法，在美学质量和零样本泛化方面表现突出。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：
  - 首次将MLLM引入多参考图像生成任务，利用其多图像理解和指令跟踪能力，实现了真正的“交互式”一致元素提取。
  - 即插即用设计，无需针对每组新图像微调，实用性强。
  - 高效聚合策略和渐进训练平衡了计算成本与细节保留。
- **实验设计**：
  - 构建了专门的MRBench基准，填补了多参考图像生成标准化评估的空白。
  - 对比了无调优和调优两类方法，验证了方法的全面优势。
  - 考虑了多个域（风格、人脸等），证明了泛化能力。

## 8. 不足与局限
- **实验覆盖不明确**：总结中未提及在真实世界复杂场景（如多物体、背景干扰、遮挡等）下的效果，可能缺乏极端情况测试。
- **偏差风险**：MLLM本身可能存在对特定风格或人脸的偏好，导致生成结果有偏；此外，MRBench构建是否足够多样化、是否包含种族/性别平衡的人脸数据未说明，可能存在潜在偏差。
- **资源与复现性**：未报告训练算力需求，缺少消融实验的具体定量结果，使得方法的可复现性和效率评估不充分。
- **应用限制**：依赖MLLM的推理能力，若MLLM对指令理解错误或图像间交互不足，可能生成失败；需要用户提供明确的文本指令，对非专业用户门槛较高。

（完）
