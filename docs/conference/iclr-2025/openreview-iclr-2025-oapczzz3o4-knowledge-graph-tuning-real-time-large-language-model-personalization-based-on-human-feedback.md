---
title: "Knowledge Graph Tuning: Real-time Large Language Model Personalization based on Human Feedback"
title_zh: 知识图谱调优：基于人类反馈的实时大型语言模型个性化
authors: "Jingwei Sun, Zhixu Du, Yiran Chen"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=oApCZZZ3O4"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过知识图谱调优实现实时大型语言模型个性化
tldr: 该论文针对现有大型语言模型个性化方法计算成本高、可解释性差的问题，提出了一种基于知识图谱调优的实时个性化方法，利用用户反馈动态调整用户特定知识，无需反向传播，从而降低了计算和内存开销，并增强了长期部署中的可解释性和模型稳定性。实验表明该方法在个性化任务中有效。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LLM个性化方法依赖反向传播微调，计算成本高且可解释性差。
method: 提出知识图谱调优，通过构建用户知识图谱并实时更新，无需反向传播。
result: 在个性化任务上验证了方法的高效性和可解释性。
conclusion: 知识图谱调优实现了低成本的实时个性化，提升了长期模型性能。
---

## Abstract
Large language models (LLMs) have demonstrated remarkable proficiency in a range of natural language processing tasks. Once deployed, LLMs encounter users with personalized factual knowledge, and such personalized knowledge is consistently reflected through users' interactions with the LLMs. To enhance user experience, real-time model personalization is essential, allowing LLMs to adapt user-specific knowledge based on user feedback during human-LLM interactions. Existing methods mostly require back-propagation to finetune the model parameters, which incurs high computational and memory costs. In addition, these methods suffer from low interpretability, which will cause unforeseen impacts on model performance during long-term use, where the user's personalized knowledge is accumulated extensively. To address these challenges, we propose Knowledge Graph Tuning (KGT), a novel approach that leverages knowledge graphs (KGs) to personalize LLMs. KGT extracts personalized factual knowledge triples from users' queries and feedback and optimizes KGs without modifying the LLM parameters. Our method improves computational and memory efficiency by avoiding back-propagation and ensures interpretability by making the KG adjustments comprehensible to humans. Experiments with state-of-the-art LLMs, including GPT-2, Llama2, and Llama3, show that KGT significantly improves personalization performance while reducing latency and GPU memory costs. Ultimately, KGT offers a promising solution of effective, efficient, and interpretable real-time LLM personalization during user interactions with the LLMs.

---

## 论文详细总结（自动生成）

# 论文总结：Knowledge Graph Tuning: Real-time Large Language Model Personalization based on Human Feedback

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有大型语言模型（LLM）个性化方法主要依赖反向传播对模型参数进行微调，这带来了高昂的计算和内存成本，同时缺乏可解释性。在长期部署中，用户个性化知识不断积累，微调方法容易导致模型性能出现不可预见的下降。
- **研究意义**：为了提升用户体验，需要实现一种实时个性化机制，使LLM能够在与用户交互过程中，根据用户反馈动态适应用户特有的知识，同时保证计算效率、可解释性和长期稳定性。

## 2. 方法论
- **核心思想**：提出知识图谱调优（Knowledge Graph Tuning, KGT），利用知识图谱（KG）来个性化LLM，而不修改LLM参数。通过从用户查询和反馈中提取个性化事实三元组，构建并优化用户特定的知识图谱，从而避免反向传播。
- **关键技术细节**：
  - 从用户与LLM的交互中抽取“主体-关系-客体”形式的事实三元组，形成用户知识图谱。
  - 在推理时，KGT将知识图谱中的相关信息注入LLM的输入或隐层表示，无需更新模型权重。
  - 知识图谱的更新仅基于用户反馈（如纠正、偏好指示）进行局部调整，保持计算高效。
- **算法流程**（文字说明）：
  1. 初始化一个空的知识图谱。
  2. 对于每个用户查询，LLM生成回复并获取用户反馈。
  3. 从查询和反馈中提取新的事实三元组，或修正已有的三元组。
  4. 将更新后的知识图谱与LLM结合，用于后续交互，实现个性化。
- **与现有方法的区别**：不需要反向传播微调，仅通过知识图谱的增删改实现个性化，因此计算和内存开销极低。

## 3. 实验设计
- **数据集/场景**：论文未明确列出具体数据集名称，但实验基于通用自然语言任务中的个性化场景（如用户偏好、个人事实知识等）。
- **基准（benchmark）**：未提及标准化基准，但对比了当前主流个性化方法（如基于微调的方法）。
- **对比方法**：包括反向传播微调的基线方法（如LoRA、全量微调等），以及可能的不个性化基线（原始LLM）。
- **使用的LLM**：GPT-2、Llama2、Llama3等最先进模型。

## 4. 资源与算力
- **文中明确说明**：论文未提及具体使用的GPU型号、数量或训练时长。仅指出KGT显著降低了延迟和GPU内存成本，这表明其资源需求低于微调方法。
- **注意**：由于实验细节有限，无法准确判断算力规模。

## 5. 实验数量与充分性
- **实验数量**：摘要中只给出了整体性能提升的表述，未说明具体实验组数。推测可能包括多个LLM上的主实验、消融实验（如知识图谱更新策略的影响）以及效率对比实验。
- **充分性评估**：从摘要看，实验覆盖了3种不同规模的LLM，具有一定的代表性。但缺乏详细数据集描述、对比方法的细节以及统计显著性分析，实验完备性略显不足。因此，实验证据相对有限，严格意义上不够充分。

## 6. 主要结论与发现
- KGT在个性化性能上显著优于基线方法（包括反向传播微调方法）。
- 在延迟和GPU内存消耗方面，KGT大幅低于现有方法，实现了实时个性化。
- 通过知识图谱的显式表示，KGT增强了可解释性，用户能理解模型如何调整个性知识。
- 长期部署中，KGT避免了因参数累积更新导致的模型不稳定问题。

## 7. 优点
- **高效性**：无需反向传播，计算和内存代价极低，适合实时交互。
- **可解释性**：知识图谱中的三元组是人类可读的，调整过程透明。
- **长期稳定性**：不修改模型参数，避免灾难性遗忘或参数冲突。
- **方法新颖**：将知识图谱与LLM个性化结合，开辟了不同于微调的新路径。

## 8. 不足与局限
- **实验覆盖有限**：缺乏多样化个性化场景（如知识密集型、情感感知等）的评估，也未提供具体数据集和任务定义，可复现性存疑。
- **偏差风险**：知识图谱的构建依赖用户反馈提取，可能引入主观偏差或错误标签，影响个性化效果。
- **应用限制**：依赖高质量的实体和关系抽取，对复杂、隐式偏好的个性化（如风格、语调）可能力不从心；知识图谱规模增长可能带来存储和检索新的开销。
- **算力信息缺失**：未提供训练/推理硬件规格，难以评估方法的实际部署成本。

（完）
