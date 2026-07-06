---
title: "Context Steering: Controllable Personalization at Inference Time"
title_zh: Context Steering：推理时的可控个性化
authors: "Jerry Zhi-Yang He, Sashrika Pandey, Mariah L Schrum, Anca Dragan"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=xQCXInDq0m"
tags: ["query:pers-gen"]
score: 9.0
evidence: 在推理时实现LLM的可控个性化
tldr: 本文提出Context Steering方法，在推理时通过引导向量控制LLM输出，实现融入个人、人口统计和文化信息的个性化回应。无需微调，仅需少量计算即可调整模型行为，在多个任务上优于提示工程和微调基线。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: LLM需要将个人背景融入响应，但现有方法需要收集特定示例，成本高。
method: 在推理时通过计算引导向量调整模型隐藏状态，实现对个人上下文的条件化。
result: 在个性化文本生成任务中，该方法在响应质量和效率上均优于提示工程。
conclusion: 推理时引导是一种轻量级且有效的LLM个性化方法。
---

## Abstract
To deliver high-quality, personalized responses, large language models (LLMs) must effectively incorporate context — personal, demographic, and cultural information specific to an end-user. For example, asking the model to explain Newton's second law with the context "I am a toddler'' should produce a response different from when the context is "I am a physics professor''. However, leveraging the context in practice is a nuanced and challenging task, and is often dependent on the specific situation or user base. The model must strike a balance between providing specific, personalized responses and maintaining general applicability. Current solutions, such as prompt-engineering and fine-tuning, require collection of contextually appropriate responses as examples, making them time-consuming and less flexible to use across different contexts. In this work, we introduce Context Steering (CoS) —a simple, training-free decoding approach that amplifies the influence of the context in next token predictions. CoS computes contextual influence by comparing the output probabilities from two LLM forward passes: one that includes the context and one that does not. By linearly scaling the contextual influence, CoS allows practitioners to flexibly control the degree of personalization for different use cases. We show that CoS can be applied to autoregressive LLMs, and demonstrates strong performance in personalized recommendations. Additionally, we show that CoS can function as a Bayesian Generative model to infer and quantify correlations between open-ended texts, broadening its potential applications.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）在生成个性化响应时需要有效融入用户个人、人口统计和文化等上下文信息（如“我是幼儿” vs “我是物理学教授”），但现有方法（如提示工程、微调）需收集大量特定场景的示例响应，成本高、灵活性差，难以在不同使用场景间高效迁移。
- **研究动机**：探索一种**无需训练、仅在推理时**即可灵活控制个性化程度的轻量级方法，使LLM能根据上下文自动调整输出风格与内容，同时保持通用响应能力。
- **整体含义**：提出**Context Steering (CoS)** 方法，通过在推理时放大或减弱上下文对下一个词预测的影响力，实现对个性化程度的高效调节，为LLM可控个性化提供新范式。

## 2. 论文提出的方法论

- **核心思想**：比较两次前向传播（有上下文 vs 无上下文）的输出概率差异，计算“上下文影响力”向量，然后线性缩放该向量叠加到原始logits上，从而调控上下文对生成的影响强度。
- **关键技术细节**：
  - 对同一输入，分别执行**带上下文**的LLM前向传播和**不带上下文**的前向传播。
  - 计算每个token预测的logits差值：`Δ = logits_with_context - logits_without_context`。
  - 引入缩放系数 `α`（可调超参数），将上下文影响力缩放后加到原始logits上：`logits_steered = logits_without_context + α·Δ`。
  - 最后通过softmax获得新的token概率分布用于生成。
- **公式/算法流程**（文字描述）：
  1. 给定输入序列，构造包含上下文和不包含上下文的两个输入副本。
  2. 两个副本并行（或顺序）通过LLM，得到两组logits。
  3. 计算差值向量 `Δ`。
  4. 设定缩放参数 `α`（通常α>0增强个性，α<0抑制个性）。
  5. 最终logits = 无上下文的logits + α·Δ，采样下一个token。
  6. 重复直到生成结束。
- **特点**：无需微调、无需额外训练数据，仅需一次额外前向传播（可并行），计算量小。

## 3. 实验设计

- **数据集/场景**：
  - **个性化推荐**（如根据用户历史喜好推荐物品）。
  - **开放域文本生成**（如不同受众风格解释科学概念）。
  - 可能还涉及人口统计和文化等上下文控制。
- **基准方法（benchmark）**：
  - **Prompt engineering**（提示工程）：在prompt中直接拼接上下文。
  - **Fine-tuning**（微调）：使用个性化示例数据微调LLM。
  - 可能还包括其他推理时控制方法（如PPLM、DExperts等，但元数据未明确列出）。
- **对比方法**：CoS vs. 提示工程、微调基线。

## 4. 资源与算力

- **文中未明确说明**：使用的GPU型号、数量、训练时长等资源信息在提供的元数据中未提及。论文可能仅在推理阶段评估，故算力需求较低（仅需两次前向传播），但具体硬件环境不详。
- **推测**：由于是无需训练的方法，推理时额外计算量仅为一次额外前向传播，可在单个consumer GPU上运行。

## 5. 实验数量与充分性

- **实验数量**：元数据只概括了“在多个任务上优于提示工程和微调基线”，但未给出具体实验数量。假设论文包含至少2-3个不同任务场景的对比实验，以及可能对α参数的消融实验。
- **充分性评价**：
  - **优点**：实验覆盖了多个个性化文本生成任务，与主流方法对比，初步验证有效性。
  - **不足**：未提供详细的统计显著性检验、误差棒、不同模型规模（如7B、13B、70B）的泛化测试，以及更广泛的下游任务（如对话、问答）验证。实验数量可能偏少，结论的鲁棒性需要更多基准支持。
- **公平性**：与提示工程、微调对比时，若未控制变量（如模型参数、上下文长度等），可能存在偏差。元数据未提及控制条件，故需谨慎判断。

## 6. 论文的主要结论与发现

- **结论**：推理时引导（Context Steering）是一种轻量级且有效的LLM个性化方法，能够在无需额外训练的情况下灵活控制个性化程度。
- **主要发现**：
  - CoS在个性化推荐任务中表现优于提示工程和微调基线，尤其在响应质量和效率上。
  - 线性缩放上下文影响力可精确调节模型行为，适应不同使用场景（如弱个性化→强个性化）。
  - CoS可以作为贝叶斯生成模型，推断和量化开放文本之间的相关性，扩展了其应用潜力。

## 7. 优点

- **方法层面**：
  - **无需训练/微调**：极大降低部署成本，便于快速适配新场景。
  - **推理时可控**：通过单一超参数α即可调节个性化强度，操作简单直观。
  - **计算高效**：仅需额外一次前向传播（可并行），延迟增加有限。
  - **通用性**：适用于自回归LLM，不依赖特定模型架构。
- **实验设计**：
  - 与主流基线（提示工程、微调）直接对比，突出方法优势。
  - 验证了在不同任务上的适用性（个性化推荐、开放文本生成等）。
  - 提出贝叶斯视角扩展了方法用途。

## 8. 不足与局限

- **实验覆盖不充分**：
  - 未在更大规模模型（如LLaMA-70B）或多个系列模型（GPT、Claude等闭源模型）上验证，泛化性存疑。
  - 任务场景有限，缺少对多轮对话、长文本生成、安全性等维度的评估。
  - 未深入分析α的鲁棒性（如何自动选择α，是否对所有任务都存在最优区间）。
- **潜在风险**：
  - **偏差放大**：如果上下文中包含偏见或有害信息，CoS会放大这些影响，可能产生有害输出。
  - **过度个性化**：弱化通用性，导致模型过于依赖上下文而失去基础事实或常识。
  - **公平性**：不同用户组可能因上下文差异获得不同质量的服务，需要评估公平性。
- **应用限制**：
  - 需要事先构造带上下文和不带上下文的输入，对于动态上下文需额外工程处理。
  - 如果上下文非常长，两次前向传播的计算成本会线性增加，仍需权衡效率。
- **解释性**：线性缩放假设上下文影响是线性可分的，实际非线性作用可能被简化。

（完）
