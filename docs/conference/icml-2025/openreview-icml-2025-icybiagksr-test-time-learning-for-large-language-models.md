---
title: Test-Time Learning for Large Language Models
title_zh: 大型语言模型的测试时学习
authors: "Jinwu Hu, Zitian Zhang, Guohao Chen, Xutao Wen, Chao Shuai, Wei Luo, Bin Xiao, Yuanqing Li, Mingkui Tan"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=iCYbIaGKSR"
tags: ["query:pers-gen"]
score: 8.0
evidence: 测试时自适应，直接面向模型的个性化适配
tldr: 该论文提出测试时学习（TTL）范式，通过在测试阶段最小化未标记数据的输入困惑度，动态适应目标领域。理论分析和实验证明该方法能有效提升大语言模型在分布偏移下的性能。该范式的核心思想是无需标注数据的自适应，为个性化模型在部署后的持续适配提供了全新思路，可广泛应用于个性化文本生成等场景。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 大语言模型在专业领域和语言变化上存在分布偏移，泛化能力受限。
method: 提出测试时学习范式，通过最小化未标记测试数据的输入困惑度实现动态自适应。
result: 在多个领域迁移任务上显著提升模型性能，优于现有微调方法。
conclusion: 为个性化模型的无监督域自适应提供了高效范式。
---

## Abstract
While Large Language Models (LLMs) have exhibited remarkable emergent capabilities through extensive pre-training,  they still face critical limitations in generalizing to specialized domains and handling diverse linguistic variations, known as distribution shifts. In this paper, we propose a Test-Time Learning (TTL) paradigm for LLMs, namely TLM, which dynamically adapts LLMs to target domains using only unlabeled test data during testing. Specifically, we first provide empirical evidence and theoretical insights to reveal that more accurate predictions from LLMs can be achieved by minimizing the input perplexity of the unlabeled test data. Based on this insight, we formulate the Test-Time Learning process of LLMs as input perplexity minimization, enabling self-supervised enhancement of LLM performance. Furthermore, we observe that high-perplexity samples tend to be more informative for model optimization. Accordingly, we introduce a Sample Efficient Learning Strategy that actively selects and emphasizes these high-perplexity samples for test-time updates. Lastly, to mitigate catastrophic forgetting and ensure adaptation stability, we adopt Low-Rank Adaptation (LoRA) instead of full-parameter optimization, which allows lightweight model updates while preserving more original knowledge from the model. We introduce the AdaptEval benchmark for TTL and demonstrate through experiments that TLM improves performance by at least 20% compared to original LLMs on domain knowledge adaptation.

---

## 论文详细总结（自动生成）

# 论文总结：Test-Time Learning for Large Language Models（大型语言模型的测试时学习）

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：大型语言模型（LLMs）虽然在大规模预训练后展现出强大的能力，但在面对专业领域和多样化的语言变体（即分布偏移）时，泛化能力仍然受限。传统微调需要大量标注数据，且无法在部署后持续自适应。
- **整体含义**：本文提出一种**测试时学习（Test-Time Learning, TTL）范式**，旨在仅利用测试阶段的未标注数据动态调整模型，使LLMs能够无监督地适应目标领域，从而解决分布偏移带来的性能下降问题。这为个性化模型等无需标注的自适应场景提供了全新思路。

## 2. 方法论
### 核心思想
- **关键洞察**：通过最小化未标注测试数据的输入困惑度（input perplexity），可以使LLMs做出更准确的预测。基于此，将测试时学习形式化为**输入困惑度最小化**问题，实现自监督的性能提升。

### 关键技术细节
- **Self-Supervised目标**：在测试阶段，对未标注数据计算输入困惑度，并以此作为损失函数进行模型更新，迫使模型更好地适应测试数据的分布。
- **高困惑度样本筛选策略**：观察到高困惑度的样本对模型优化更具信息量，因此提出**样本高效学习策略（Sample Efficient Learning Strategy）**，在测试时更新中主动选择并强调这些样本，提升自适应效率。
- **轻量级更新方法**：为了避免灾难性遗忘并保持稳定性，采用**低秩适应（LoRA）** 替代全参数优化，仅更新少量低秩矩阵，保留模型原有知识的同时实现高效自适应。

### 算法流程（文字描述）
1. **测试阶段**：获取一批未标注的目标域测试数据。
2. **计算困惑度**：对每个样本计算输入困惑度，识别高困惑度样本。
3. **自监督更新**：对高困惑度样本（或全部样本）的输入困惑度进行最小化，通过LoRA模块进行梯度更新。
4. **推理**：更新后的模型对测试样本进行预测。

## 3. 实验设计
- **数据集/场景**：针对**领域知识自适应**任务，作者提出了新的基准 **AdaptEval**，但摘要中未具体列出使用了哪些数据集（如医疗、法律、科学等）。
- **对比方法**：与**原始LLMs**（无自适应）对比，可能还包括其他微调方法（如全参数微调、标准LoRA微调等），但摘要仅提及相比原始模型性能提升至少20%。
- **Benchmark**：AdaptEval专门用于TTL场景，但具体测试集构成和评价指标未详述。

## 4. 资源与算力
- 文中**未明确说明**使用的GPU型号、数量或训练时长。仅在方法中提到LoRA轻量更新，推测所需算力较低，但无具体数字。

## 5. 实验数量与充分性
- 根据摘要，仅报告了**在AdaptEval上整体性能提升至少20%**，未见消融实验、不同域迁移任务分解、各组件贡献分析等细节。由于提供的文本有限，无法判断实验的充分性和公平性。可能论文正文包含更多实验，但在此摘要中信息不足。
- **小结**：实验数量与充分性无法从摘要中全面评估，存在一定信息缺失。

## 6. 主要结论与发现
- 通过最小化未标注测试数据的输入困惑度，可以在测试阶段提升LLMs对分布偏移的适应性。
- 高困惑度样本对模型更新更有效，采用LoRA可避免灾难性遗忘。
- 提出的TLM方法在AdaptEval上相比原始LLMs性能提升至少20%，表明测试时自监督学习是一种有效且高效的域自适应范式。

## 7. 优点
- **概念新颖**：将测试时学习引入LLMs自适应，直接针对部署后分布偏移，与个性化需求高度契合。
- **无需标注数据**：仅需未标注测试数据，大大降低了实际应用成本。
- **轻量高效**：采用LoRA更新，计算开销小，易于集成。
- **理论支持**：论文提供了经验证据和理论见解，支持困惑度最小化的有效性。

## 8. 不足与局限
- **缺少实验细节**：未提供具体数据集、对比方法、消融实验、超参数设置等，难以完整评估方法泛化能力。
- **潜在偏差风险**：高困惑度样本选择策略可能引入采样偏差，若测试数据本身噪声大，可能影响自适应质量。
- **应用限制**：仅适用于测试阶段能批量获取未标注数据的场景（如在线流式数据），对于单次推理任务或实时性要求高的场景可能不适用。
- **未讨论与其他TTL基线对比**：未提及现有TTL方法（如TENT、SHOT等）的对比，无法判断最优性。

（完）
