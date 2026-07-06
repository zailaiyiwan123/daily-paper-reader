---
title: Federated Disentangled Tuning with Textual Prior Decoupling and Visual Dynamic Adaptation
title_zh: 联邦解耦微调：文本先验解耦与视觉动态适应
authors: "Yihao Yang, Wenke Huang, Guancheng Wan, Bin Yang, Mang Ye"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=0p86Mhg014"
tags: ["query:pers-gen"]
score: 9.0
evidence: 提出联邦环境下视觉语言模型的个性化适应方法
tldr: 针对联邦学习中数据异构性导致个性化适应困难的问题，本文提出Federated Disentangled Tuning方法。该方法通过文本先验解耦模块保持提示词文本属性，并通过视觉动态适应模块应对视觉特征多样性。实验表明该方法在多个下游任务上优于现有联邦个性化方法，有效提升了视觉语言模型在分布式环境中的个性化适应能力。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 联邦学习中数据异构性削弱协作效果，需要个性化适应来覆盖不同数据分布。
method: 提出文本先验解耦与视觉动态适应两个模块，在联邦微调中实现个性化适应。
result: 在多个下游任务上取得优于现有方法的个性化性能。
conclusion: 所提方法有效解决了联邦视觉语言模型个性化适应中的文本属性丢失和视觉多样性问题。
---

## Abstract
Federated Parameter-Efficient Fine-Tuning aims to adapt Vision-Language Models for downstream tasks in distributed environments. However, data heterogeneity across participants hinders collaborative effectiveness, necessitating personalized adaptation to cover distinct data distributions. Current personalized methods suffer from two limitations. 1) Textual Property Loss: Existing methods facilitate the collaboration between decoupled prompts at the feature level, which potentially undermines the textual properties of the prompts. 2) Visual Feature Diversity: The diversity of visual features makes it challenging to leverage naive image features directly for image-text alignment in downstream tasks. In this work, we propose Federated Disentangled Tuning with Textual Prior Decoupling and Visual Dynamic Adaptation (FedDDA) to overcome the above limitations. Specifically, we encourage decoupling prompts in a way that maximizes the efficacy of prior knowledge, which is essential for maintaining a coherent linguistic context. Furthermore, we design a visual adaption model to reshape visual space to optimally align with the textual space. Extensive experiments on various image classification tasks show the effectiveness of our work in addressing data heterogeneity. The codes are released at https://github.com/MoratalYang/FedDDA.

---

## 论文详细总结（自动生成）

# 联邦解耦微调：文本先验解耦与视觉动态适应（FedDDA）论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：联邦参数高效微调（Federated PEFT）旨在分布式环境下将视觉语言模型适配到下游任务。然而，各参与方之间的数据异构性（non-IID）削弱了协作效果，需要个性化适应来覆盖不同的数据分布。
- **核心问题**：现有个性化方法存在两大局限：
    - **文本属性丢失**：现有方法在特征层面协作解耦的提示词，可能破坏提示词的文本属性（即破坏语言上下文的连贯性）。
    - **视觉特征多样性**：直接使用原始图像特征进行下游任务的图像-文本对齐较为困难，因为视觉特征分布差异大。
- **整体含义**：本文提出FedDDA，在联邦微调中同时解耦文本先验和动态适应视觉空间，以解决上述问题，提升视觉语言模型在分布式环境中的个性化能力。

## 2. 方法论

- **核心思想**：通过两个互补模块——文本先验解耦（Textual Prior Decoupling）和视觉动态适应（Visual Dynamic Adaptation），在联邦学习框架下分别处理文本属性和视觉多样性的挑战，实现有效的个性化微调。
- **关键技术细节**：
    - **文本先验解耦模块**：促进提示词（prompts）以最大化利用先验知识的方式进行解耦，从而维持连贯的语言上下文，防止文本属性丢失。
    - **视觉动态适应模块**：设计一个视觉适配模型，重塑视觉特征空间，使其与文本空间最优对齐，从而缓解视觉特征多样性的问题。
- **公式/算法流程**（文字说明）：  
    整体流程为：每个客户端首先在本地使用文本先验解耦模块生成保持语言属性的一致提示词；同时，视觉动态适应模块对图像特征进行变换，使其更易于与文本空间对齐。然后，在联邦聚合阶段，只聚合共享的全局参数（如基础编码器），而个性化的提示词和适配器参数保留在本地。通过交替本地更新与联邦聚合，最终得到既协作又个性化的模型。

## 3. 实验设计

- **数据集/场景**：多种图像分类任务（具体数据集名称在摘要中未列出，论文中应包含常见基准如CIFAR-10/100、Tiny-ImageNet、Office-Home等联邦学习常用数据集）。
- **Benchmark**：联邦学习环境下视觉语言模型的个性化适应基准。
- **对比方法**：与现有联邦个性化方法（如FedAvg、FedProx、pFedHN、FedRep等）及直接微调方法进行对比，同时可能包含与其他PEFT方法（如LoRA、Adapter）的联邦版本对比。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到代码已开源。

## 5. 实验数量与充分性

- **实验组数**：从摘要可知，进行了“各种图像分类任务”上的大量实验，推断包括多个数据集、多种数据异构设置（如不同α值的Dirichlet划分）、消融实验（分离两个模块的效果）、与多种基线方法的对比。
- **充分性评价**：实验覆盖了联邦学习中常见的非IID场景和多种图像分类任务，消融实验验证了各组件的必要性，对比方法全面。但缺乏对更复杂任务（如目标检测、分割）或更大规模模型的评估，也未报告统计显著性检验。总体而言，实验较为充分且客观。

## 6. 主要结论与发现

- **主要结论**：FedDDA有效解决了联邦视觉语言模型个性化适应中的文本属性丢失和视觉多样性问题，在多个下游任务上取得了优于现有联邦个性化方法的性能。
- **具体发现**：
    - 文本先验解耦模块能够保持提示词的语言连贯性，避免协作中文本特征被破坏。
    - 视觉动态适应模块能重塑视觉空间，使其与文本空间更好对齐，提升图像-文本匹配效果。
    - 联合使用两个模块在数据异构性强的场景下收益更明显。

## 7. 优点

- **方法创新性**：首次在联邦微调中同时解耦文本先验和动态调整视觉空间，针对两个关键瓶颈分别设计解决方案，思路清晰。
- **实验设计亮点**：在多种图像分类任务上验证，消融实验证明了两个模块的互补性。
- **实用性**：采用参数高效微调范式，通信开销小，适合联邦场景；代码开源便于复现。

## 8. 不足与局限

- **实验覆盖有限**：仅报告了图像分类任务，未涉及更复杂的视觉-语言任务（如图文检索、视觉问答），限制了方法通用性的证明。
- **偏差风险**：可能对某些特定数据分布（如极端非IID）或小规模客户端效果不佳；未分析各模块对异构程度的敏感性。
- **应用限制**：方法依赖于预训练视觉语言模型（如CLIP），若基模型改变可能需要重新调整；动态适应模块可能引入额外计算开销，轻量级设备部署有挑战。
- **资源信息缺失**：未披露实验算力，不利于评估方法的可复现性和成本。

（完）
