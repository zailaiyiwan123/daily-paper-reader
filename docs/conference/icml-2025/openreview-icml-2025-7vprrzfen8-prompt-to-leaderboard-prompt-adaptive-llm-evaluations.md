---
title: "Prompt-to-Leaderboard: Prompt-Adaptive LLM Evaluations"
title_zh: Prompt-to-Leaderboard：提示自适应的大语言模型评估
authors: "Evan Frick, Connor Chen, Joseph Tennyson, Tianle Li, Wei-Lin Chiang, Anastasios Nikolas Angelopoulos, Ion Stoica"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=7VPRrzFEN8"
tags: ["query:pers-gen"]
score: 8.0
evidence: 提示自适应LLM评估，为用户特定提示生成个性化排行榜
tldr: 传统LLM评估聚合了所有用户和提示，忽略了个体差异。P2L方法训练一个LLM根据自然语言提示输出Bradley-Terry系数，从而生成针对特定提示的排行榜。该排行榜支持无监督的任务特定评估、查询最优路由和个性化。在Chatbot Arena数据上，P2L展现了有效的个性化评估能力。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有LLM评估忽略用户和提示差异，无法提供个性化模型比较。
method: 训练一个LLM以自然语言提示为输入，输出Bradley-Terry系数向量，生成提示相关的排行榜。
result: 在Chatbot Arena数据上成功生成个性化排行榜，支持任务特定评估和模型路由。
conclusion: P2L为LLM评估提供了个性化维度，有助于用户选择最适合其需求的模型。
---

## Abstract
Large language model (LLM) evaluations typically rely on aggregated metrics like accuracy or human preference, averaging across users and prompts. This averaging obscures user- and prompt-specific variations in model performance. To address this, we propose Prompt-to-Leaderboard (P2L), a method that produces leaderboards specific to a prompt or set of prompts. The core idea is to train an LLM taking natural language prompts as input to output a vector of Bradley-Terry coefficients which are then used to predict the human preference vote. The resulting prompt-dependent leaderboards allow for unsupervised task-specific evaluation, optimal routing of queries to models, personalization, and automated evaluation of model strengths and weaknesses. Data from Chatbot Arena suggest that P2L better captures the nuanced landscape of language model performance than the averaged leaderboard. Furthermore, our findings suggest that P2L's ability to produce prompt-specific evaluations follows a power law scaling similar to that observed in LLMs themselves. In January 2025, the router we trained based on this methodology achieved the \#1 spot on the Chatbot Arena leaderboard. Our code is available at this GitHub link: https://github.com/lmarena/p2l.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有的大语言模型（LLM）评估方法通常依赖于平均后的指标（如准确率或人类偏好），这些指标对所有用户和提示进行汇总，从而掩盖了模型在不同用户特定提示下的性能差异。用户可能关心特定任务或查询类型的模型表现，但传统排行榜无法提供这种个性化信息。
- **整体含义**：提出一种**提示自适应评估**范式，使排行榜能根据输入的自然语言提示动态生成，从而更准确地反映模型在具体场景下的相对优势。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：训练一个 LLM（称为 P2L 模型），以自然语言提示作为输入，输出该提示对应的 **Bradley-Terry 系数向量**。该系数向量用于预测人类偏好的投票结果，进而生成针对该提示的个性化排行榜。
- **关键技术细节**：
  - **输入**：任意自然语言提示（例如“写一首关于星空的诗”）。
  - **输出**：一个向量，每个分量代表一个模型在该提示下的 Bradley-Terry 强度参数。
  - **训练**：使用人类偏好数据（如 Chatbot Arena 中的 pairwise 投票），让 P2L 模型学习从提示到系数向量的映射。损失函数基于 Bradley-Terry 模型的似然函数，使得预测的系数能最大化观察到的人类投票概率。
  - **应用场景**：生成的提示特定排行榜可用于：
    - 无监督的任务特定评估；
    - 最优查询路由（将用户的提示路由到预测表现最好的模型）；
    - 个性化模型选择；
    - 自动分析模型的优势与弱点。
- **公式/算法流程**（文字说明）：
  1. 收集大量（提示，模型对，人类偏好）三元组。
  2. 对于每个提示 \(x\)，假设模型 \(i\) 和 \(j\) 的偏好概率为 \(P(i \succ j | x) = \frac{\exp(\beta_i(x))}{\exp(\beta_i(x))+\exp(\beta_j(x))}\)，其中 \(\beta_i(x)\) 是 P2L 模型输出的模型 \(i\) 的 Bradley-Terry 系数。
  3. 训练 P2L 模型（通常是一个基于 Transformer 的语言模型），通过最大化所有人类投票的对数似然来学习映射。

### 3. 实验设计：使用了哪些数据集 / 场景，基准是什么，对比了哪些方法

- **数据集**：只明确提到了 **Chatbot Arena** 数据（人类偏好投票数据）。
- **基准（Benchmark）**：以 Chatbot Arena 上的平均排行榜（即对所有提示和用户投票求平均后的传统排行榜）作为基线。
- **对比方法**：直接对比了 P2L 生成的提示特定排行榜与平均排行榜在捕捉模型性能差异上的效果。此外，基于此方法训练的路由模型在 **Chatbot Arena leaderboard** 上获得了 **#1 排名**（截至 2025 年 1 月），间接证明了方法的有效性。
- **场景评估**：包括任务特定评估、查询最优路由、个性化等，但未给出独立于 Chatbot Arena 的额外基准数据集。

### 4. 资源与算力

- **文中未明确说明**训练 P2L 模型使用了多少 GPU、型号、训练时长或数据量。元数据中也没有补充算力信息。因此无法总结具体资源消耗。

### 5. 实验数量与充分性

- **实验数量**：论文主要展示了在 Chatbot Arena 单一数据源上的结果。虽然包含了路由模型在公开排行榜上的表现，但缺少多个不同领域/任务数据集的系统性实验。
- **充分性评估**：
  - **优点**：使用了真实人类偏好数据（Chatbot Arena），具有现实意义；路由模型的实际部署排名也提供了较强的实证。
  - **不足**：实验覆盖范围有限，仅依赖一个数据源，可能无法充分证明 P2L 在不同提示分布和用户群体下的泛化能力。消融实验（如不同模型架构、不同输入表示）未在摘要中提及。
  - **客观性与公平性**：对比的是平均排行榜，属于最直接且公平的基线。但未比较其他个性化评估方法（如基于聚类或元学习的方案），因此对比范围的广度有待加强。

### 6. 论文的主要结论与发现

- P2L 能够生成**提示自适应的排行榜**，比平均排行榜更好地捕捉模型性能的细微差异。
- P2L 的提示特定评估能力遵循**幂律缩放规律**（类似于大语言模型本身的表现），说明随着模型规模或数据量增加，个性化评估能力提升。
- 基于 P2L 训练的路由模型在 Chatbot Arena 排行榜上取得了**第一名**，验证了方法的实际效用。
- 该方法支持无监督的任务特定评估、最优路由、个性化以及自动分析模型优缺点，为 LLM 评估提供了新的个性化维度。

### 7. 优点：方法或实验设计上的亮点

- **个性化评估**：直接根据自然语言提示输出系数，无需人工定义任务类别，自动化程度高。
- **灵活性**：生成的排行榜可应用于多种下游任务（路由、个性化、弱点分析），实用性广。
- **理论基础**：基于 Bradley-Terry 模型，有明确的概率解释和似然优化目标。
- **实证验证强**：真实路由模型在公开排行榜夺冠，证明方法在真实部署中有效。
- **缩放规律发现**：揭示了个性化评估能力随规模提升的规律，具有潜在的科学价值。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：仅使用 Chatbot Arena 数据，未在多个领域（如代码、数学、多语言）或多种提示分布下验证，可能限制了结论的泛化性。
- **偏差风险**：Chatbot Arena 的投票数据可能本身存在用户偏好偏差（如对特定模型或回答风格的偏好），P2L 学到的映射可能继承这些偏差，导致对特定提示的排名不一定客观。
- **计算开销**：虽然未报告算力，但训练一个从提示到系数向量的 LLM 可能需要较大资源；推理时也需要对每个新提示运行模型，延迟较高。
- **应用限制**：P2L 依赖高质量人类偏好数据，在数据稀疏或分布外提示上可能失效。此外，输出的系数向量需要为每个模型都估计一个参数，当模型数量很大时，输出维度增加可能导致训练困难。
- **对比不足**：未与其他个性化评估方法（如基于 LLM 的打分器、少样本偏好预测等）进行直接对比，无法判断 P2L 的相对优势量化程度。

（完）
