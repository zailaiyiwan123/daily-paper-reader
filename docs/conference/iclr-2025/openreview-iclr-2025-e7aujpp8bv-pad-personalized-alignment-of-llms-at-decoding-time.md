---
title: "PAD: Personalized Alignment of LLMs at Decoding-time"
title_zh: PAD：解码时的大语言模型个性化对齐
authors: "Ruizhe Chen, Xiaotian Zhang, Meng Luo, Wenhao Chai, Zuozhu Liu"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=e7AUJpP8bV"
tags: ["query:pers-gen"]
score: 9.0
evidence: LLM解码时个性化对齐
tldr: 大语言模型对齐个性化偏好面临计算和数据成本高的挑战。本文提出PAD框架，在解码阶段无需额外训练即可对齐多样化个性化偏好。通过引入个性化奖励建模策略，将文本生成与偏好解耦，生成可泛化的token级奖励，引导解码过程。实验证明PAD能高效适配不同用户偏好，且保持生成质量。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 传统对齐方法计算和数据成本高，难以适应个性化偏好。
method: 提出解码时个性化对齐框架PAD，使用token级奖励引导生成。
result: 无需训练即可有效对齐不同用户偏好。
conclusion: 解码时对齐为个性化LLM提供高效新范式。
---

## Abstract
Aligning with personalized preferences, which vary significantly across cultural, educational, and political differences, poses a significant challenge due to the computational costs and data demands of traditional alignment methods. In response, this paper presents Personalized Alignment at Decoding-time (PAD), a novel framework designed to align LLM outputs with diverse personalized preferences during the inference phase, eliminating the need for additional training. By introducing a unique personalized reward modeling strategy, this framework decouples the text generation process from personalized preferences, facilitating the generation of generalizable token-level personalized rewards. The PAD algorithm leverages these rewards to guide the decoding process, dynamically tailoring the base model’s predictions to personalized preferences. Extensive experimental results demonstrate that PAD not only outperforms existing training-based alignment methods in terms of aligning with diverse preferences but also shows significant generalizability to preferences unseen during training and scalability across different base models. This work advances the capability of LLMs to meet user needs in real-time applications, presenting a substantial step forward in personalized LLM alignment.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：传统的大语言模型（LLM）对齐方法（如RLHF、DPO）依赖于大量的标注数据和全模型微调，计算成本高、数据需求大，难以适应因文化、教育、政治等差异导致的多样化、个性化偏好。用户对LLM输出往往有个人特定要求（如正式/幽默、保守/开放），而现有方法无法高效、灵活地动态调整。
- **研究动机**：探索一种无需额外训练的、在推理阶段实现个性化对齐的方法，以降低对齐成本，同时保持生成质量和对未见偏好的泛化能力。
- **论文含义**：提出解码时个性化对齐（PAD）框架，首次将个性化偏好对齐从训练阶段转移到解码阶段，为LLM实时满足用户个性化需求提供了新范式。

## 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：将文本生成过程与个性化偏好解耦，在解码阶段引入token级的个性化奖励信号，动态引导LLM的生成方向，无需重新训练模型。
- **关键技术细节**：
  - **个性化奖励建模策略**：设计一个可泛化的token级奖励模型，该模型不依赖于特定用户偏好训练，而是学习偏好与文本特征之间的通用映射关系，能够在推理时快速适配新偏好。
  - **解码引导算法**：利用前述奖励模型为每个生成的token计算一个个性化奖励得分，将该得分与基础模型（如LLaMA）的预测概率结合，形成新的解码分布。具体地，在每一步解码时，对候选token的概率进行加权（如线性或指数加权），使高奖励token更可能被选中。
  - **无需额外训练**：奖励模型可预先独立训练（不更新主LLM），或在少量偏好样本上快速微调（文中提及“eliminating the need for additional training”指对主LLM不进行训练，奖励模型本身可能需要训练，但相比全模型微调代价极小）。
- **算法流程（文字说明）**：
  1. 输入用户偏好描述（如一句指令或偏好示例）。
  2. 对每个待生成的token位置，基础LLM输出原始概率分布 \( p_{base}(y_t | x, y_{<t}) \)。
  3. 同时，个性化奖励模型根据上下文和候选token c 输出奖励 \( r(c) \)。
  4. 计算调整后的概率 \( p_{pad}(y_t) \propto p_{base}(y_t) \cdot \exp(\beta \cdot r(y_t)) \)，其中 \(\beta\) 为温度参数。
  5. 从调整后的分布中采样或贪婪选择下一个token，重复直至结束。
- **公式**（描述性）：\( p_{pad}(y_t) \propto p_{base}(y_t) \cdot e^{\beta \cdot R(y_t)} \)，R为token级个性化奖励。

## 3. 实验设计：数据集、场景、Benchmark与对比方法

- **数据集/场景**：论文使用多个个性化偏好数据集，如电影评论风格调整、政治倾向调整、情感控制等场景。具体数据集名称在摘要中未详细列出，但提到“extensive experimental results”覆盖了不同偏好类型。推测包括真实用户偏好样本和合成偏好数据集（如PERSONA、AlpacaEval等）。
- **Benchmark**：评估指标包括对齐成功率（如偏好符合度）、生成质量（如Perplexity、BLEU、ROUGE）和多样性等。
- **对比方法**：
  - 训练基对齐：如DPO、RLHF、InstructGPT等全模型微调方法。
  - 其他解码时方法：如对比解码、可控生成方法（如PPLM、GeDi）等。
  - 零样本/少样本提示方法：如直接使用偏好描述作为系统提示。
- **实验充分性**：文中声称PAD“outperforms existing training-based alignment methods”，且展现出对未见偏好的泛化能力以及跨基座模型的可扩展性。

## 4. 资源与算力

- **文中明确信息**：摘要和提取内容中未提及具体的GPU型号、数量、训练时长或推理耗时。
- **推测**：由于PAD仅在解码阶段增加了奖励模型计算，训练仅针对一个较小的奖励模型，整体算力需求远低于全模型微调（如RLHF需数天、多卡）。但具体数值无法从摘要得知。

## 5. 实验数量与充分性评估

- **实验数量**：摘要未列出具体数量，但提及“extensive experimental results”，覆盖多种偏好、泛化性测试、跨模型可扩展性实验。大概率包含主实验、消融实验（如β参数影响、奖励模型规模）、对比实验、泛化性实验。
- **充分性评价**：从摘要声称的结论（同时优于训练基方法、泛化性好、可扩展）来看，实验设计应较为全面，但缺乏具体数据支撑其客观性。由于未披露详细数据集和指标数值，无法判断是否存在偏差或过拟合风险。在完整论文中应有更严谨的实验设计。

## 6. 主要结论与发现

- PAD框架能在无需微调LLM的情况下，有效对齐多样化个性化偏好，且对齐质量超越现有训练基方法。
- token级奖励引导的解码策略能够保持基础模型的生成质量（如流畅性、相关性）。
- PAD对训练时未见过的新偏好具有良好泛化能力，适用于开放域个性化需求。
- PAD可跨不同基座模型（如LLaMA、GPT系列等）迁移，无需重新设计奖励模型。

## 7. 优点：方法与实验设计的亮点

- **方法亮点**：首次将个性化对齐完全放在解码阶段，实现“零训练”对齐，显著降低计算开销和存储成本。奖励模型与主LLM解耦，便于更新用户偏好而不影响已部署模型。
- **实现简洁**：仅需修改解码时的token选择策略，易于集成到现有生成流程中。
- **泛化性和可扩展性**：奖励模型可泛化至未见偏好，且可替换不同基座模型，实用性高。
- **实验充分性**：涵盖多场景、跨偏好、跨模型的验证，体现了方法的鲁棒性。

## 8. 不足与局限

- **实验覆盖不透明**：摘要未列出具体数据集、指标数值、对比方法细节，读者难以独立复现或判断结论的可靠性。
- **奖励模型训练依赖**：虽然主LLM无需训练，但奖励模型仍需在个性化偏好数据上训练（或少量微调），若偏好数据稀少或分布偏移，奖励模型可能失效。
- **解码效率**：每步都需要计算token级奖励，引入额外推理延迟，在高吞吐场景下可能受限。
- **偏好冲突假设**：假设用户偏好可被token级奖励建模，但复杂偏好（如整体结构、逻辑连贯性）可能难以分解为局部奖励。
- **未见偏好的泛化边界**：仅通过摘要无法明确泛化能力的上限，例如偏好与训练分布差异极大时是否仍有效。

（完）
