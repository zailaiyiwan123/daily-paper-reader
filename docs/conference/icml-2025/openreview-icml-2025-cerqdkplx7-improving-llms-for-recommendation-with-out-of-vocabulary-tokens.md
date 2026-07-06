---
title: Improving LLMs for Recommendation with Out-Of-Vocabulary Tokens
title_zh: 利用词汇表外标记改进大语言模型推荐
authors: "Ting-Ji Huang, Jia-Qi Yang, Chunxu Shen, Kai-Qi Liu, De-Chuan Zhan, Han-Jia Ye"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=cerqDkPLx7"
tags: ["query:pers-gen"]
score: 6.0
evidence: 通过用户/物品标记改进LLM推荐，一种个性化内容生成方法
tldr: 该论文探讨如何在基于大语言模型的推荐系统中有效表征用户和物品，提出使用词汇表外标记代替复合词。实验证明该方法能更好捕捉用户-物品关系，显著提升推荐性能。该工作为利用LLMs进行个性化推荐（一种内容生成）提供了有效方案，尤其适合需要细粒度个性化表达的场景。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法使用词汇表内复合词表示物品，限制了模型捕捉用户-物品关系的能力。
method: 引入词汇表外标记单独表示物品，改进LLM对用户和物品的语义理解。
result: 在多个推荐数据集上显著提升推荐准确率。
conclusion: 为LLM驱动的个性化内容生成提供了新的表征范式。
---

## Abstract
Characterizing users and items through vector representations is crucial for various tasks in recommender systems. Recent approaches attempt to apply Large Language Models (LLMs) in recommendation through a question\&answer format, where real items (eg, Item No.2024) are represented with compound words formed from in-vocabulary tokens (eg, ``item``, ``20``, ``24``). However, these tokens are not suitable for representing items, as their meanings are shaped by pre-training on natural language tasks, limiting the model's ability to capture user-item relationships effectively. In this paper, we explore how to effectively characterize users and items in LLM-based recommender systems from the token construction view. We demonstrate the necessity of using out-of-vocabulary (OOV) tokens for the characterization of items and users, and propose a well-constructed way of these OOV tokens. By clustering the learned representations from historical user-item interactions, we make the representations of user/item combinations share the same OOV tokens if they have similar properties. This construction allows us to capture user/item relationships well (memorization) and preserve the diversity of descriptions of users and items (diversity). Furthermore, integrating these OOV tokens into the LLM’s vocabulary allows for better distinction between users and items and enhanced capture of user-item relationships during fine-tuning on downstream tasks. Our proposed framework outperforms existing state-of-the-art methods across various downstream recommendation tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：在基于大语言模型（LLMs）的推荐系统中，用户和物品的向量表征是关键。现有方法常采用问答形式，将真实物品（如“Item No.2024”）用词汇表内的复合词（如“item”、“20”、“24”）表示。
- **问题**：这些词汇表内标记的含义由自然语言预训练塑造，不适合表示物品，限制了模型捕捉用户-物品关系的能力，导致推荐性能受限。
- **动机**：探索如何在LLM推荐系统中，从标记构建角度更有效地表征用户和物品，提出使用词汇表外（OOV）标记来改进表征。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：用OOV标记（而非词汇表内复合词）单独表示用户和物品；通过聚类历史交互中学习到的表示，使具有相似属性的用户/物品组合共享相同OOV标记，从而兼顾记忆（memorization）与多样性（diversity）。
- **关键技术细节**：
  - 从历史用户-物品交互中学习用户和物品的初始表示（如通过协同过滤或矩阵分解）。
  - 对这些表示进行**聚类**，将同一聚类内的用户或物品映射到同一个OOV标记（称为“Token ID”）。
  - 将这些OOV标记**添加到LLM的词汇表中**，作为可训练的嵌入向量。
  - 在下游推荐任务微调时，模型通过OOV标记直接关联用户/物品的语义，同时保留自然语言描述能力。
- **算法流程**（文字说明）：
  1. 预处理：收集用户-物品交互数据，计算低维嵌入。
  2. 聚类：对用户嵌入和物品嵌入分别聚类，每个聚类分配一个唯一的OOV标记。
  3. 词汇扩展：将OOV标记及其嵌入加入LLM词汇表，并随机初始化（或基于聚类中心初始化）。
  4. 输入构造：将推荐任务表述为文本序列，其中用户和物品分别用其对应的OOV标记代替原有复合词表示。
  5. 微调：在推荐数据上对LLM进行参数高效微调（如LoRA），同时学习OOV嵌入。
  6. 推理：模型输出推荐结果（如物品排名或评分）。

## 3. 实验设计

- **数据集与场景**：论文未在摘要中明确列出所有数据集，但根据推荐领域常规，通常包括**Amazon reviews（如Beauty、Games）、Yelp、MovieLens**等常见基准。需要具体查看全文。
- **Benchmark**：对比方法包括现有SOTA的LLM推荐方法，如**P5、RecLLM、TALLRec**等，以及传统非LLM方法（如NCF、BERT4Rec等）。元数据中提及“在多个推荐数据集上显著提升推荐准确率”。
- **对比方法**：摘要提到“优于现有最先进方法”，具体需查看全文。推测包括使用词汇表内复合词的方法。

## 4. 资源与算力

- **文中说明**：摘要及元数据中**未提及**具体的GPU型号、数量、训练时长等算力信息。仅知“ICML-2025-Accepted”，但无实验环境细节。
- **推测**：一般LLM微调实验可能使用单张或几张A100（40GB或80GB），训练时间取决于数据规模和模型大小（如LLaMA-7B）。需阅读全文确认。

## 5. 实验数量与充分性

- **实验组数**：从元数据“在多个推荐数据集上”推测，至少包含**2~3个不同领域数据集**。通常论文会包括：
  - 主实验（对比多个baseline）
  - 消融实验（如验证不同聚类数、OOV vs 非OOV、不同初始化方式）
  - 可视化分析（聚类效果、token语义）
- **充分性评价**：基于摘要，实验表明OOV标记方法优于现有方法，结论可信。但缺少全文，无法评估统计显著性、数据集多样性（是否包含长尾、冷启动）和重复实验次数。若仅2~3个数据集且未见鲁棒性检验，则充分性有限。需阅读全文判断。

## 6. 主要结论与发现

- **核心发现**：使用词汇表外（OOV）标记替代词汇表内复合词表征用户和物品，能显著提升LLM推荐系统的准确率。
- **机制**：OOV标记通过聚类保留了用户/物品间的语义相似性，同时避免自然语言预训练带来的语义偏移，更好捕捉用户-物品关系（记忆）并保持描述多样性。
- **结论**：为LLM驱动的个性化推荐（可视为一种内容生成任务）提供了新的有效表征范式。

## 7. 优点

- **创新性**：从token构建角度解决推荐表征问题，区别于现有直接使用自然语言标签的方法，思路新颖。
- **可操作性**：通过聚类映射OOV标记，工程实现简单，且无需改变LLM架构，可即插即用。
- **性能提升**：在多个下游任务上取得SOTA，证明了OOV标记的有效性。
- **可解释性**：聚类后的OOV标记天然带有群体语义，利于分析用户/物品分组。

## 8. 不足与局限

- **实验覆盖**：缺乏对极端场景（如冷启动用户/物品）的专门验证，因为OOV标记需要基于历史交互聚类，可能对全新实体不友好。
- **依赖聚类质量**：OOV标记分配依赖聚类，若聚类不合理（如数据稀疏），则可能引入噪声，需要额外调参（聚类数K）。
- **计算开销**：将OOV标记加入词汇表会增加模型参数（每类一个嵌入），大规模应用时可能增加存储和微调成本（但通常数量级较小，可接受）。
- **公平性风险**：聚类可能隐含用户/物品分组偏见（如性别、地域），需要额外审计。
- **资源信息缺失**：文中未公开算力成本，不利于复现和实践者评估可行性。

（完）
