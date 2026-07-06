---
title: "TRACE Back from the Future: A Probabilistic Reasoning Approach to Controllable Language Generation"
title_zh: TRACE：从未来回溯——可控语言生成的概率推理方法
authors: "Gwen Yidou Weng, Benjie Wang, Guy Van den Broeck"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=LhkSfpfRXW"
tags: ["query:pers-gen"]
score: 9.0
evidence: 可控生成框架明确以个性化作为目标属性
tldr: 大语言模型生成时难以控制全局属性如个性化，现有方法成本高或不可靠。TRACE通过可计算概率推理高效估计未来序列的期望属性概率，并自适应调整生成过程。该方法无需为每个属性重新训练模型，在毒性控制和个性化主题生成等任务上表现优异，为可控生成提供了灵活高效的解决方案。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 自回归模型难以在生成时控制全局属性，现有方法需要重训练或采样慢。
method: TRACE利用可计算概率推理高效计算期望属性概率，并在生成过程中进行控制。
result: 在毒性减少和个性化主题生成任务上，无需重训练即可有效控制输出属性。
conclusion: TRACE为语言模型的可控生成提供了灵活高效的概率推理框架，支持个性化等属性。
---

## Abstract
As large language models (LMs) advance, there is an increasing need to control their outputs to align with human values (e.g., detoxification) or desired attributes (e.g., personalization, topic). However, autoregressive models focus on next-token predictions and struggle with global properties that require looking ahead. Existing solutions either post-train LMs for each new attribute—expensive and inflexible—or approximate the Expected Attribute Probability (EAP) of future sequences by sampling or training, which is slow and unreliable for rare attributes. We introduce **TRACE** (Tractable Probabilistic Reasoning for Adaptable Controllable gEneration), a novel framework that efficiently computes EAP and adapts to new attributes through tractable *probabilistic* reasoning and lightweight *control*. TRACE distills a Hidden Markov Model (HMM) from an LM and pairs it with a small classifier to estimate attribute probabilities, enabling exact EAP computation over the HMM’s predicted futures. This EAP is then used to reweigh the LM’s next-token probabilities for globally compliant continuations. Empirically, TRACE achieves state-of-the-art detoxification results with only 20% decoding overhead, yields 76 low-resource personalized LMs within seconds, and seamlessly extends to composite attributes.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大型语言模型（LLM）在自回归生成过程中，专注于预测下一个token，难以控制全局属性（如毒性、个性化主题等）。现有方法要么为每个新属性重新训练模型（成本高、不灵活），要么通过采样或训练来近似未来序列的“期望属性概率”（EAP），但速度慢且对稀有属性不可靠。
- **动机**：需要一种**灵活、高效、无需重训练**的可控生成方法，能够动态调整生成过程以符合人类价值观或特定属性（例如个性化、去毒化）。

## 2. 方法论：TRACE框架
- **核心思想**：利用**可计算概率推理**高效估计未来序列的期望属性概率（EAP），并据此自适应调整当前token的概率分布，实现全局可控生成。
- **关键技术细节**：
  1. 从原语言模型（LM）中**蒸馏出一个隐马尔可夫模型（HMM）**，HMM能够预测未来token的概率分布。
  2. 搭配一个**轻量级分类器**，用于估计每个未来序列与目标属性（如毒性、主题）的匹配概率。
  3. 在HMM上通过**精确推理**计算期望属性概率（EAP），无需采样或训练。
  4. 将EAP作为权重，对原LM的next-token概率进行**重新加权**，从而引导生成朝向全局属性合规的方向。
  5. 支持**复合属性**（如同时满足多个属性）的扩展。
- **算法流程**（文字说明）：
  - 输入当前已生成前缀，利用蒸馏HMM展开多种未来可能序列。
  - 利用分类器计算每个未来序列的属性概率，并对所有可能序列进行加权平均得到EAP。
  - 根据EAP调整当前token的logits（例如乘以属性概率比值），采样下一个token。
  - 重复直到生成结束。

## 3. 实验设计
- **数据集/场景**：
  - **毒性控制**（detoxification）任务。
  - **个性化主题生成**任务（低资源个性化语言模型，76个不同属性）。
  - 复合属性生成任务。
- **Benchmark**：未明确列出标准基准，但对比了现有方法（如重训练模型、采样近似方法等）。
- **对比方法**：包括传统重训练方法、基于采样的EAP近似方法等（具体名称未给出，但强调TRACE在去毒化任务上实现了SOTA）。

## 4. 资源与算力
- 文中**未明确说明**所使用GPU型号、数量、训练时长等具体算力信息。
- 但提到：蒸馏HMM和轻量分类器**计算开销小**，去毒化任务仅增加20%解码开销；个性化模型可在**数秒内**获得76个低资源个性化LM。这表明资源需求较低，但具体硬件细节缺失。

## 5. 实验数量与充分性
- 实验覆盖**至少三个场景**：毒性控制、个性化主题生成、复合属性生成。
- 消融实验：可能包含对EAP计算精确性、HMM蒸馏效果等的消融（但摘要未详述）。
- **充分性判断**：实验场景多样，且包含了低资源个性化（76个模型）的大规模验证，结果展示了SOTA性能。但缺乏在更多标准可控生成基准（如情感控制、话题控制等）上的对比，也未涉及与最新强化学习方法的比较。总体较充分但可进一步扩展。

## 6. 主要结论与发现
- TRACE框架在**毒性减少**任务上达到**SOTA**，且**解码开销仅增加20%**。
- 可在**数秒内**生成76个低资源个性化语言模型，无需为每个属性重新训练。
- 能够**无缝扩展**到复合属性，灵活性高。
- 验证了**可计算概率推理**在可控生成中的有效性，避免了采样不准确或训练成本高的问题。

## 7. 优点（亮点）
- **无需重训练**：只需要一个轻量分类器和一次HMM蒸馏，即可适配新属性。
- **高效**：精确推理代替采样，计算开销远低于采样/训练方法。
- **支持个性化等全局属性**：直接建模未来序列概率，符合“从未来回溯”的直觉。
- **可扩展性**：复合属性只需组合分类器，易于实现。
- **理论优雅**：基于概率推理，避免了启发式调参。

## 8. 不足与局限
- **实验覆盖有限**：未在更多标准可控生成任务（如情感、风格、长度控制）上验证，个性化主题生成的具体领域未说明。
- **HMM蒸馏的精度**：HMM对LM的近似可能损失表达能力，影响复杂长程依赖的属性控制效果。
- **分类器依赖**：需要针对每个属性训练一个分类器（轻量但仍需标注数据），对于罕见属性可能仍然有数据需求。
- **资源算力信息缺失**：未提供训练/推理的具体硬件配置，不利于复现公平比较。
- **潜在偏差风险**：分类器和HMM的偏差可能被放大，导致生成不确定性。

（完）
