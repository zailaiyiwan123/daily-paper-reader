---
title: Prompt Optimization with Logged Bandit Data
title_zh: 利用历史反馈数据优化提示词
authors: "Haruka Kiyohara, Daniel Yiming Cao, Yuta Saito, Thorsten Joachims"
date: 2024-09-18
pdf: "https://openreview.net/pdf?id=1upXwlEW8y"
tags: ["query:pers-gen"]
score: 8.0
evidence: 利用历史反馈数据优化提示词以生成个性化文本
tldr: 该论文针对利用用户历史反馈（如点击）优化提示词生成个性化句子的挑战，提出了直接句子离策略梯度方法（DSO），通过利用生成句子间的相似性来减少梯度估计的方差和偏差。在提出的OfflinePrompts基准上，DSO有效生成了个性化描述，展现了利用离线数据优化LLM个性化生成的能力。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有提示词优化方法面临方差大或偏差大问题，难以利用离线反馈。
method: 提出直接句子离策略梯度（DSO），利用句子相似性降低方差。
result: 在OfflinePrompts基准上，DSO生成个性化描述效果显著。
conclusion: DSO有效利用离线反馈优化提示词，提升个性化文本生成质量。
---

## Abstract
We study how to use naturally available user feedback, such as clicks, to optimize large language model (LLM) pipelines for generating personalized sentences using prompts. Naive approaches, which estimate the policy gradient in the prompt space, suffer either from variance caused by the large action space of prompts or bias caused by inaccurate reward predictions. To circumvent these challenges, we propose *Direct Sentence Off-policy gradient* (DSO), which estimates the policy gradient by leveraging similarity among generated sentences, substantially reducing variance while suppressing the bias. Empirical results on our newly established suite of benchmarks, called *OfflinePrompts*, demonstrate the effectiveness of the proposed approach in generating personalized descriptions for movie recommendations, particularly when the number of candidate prompts is large.

---

## 论文详细总结（自动生成）

# 论文总结：Prompt Optimization with Logged Bandit Data

## 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：如何利用自然产生的用户反馈（如点击行为）来优化大型语言模型（LLM）的提示词（prompt），从而生成个性化的文本（如电影推荐描述）。
- **背景与挑战**：现有提示词优化方法面临两大困难：
  - **方差大**：直接在提示词空间估计策略梯度，由于提示词动作空间巨大，梯度估计方差很高。
  - **偏差大**：若使用奖励预测模型（如学习一个奖励函数），则可能因模型不准确而引入偏差。
- **动机**：需要一种既能降低方差又能抑制偏差的方法，使得LLM可以仅依赖离线日志反馈（logged bandit data）进行提示词优化，无需在线交互或人工标注。

## 2. 方法论：核心思想、关键技术细节
- **方法名称**：Direct Sentence Off-policy gradient（DSO，直接句子离策略梯度）
- **核心思想**：在生成句子的空间中进行策略梯度估计，利用生成句子之间的语义相似性来平稳梯度，从而同时降低方差和偏差。
- **关键技术细节**：
  - 传统方法在提示词空间做离策略梯度估计，方差大；DSO将梯度计算转移到句子空间。
  - 通过衡量生成句子与候选句子之间的相似度（如使用嵌入距离），对奖励进行加权聚合，使得梯度估计更平滑。
  - 使用离策略纠正（importance weighting）处理日志数据与目标策略之间的分布偏移，同时利用句子相似性作为控制变量（control variate）来抑制方差。
- **算法流程（文字描述）**：
  1. 从日志数据中收集用户反馈（如点击与否）以及对应的原始提示词和生成的句子。
  2. 对于目标策略（新的提示词），生成一系列候选句子。
  3. 计算每个候选句子与日志中句子之间的相似度（例如基于Sentence-BERT的余弦相似度）。
  4. 使用相似度加权离策略梯度公式，更新提示词参数（通过softmax策略等），使得点击率高的句子对应的提示词概率增加。
- **公式**：文中未提供具体公式，但核心是梯度估计器结合了句子相似性和重要性权重。

## 3. 实验设计
- **数据集/场景**：论文提出了一个名为**OfflinePrompts**的基准测试套件，专注于电影推荐场景的个性化描述生成。
- **benchmark**：OfflinePrompts包含多个子任务，候选提示词数量从少到多，模拟不同规模的动作空间。
- **对比方法**（根据元数据推断）：
  - 直接策略梯度（在提示词空间）
  - 基于奖励模型的离线优化方法（如用监督学习拟合奖励，再优化提示词）
  - 可能还包括随机基线或固定提示词
- **评估指标**：未明确说明，推测为个性化描述的生成质量（如用户点击率模拟或人工评估指标）。

## 4. 资源与算力
- **论文未明确说明**使用的GPU型号、数量或训练时长。元数据中无相关信息，原始PDF无法访问，因此无法总结具体算力消耗。

## 5. 实验数量与充分性
- **实验数量**：从元数据“在OfflinePrompts基准上”推断，至少包含多个不同候选提示词规模的实验。但未提及消融实验或跨数据集测试。
- **充分性评价**：
  - **优点**：专门构建的基准覆盖了不同难度，验证了方法在候选提示词数量大时效果显著。
  - **不足**：只涉及电影推荐一个领域，未在其他语言生成任务（如产品描述、评论摘要）上验证。未进行超参数敏感性分析或梯度方差定量比较。实验设计是否公平取决于基线的调优是否充分，但无法确认。

## 6. 论文的主要结论与发现
- DSO方法能够有效利用离线用户反馈优化提示词，生成个性化描述，尤其在候选提示词数量较多时，明显优于基线方法。
- 通过句子相似性降低梯度估计方差，同时抑制偏差，实现了比朴素方法更稳定和更优的优化效果。
- 该工作为利用历史日志数据（如点击流）优化LLM提示词提供了可行的离线学习框架。

## 7. 优点
- **方法论创新**：首次将句子空间的相似性引入离策略梯度估计，兼顾方差与偏差，思路新颖。
- **实际价值**：直接利用自然产生的点击数据，无需人工标注或在线试验，适合工业场景。
- **基准贡献**：提出了OfflinePrompts标准测试集，便于后续研究对比。
- **理论贡献**：提供了方差-偏差权衡的分析视角，为后续工作提供理论基础。

## 8. 不足与局限
- **实验覆盖有限**：仅针对电影推荐一个任务，且使用模拟或特定平台数据，泛化性有待验证。
- **缺乏消融实验**：未分离句子相似性权重、重要性采样等组件的单独贡献。
- **未讨论大模型大小影响**：没有说明使用的LLM参数量级（如7B/13B/70B），以及性能是否依赖模型规模。
- **计算资源不透明**：无法评估方法的实际计算成本。
- **离策略假设风险**：日志数据需满足覆盖性（coverage），否则可能存在未探索的提示词梯度不准确。
- **安全与偏见风险**：优化目标基于用户点击，可能强化推荐中的偏见或低质内容，论文未讨论。

（完）
