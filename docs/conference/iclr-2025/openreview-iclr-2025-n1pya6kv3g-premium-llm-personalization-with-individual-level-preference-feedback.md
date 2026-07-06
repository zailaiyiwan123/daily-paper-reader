---
title: "PREMIUM: LLM Personalization with Individual-level Preference Feedback"
title_zh: PREMIUM：基于个体偏好反馈的大语言模型个性化
authors: "Yihang Sun, Tao Feng, Ge Liu, Jiaxuan You"
date: 2024-09-26
pdf: "https://openreview.net/pdf?id=N1pya6kv3g"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过标签系统和偏好排序实现LLM个性化
tldr: 该论文提出PREMIUM框架，利用标签系统高效刻画用户画像，并基于偏好排序实现LLM个性化。该框架支持本地部署，兼顾隐私与动态偏好适应，在多种个性化任务上表现优异。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LLM个性化方法难以本地部署，存在计算成本高、隐私风险大、无法适应动态偏好等问题。
method: 引入标签系统表征用户画像，结合个性化排序方法实现轻量级个性化。
result: 在多个个性化基准上，PREMIUM以更低的计算成本取得了与微调方法相当的性能。
conclusion: 标签系统为LLM个性化提供了一种高效且保护隐私的解决方案。
---

## Abstract
With an increasing demand for LLM personalization, various methods have been developed to deliver customized LLM experiences, including in-context learning, retrieval augmentation, and parameter-efficient fine-tuning. However, most existing methods are not readily locally deployable, limited by the compute cost, privacy risks, and an inability to adapt to dynamic user preferences. Here, we propose to use a tag system to efficiently characterize user profiles, inspired from the insights from personality typology and recommendation systems. Based on the observation, we present a locally deployable LLM-agnostic framework for achieving LLM personalization: $\textbf{PREMIUM}$ ($\textbf{P}$reference $\textbf{R}$anking $\textbf{EM}$powered $\textbf{I}$ndividual $\textbf{U}$ser $\textbf{M}$odeling), which obtains individual-level feedback by having users rank responses and continuously self-iterates optimization during the interaction between the user and the LLM. Notably, a variant of PREMIUM, PREMIUM-Embed, can effectively capture user preferences while being deployable with laptop-level resources. Besides algorithmic innovation, we further prepare a novel dataset, Ranking-TAGER, which provides a valuable evaluation protocol for LLM personalization. Extensive experiments validate that PREMIUM remarkably outperforms various baselines, achieving a 15\%-50\% higher accuracy and a 2.5\%-35\% higher win rate on Ranking-TAGER, as well as a 3\%-13\% higher accuracy and a 2\%-7.5\% higher F1 Score on LaMP-2. More importantly, we further demonstrate that PREMIUM can develop an effective strategy with minimal interactive data, adapt to dynamic user preferences, and demonstrate excellent scalability in both scale and functionality.

---

## 论文详细总结（自动生成）

## 1. 论文核心问题与整体含义

- **研究动机**：现有LLM个性化方法（如上下文学习、检索增强、参数高效微调）大多依赖云端部署，存在计算成本高、隐私风险大、无法动态适应用户偏好变化等问题。
- **核心问题**：如何在不依赖大规模微调或云端算力的前提下，实现高效、隐私友好、可本地部署的LLM个性化？
- **整体含义**：通过引入标签系统高效刻画用户画像，并结合偏好排序（Ranking）反馈，使LLM能够以轻量级方式持续学习个体用户偏好，同时支持本地部署（甚至笔记本级资源），兼顾隐私与动态适应性。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：借鉴人格类型学与推荐系统中的标签思想，设计一个用户画像标签系统（Tag System），将用户偏好抽象为可量化的标签向量；然后通过用户对LLM输出的排序反馈，迭代优化标签权重，从而引导LLM生成更符合用户偏好的回复。
- **技术流程**（文字说明）：
  1. **标签系统初始化**：定义一组通用标签（如风格、语气、详细程度、立场等），每个标签对应一个可训练的权重向量。
  2. **用户反馈收集**：每次交互时，LLM生成多个候选回复，用户对这些回复进行排序（rank），作为偏好信号。
  3. **偏好建模与优化**：基于排序结果，使用对比学习或排序损失函数（如ListMLE、RankNet）更新标签权重，使得高排位回复对应的标签权重被强化，低排位回复对应权重被削弱。
  4. **推理时个性化**：给定用户查询，通过标签系统计算当前用户的偏好向量，并对LLM的输出进行重排序或直接调整生成分布，使得输出与用户画像对齐。
- **关键变体**：PREMIUM-Embed，使用嵌入层替代全连接层，进一步降低参数规模，可部署在笔记本级硬件上。
- **公式与算法**：文中未给出具体公式，但核心为基于排序反馈的标签权重更新机制，类似记忆网络中的注意力更新过程。

## 3. 实验设计：数据集、Benchmark与对比方法

- **数据集**：
  - **Ranking-TAGER**：作者新构建的个性化评估数据集，包含用户对LLM回复的排序标签。
  - **LaMP-2**：现有个性化基准数据集（用户身份与偏好分类任务）。
- **Benchmark**：在Ranking-TAGER上评估准确率和胜率（Win Rate）；在LaMP-2上评估准确率和F1分数。
- **对比方法**：
  - 基准：无个性化（No Personalization）、上下文学习（ICL）、检索增强（RAG）、参数高效微调（LoRA等）。
  - 消融：PREMIUM完整版、PREMIUM-Embed（笔记本级部署版）、去除标签系统或去除排序反馈的变体。

## 4. 资源与算力

- 论文中明确提及：PREMIUM-Embed可在**笔记本级资源**（如单GPU或仅CPU）上部署运行。
- 未明确说明训练使用的具体GPU型号、数量或训练时长。仅在能耗比较中提到PREMIUM的计算成本远低于微调方法，但未给出具体硬件配置。

## 5. 实验数量与充分性

- **实验组数**：主要包括：
  - 在Ranking-TAGER上的准确率与胜率对比（5种以上基线）。
  - 在LaMP-2上的准确率与F1对比。
  - 消融实验（标签系统、排序反馈、不同冷启动策略）。
  - 动态偏好适应性实验（模拟用户偏好漂移）。
  - 数据效率实验（不同交互轮次下的性能）。
  - 扩展性实验（功能扩展如风格迁移）。
- **充分性评价**：实验覆盖了主要基准和关键变量（数据量、动态性、资源开销），消融设计合理，对比方法覆盖主流方案。但缺乏在更多真实用户场景（如多轮对话）和更大规模模型（如70B以上）上的验证，可能限制结论的泛化性。总体实验设计客观且较为充分。

## 6. 主要结论与发现

- **性能优势**：PREMIUM在Ranking-TAGER上比最佳基线准确率高15%-50%，胜率高2.5%-35%；在LaMP-2上准确率高3%-13%，F1高2%-7.5%。
- **轻量级部署**：PREMIUM-Embed在笔记本级资源上即可运行，性能接近完整版。
- **高效数据利用**：仅需少量交互数据（如10-20次排序）即可学到有效策略。
- **动态适应**：能快速跟踪和适应用户偏好的变化（如用户口味改变后重新收敛）。
- **可扩展性**：可扩展到不同模型规模和功能任务（如风格控制）。

## 7. 优点

- **创意亮点**：将标签系统与偏好排序相结合，形成一种无需微调、可本地部署的个性化框架，兼顾隐私与计算效率。
- **实验设计**：构建了专门的排序数据集Ranking-TAGER，填补了LLM个性化中排序反馈评估的空白；动态偏好实验验证了框架的适应性。
- **实用性**：提出的PREMIUM-Embed可以直接部署在普通笔记本上，降低了应用门槛。
- **结论可靠**：对比方法覆盖全面，消融实验清晰验证了各模块贡献。

## 8. 不足与局限

- **实验覆盖**：仅使用两个数据集（一个自建、一个公开），缺乏在更多个性化任务（如推荐解释、对话系统）上的验证；用户群体可能较单一（如学术标注者），存在样本偏差。
- **资源细节缺失**：未提供训练时具体GPU型号、时长，不利于复现和能耗比较。
- **标签系统设计**：标签的定义和数量依赖于人为设定，可能存在主观性；不同领域可能需要重新设计标签体系。
- **排序反馈效率**：用户需要每次对多个回复进行排序，可能增加交互负担；文中未讨论标签冲突或用户随机排序等噪声问题。
- **应用限制**：仅适用于单用户独立场景，不支持多用户共享模型时的并行个性化；未考虑非英语语言场景。

（完）
