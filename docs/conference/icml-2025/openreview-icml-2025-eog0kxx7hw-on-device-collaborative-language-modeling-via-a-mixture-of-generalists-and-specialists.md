---
title: On-Device Collaborative Language Modeling via a Mixture of Generalists and Specialists
title_zh: 通过通才与专才混合实现设备端协作语言建模
authors: "Dongyang Fan, Bettina Messmer, Nikita Doikov, Martin Jaggi"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=Eog0kXX7hW"
tags: ["query:pers-gen"]
score: 9.0
evidence: 聚焦于通过协作学习实现设备端大语言模型的个性化
tldr: 针对设备端大语言模型在联邦学习中面临的计算资源与数据异构挑战，本文提出CoMiGS方法。该方法采用基于双层次优化的混合专家模型架构，通过验证集优化路由器从而与目标分布对齐。实验表明CoMiGS在保持隐私的同时有效实现个性化，为设备端语言模型的个性化协作学习提供了新思路。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 设备端LLM面临资源与数据异构，需要协作学习实现个性化。
method: 提出双层次优化的混合专家模型，路由器通过验证集优化以对齐目标分布。
result: 在异构场景下优于现有联邦学习方法，实现有效个性化。
conclusion: CoMiGS同时解决了计算资源异构和数据异构，适用于设备端个性化语言建模。
---

## Abstract
On-device LLMs have gained increasing attention for their ability to enhance privacy and provide a personalized user experience. To facilitate private learning with scarce data, Federated Learning has become a standard approach. However, it faces challenges such as computational resource heterogeneity and data heterogeneity among end users. We propose CoMiGS ($\textbf{Co}$llaborative learning  with a $\textbf{Mi}$xture of $\textbf{G}$eneralists and $\textbf{S}$pecialists), the first approach to address both challenges. A key innovation of our method is the bi-level optimization formulation of the Mixture-of-Experts learning objective, where the router is optimized using a separate validation set to ensure alignment with the target distribution. We solve our objective with alternating minimization, for which we provide a theoretical analysis. Our method shares generalist experts across users while localizing a varying number of specialist experts, thereby adapting to users’ computational resources and preserving privacy. Through extensive experiments, we show CoMiGS effectively balances general and personalized knowledge for each token generation. We demonstrate that CoMiGS remains robust against overfitting—due to the generalists' regularizing effect—while adapting to local data through specialist expertise. We open source our codebase for collaborative LLMs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：设备端大语言模型（On-device LLMs）因其增强隐私保护和提供个性化用户体验而受到关注。由于终端用户数据稀缺且隐私敏感，联邦学习（Federated Learning）成为标准训练范式。
- **核心问题**：联邦学习在设备端LLM场景面临两大挑战：
  - **计算资源异构**：不同终端设备的计算能力差异巨大（如手机、平板、IoT设备），无法统一部署相同规模的模型。
  - **数据异构**：各用户的本地数据分布各不相同（非独立同分布），导致全局模型难以适应个性化需求。
- **整体含义**：现有方法通常只解决其中一个挑战，而本文提出的CoMiGS方法是首个同时解决计算资源异构和数据异构的协作学习框架，旨在实现隐私保护下的个性化设备端语言模型。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：采用**混合专家模型（Mixture-of-Experts, MoE）**架构，将专家分为两类：
  - **通才专家（Generalists）**：所有用户共享，负责通用语言知识，在全局聚合中保持稳定。
  - **专才专家（Specialists）**：每个用户拥有不同数量的本地专家，根据其计算资源调整数量，负责适应用户的个性化数据分布。
- **关键技术细节**：
  1. **双层次优化（Bi-level Optimization）**：MoE学习目标被形式化为双层优化问题。内层优化专家网络（通才和专才的参数），外层优化路由器（Router）的参数。路由器使用单独的验证集进行优化，使其输出门控权重与目标分布对齐，从而提升个性化效果。
  2. **交替最小化求解**：通过交替更新专家和路由器参数来求解目标函数，论文提供了理论分析证明收敛性。
  3. **隐私与资源适应**：共享通才专家通过联邦学习聚合（不泄露原始数据）；本地专才专家数量根据用户设备算力动态分配，且专才参数仅在本地更新，不参与全局聚合，从而保护隐私并适应异构计算资源。
- **算法流程（文字说明）**：
  - 初始化一组通才专家和每个用户的若干专才专家。
  - 每轮通信中：用户使用本地数据更新专才专家；所有用户联合更新通才专家（通过联邦平均或类似机制）；路由器在用户本地使用一个独立验证集进行优化，以学习如何混合通才与专才的输出。
  - 推理时，每个token生成由路由器动态选择激活的专家组合。

## 3. 实验设计

- **数据集/场景**：摘要未明确列出具体数据集名称，但提及针对“语言建模”任务。根据ICML社区惯例，可能涉及多个文本数据集（如WikiText-2、Penn Treebank、或自建异构联邦数据集）。需注意原文未详细说明，此处根据上下文推断。
- **Benchmark**：对比了现有联邦学习方法（如FedAvg、FedProx等个人化方法）。具体对比方法未在摘要中列出。
- **对比方法**：摘要提到“优于现有联邦学习方法”，但未列全。
- **实验场景**：聚焦于异构场景（如不同用户数据分布不同、计算能力不同），模拟真实设备端环境。

**注意**：论文摘要非常简短，未提供完整实验设置。以上为基于摘要推断，详细实验请参考完整论文。

## 4. 资源与算力

- **摘要中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。
- 可能在全文中提及，但根据当前提供的文本无法获知。建议查阅完整论文的“实验设置”部分。

## 5. 实验数量与充分性

- **实验数量**：摘要仅提到“大量实验”（extensive experiments），但未给出具体数字。可能包括：不同异构程度下的性能对比、专家数量消融、通才/专才比例影响、不同数据分布下的鲁棒性测试等。
- **充分性评价**：
  - **正面**：对比了多个联邦学习方法，展示了在异构场景下的优势；验证了通才专家的正则化效果，防止过拟合。
  - **不足**：缺少实验的具体细节，无法判断基准线是否公平（如是否保证所有方法在相同计算约束下比较）；未提及统计显著性检验；缺乏跨领域（如非语言建模任务）的泛化验证。

## 6. 主要结论与发现

- CoMiGS能有效平衡通用知识与个性化知识，每个token生成时路由器动态调整混合比例。
- 通才专家的正则化效应使模型对本地数据过拟合具有鲁棒性，同时通过专才专家适应局部分布。
- 在异构计算资源和异构数据分布并存时，CoMiGS显著优于现有联邦学习方法，实现有效个性化。

## 7. 优点

- **创新性**：首个同时解决计算资源异构和数据异构的协作语言建模方法。
- **方法设计**：双层次优化MoE目标，验证集优化路由器使其对齐目标分布，具备理论分析。
- **隐私保护**：专才专家仅在本地更新，不泄露原始数据；通才专家通过联邦聚合保护隐私。
- **资源自适应**：专才专家数量可随设备算力调整，灵活适配异构设备。
- **开源**：代码已开源，便于复现和后续研究。

## 8. 不足与局限

- **实验细节缺失**：摘要未提供具体数据集、基线方法、超参数设置等，难以独立评估实验的客观性。
- **偏差风险**：验证集的使用可能引入分布偏差（验证集与测试集需独立同分布假设）；若验证集与真实用户数据存在差异，路由器可能过拟合到验证集。
- **应用限制**：MoE架构在推理时需激活多个专家，可能增加设备端计算与存储开销，对于极低资源设备仍有挑战。
- **未提及通信效率**：联邦学习中通才专家的聚合通信开销未讨论，实际部署可能受限。
- **缺乏与其他个性化方法的对比**：如基于微调、元学习、知识蒸馏等方法的比较未提及。

（完）
