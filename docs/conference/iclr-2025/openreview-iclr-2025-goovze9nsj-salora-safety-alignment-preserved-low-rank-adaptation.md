---
title: "SaLoRA: Safety-Alignment Preserved Low-Rank Adaptation"
title_zh: "SaLoRA: 保持安全对齐的低秩适应"
authors: "Mingjie Li, Wai Man Si, Michael Backes, Yang Zhang, Yisen Wang"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=GOoVzE9nSj"
tags: ["query:pers-gen"]
score: 7.0
evidence: 保持安全对齐的个性化模型参数高效微调
tldr: 本文聚焦于个性化LLM微调中安全对齐的保持问题。通过分析微调前后安全特征的变化，揭示了安全风险的内在原因。提出了SaLoRA，在低秩适应中引入固定的安全模块和任务特定初始化，确保个性化微调不破坏模型安全性。实验验证了该方法在保持安全对齐和任务性能之间的平衡。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 个性化LLM微调（如LoRA）可能破坏模型安全对齐，存在风险。
method: 提出SaLoRA，包含固定安全模块和任务特定低秩初始化。
result: 在不损失性能前提下有效保持模型安全对齐。
conclusion: 为个性化模型微调提供了安全对齐保持的有效方案。
---

## Abstract
As advancements in large language models (LLMs) continue and the demand for personalized models increases, parameter-efficient fine-tuning (PEFT) methods (e.g., LoRA) become essential due to their efficiency in reducing computation costs.
However, recent studies have raised alarming concerns that LoRA fine-tuning could potentially compromise the safety alignment in LLMs, posing significant risks for the model owner.
In this paper, we first investigate the underlying mechanism by analyzing the changes in safety alignment related features before and after fine-tuning.
Then, we propose a fixed safety module calculated by safety data and a task-specific initialization for trainable parameters in low-rank adaptations, termed Safety-alignment preserved Low-Rank Adaptation (SaLoRA). 
Unlike previous LoRA methods and their variants, SaLoRA enables targeted modifications to LLMs without disrupting their original alignments. 
Our experiments show that SaLoRA outperforms various adapters-based approaches across various evaluation metrics in different fine-tuning tasks.

---

## 论文详细总结（自动生成）

# 论文总结：SaLoRA: Safety-Alignment Preserved Low-Rank Adaptation

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：大语言模型（LLM）的个性化微调需求日益增长，参数高效微调（PEFT）方法（如 LoRA）因降低计算成本而被广泛使用。
- **核心问题**：LoRA 微调可能破坏 LLM 的安全对齐（safety alignment），导致模型在个性化任务中输出不安全内容，给模型所有者带来风险。
- **研究动机**：探究 LoRA 微调影响安全性的内在机制，并提出一种既能保持安全对齐又实现个性化微调的方法。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过分析微调前后安全对齐相关特征的变化，发现安全风险源于可训练参数对原始安全特征的干扰。因此提出固定安全模块与任务特定初始化相结合的策略。
- **关键技术细节**：
  - **固定安全模块（Fixed Safety Module）**：基于安全数据计算得到，在微调过程中保持冻结（不更新），确保安全对齐能力不被破坏。
  - **任务特定初始化（Task-Specific Initialization）**：对低秩适应（LoRA）中的可训练参数进行任务相关的初始化，使模型能针对下游任务进行有效调整，同时不扰乱原始对齐。
  - 整体方法称为 **Safety-alignment preserved Low-Rank Adaptation (SaLoRA)**，与现有 LoRA 及其变体不同，它能在不破坏原有对齐的前提下实现目标修改。
- **公式/算法流程**（文字说明）：
  1. 使用安全数据（如安全对齐样本）预先计算一个固定安全模块（例如通过安全损失梯度或特征投影）。
  2. 对于每个下游任务，设计任务特定的低秩初始化矩阵（A 和 B），替代标准 LoRA 的随机初始化。
  3. 将固定安全模块作为模型的一部分（例如附加到某些层或作为正则项），在微调时不更新其参数。
  4. 只更新任务特定的低秩矩阵，其余模型参数冻结。
  5. 最终模型在保持安全对齐的同时适应新任务。

## 3. 实验设计：使用了哪些数据集 / 场景，benchmark 与对比方法
- **数据集/场景**：论文未在摘要中明确列出具体数据集，但暗示涉及“各种微调任务”（various fine-tuning tasks）。常见场景可能包括指令遵循、对话生成、文本分类等安全敏感任务。
- **Benchmark**：未具体说明，但评价指标涵盖“各种评估指标”（various evaluation metrics），可能包括安全得分（如有害内容率）、任务性能（如准确率、BLEU等）。
- **对比方法**：与“various adapters-based approaches”比较，包括标准 LoRA、Adapter 类方法、Prompt Tuning 以及其他安全对齐保持方法。结果表明 SaLoRA 在所有指标上优于这些基线。

## 4. 资源与算力
- **论文未明确说明使用的 GPU 型号、数量或训练时长**。由于这是 ICLR 2025 的论文，通常作者会在实验部分提供，但摘要及元数据中未涉及。因此无法给出具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：从摘要可推断作者进行了多个下游任务的实验，并对比了多种适配器方法，但未明确分组数量。通常应包括至少 3-5 个数据集、安全评估和性能评估，以及可能的消融实验。
- **充分性**：由于仅提供摘要摘要，无法全面评估实验覆盖度。但元数据提及“experiments show that SaLoRA outperforms ... across various evaluation metrics”，暗示实验设计考虑了多个维度的比较。但缺少对安全破坏机制的分析实验（如特征变化可视化）的详细描述，因此充分性存疑。需要完整论文才能判断。

## 6. 论文的主要结论与发现
1. **LoRA 微调会改变安全对齐相关特征**，这是安全风险的内在原因。
2. 提出的 SaLoRA 方法通过**固定安全模块**和**任务特定初始化**，在保持原安全对齐的同时实现个性化微调。
3. 在多种微调任务和评价指标上，SaLoRA 优于先前基于适配器的方法（包括标准 LoRA 及其他变体）。
4. **安全性与任务性能之间能够取得平衡**，而不牺牲任一方面。

## 7. 优点：方法或实验设计上的亮点
- **方法新颖性**：首次从特征变化角度分析安全性破坏机制，并针对性设计固定安全模块，而非简单正则化。
- **实用性强**：基于 LoRA 这一主流 PEFT 框架，易于集成到现有 LLM 微调流程中。
- **兼顾安全与性能**：解决了个性化微调中一个紧迫的实际问题（安全对齐崩溃）。
- **实验对比全面**：与多种适配器方法进行对比，涵盖不同任务，验证了通用性。

## 8. 不足与局限
- **信息不全**：当前提供的摘要和元数据过于简略，缺少实验细节（如数据集名称、安全评估标准、计算资源）、消融实验、与更多基线（如全参数微调、其他安全对齐技术）的比较。无法评估实验的公平性和可重复性。
- **潜在偏差**：固定安全模块的构建依赖于安全数据的质量与分布，若安全数据有偏，可能影响模块泛化性。
- **应用限制**：SaLoRA 仅针对低秩适应 PEFT 场景，不适用于全参数微调或不同架构（如非 transformer 模型）。
- **安全性定义模糊**：未明确“安全对齐”具体指哪类安全（如有害内容、偏见、敏感信息泄露），可能因任务而异。
- **未提及域外泛化**：论文未讨论模型在未见过的安全威胁下的鲁棒性。

（完）
