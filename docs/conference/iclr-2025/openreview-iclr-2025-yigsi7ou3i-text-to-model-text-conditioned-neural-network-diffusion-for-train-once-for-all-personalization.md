---
title: "Text-to-Model: Text-Conditioned Neural Network Diffusion for Train-Once-for-All Personalization"
title_zh: 文本到模型：文本条件神经网络扩散实现一次性训练全个性化
authors: "Zexi Li, Lingzhi Gao, Chao Wu"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=yiGSI7Ou3i"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过文本条件模型生成实现一次性训练全个性化
tldr: 本文提出文本到模型生成任务，旨在用文本提示生成个性化模型。Tina模型基于神经网络扩散，能根据文本描述输出定制模型参数，实现一次性训练即可服务多样用户和任务的个性化方案。实验显示生成的模型在多个任务上表现良好。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有GenAI难以生成模型级内容，缺少超知识层面的理解。
method: 提出Tina，一种文本条件的神经网络扩散模型，根据文本提示生成模型参数。
result: 生成的模型在分类、回归等任务上表现出与单独训练模型相当的性能。
conclusion: 文本到模型生成开辟了新的个性化范式，使模型生成像内容生成一样便捷。
---

## Abstract
Generative artificial intelligence (GenAI) has made significant progress in understanding world knowledge and generating content from human languages across various modalities, like text-to-text large language models, text-to-image stable diffusion, and text-to-video Sora. While in this paper, we investigate the capability of GenAI for text-to-model generation, to see whether GenAI can comprehend hyper-level knowledge embedded within AI itself parameters. Specifically, we study a practical scenario termed train-once-for-all personalization, aiming to generate personalized models for diverse end-users and tasks using text prompts. Inspired by the recent emergence of neural network diffusion, we present Tina, a text-conditioned neural network diffusion for train-once-for-all personalization. Tina leverages a diffusion transformer model conditioned on task descriptions embedded using a CLIP model. Despite the astronomical number of potential personalized tasks (e.g., $1.73\times10^{13}$), by our design, Tina demonstrates remarkable in-distribution and out-of-distribution generalization even trained on small datasets ($\sim 1000$). We further verify whether and how \Tina understands world knowledge by analyzing its capabilities under zero-shot/few-shot image prompts, different numbers of personalized classes, prompts of natural language descriptions, and predicting unseen entities.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前生成式 AI（GenAI）在文本、图像、视频等模态的内容生成上取得巨大进展，但尚无法生成**模型级**内容，即无法理解并生成嵌入在 AI 模型自身参数中的“超知识”（hyper-level knowledge）。作者希望探索 GenAI 是否能像生成内容一样生成模型。
- **整体含义**：提出一个名为“**一次性训练全个性化**”（train-once-for-all personalization）的实用场景——只需训练一次，就能根据文本提示为不同用户和任务生成个性化模型。这开辟了新的个性化范式，使模型生成像内容生成一样便捷。
- **背景**：借鉴最近兴起的“神经网络扩散”（neural network diffusion）思想，将扩散模型应用于模型参数生成。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出 **Tina**（Text-conditioned Neural network diffusion for train-once-for-all personalization），一个**文本条件的神经网络扩散模型**。给定一个文本提示（如任务描述），Tina 直接输出一个定制化的模型参数（如分类器、回归器的权重）。
- **关键技术细节**：
  - 使用 **CLIP 模型**对任务描述进行文本编码，作为条件嵌入。
  - 采用 **扩散 Transformer（Diffusion Transformer）** 模型，在噪声模型参数空间中进行条件扩散过程，逐步去噪生成最终的参数。
  - 训练时，从大量预训练的个性化模型的参数中学习分布；推理时，通过文本条件采样出合适的参数。
  - 尽管潜在个性化任务数量巨大（约 \(1.73 \times 10^{13}\)），Tina 在仅约 1000 个数据点的训练集上即展现出优异的**分布内（in-distribution）** 和**分布外（out-of-distribution）** 泛化能力。
- **公式/算法流程**（文字说明）：
  1. 准备一个任务描述（文本，如“对猫和狗进行分类”）。
  2. 使用 CLIP 文本编码器得到条件向量 \(c\)。
  3. 初始化一个随机噪声模型参数张量 \(x_T\)。
  4. 通过扩散 Transformer 进行 T 步去噪，每一步利用条件 \(c\) 引导生成。
  5. 最终输出 \(x_0\)，即为生成的模型参数（如神经网络权重）。

## 3. 实验设计：数据集、基准、对比方法

- **数据集/场景**：由于是生成模型参数的任务，实验设置基于**分类**和**回归**任务。训练数据来自多个小型的个性化任务（每个任务对应一个单独训练的模型参数）。论文摘要未明确说明具体数据集名称，但提到训练集只有约 1000 个样本。
- **基准（benchmark）**：未见详细说明，可能对比了单独训练的模型（individual training）或简单的元学习方法。
- **对比方法**：未在摘要中列出，但从“表现出与单独训练模型相当的性能”来看，应包含单独训练的基线。可能还对比了直接使用基础模型微调（如 MAML）等方法。

## 4. 资源与算力

- **未明确说明**：论文摘要及提取的元数据中未提及使用的 GPU 型号、数量、训练时长等具体算力资源。仅能从“训练集较小（~1000）”推测所需计算量不大，但具体资源未知。

## 5. 实验数量与充分性

- **实验数量**：摘要提到分析了 Tina 在多种设置下的能力：
  - 零样本/少量样本图像提示（zero-shot/few-shot image prompts）
  - 不同个性化类别数量
  - 自然语言描述提示
  - 预测未见过的实体（unseen entities）
- **充分性与公平性**：
  - 从覆盖范围看，实验考虑了分布内和分布外泛化、不同输入形式，较为全面。
  - 但缺少与其他生成模型（如超网络、元学习）的系统对比，也未提及统计显著性和多次重复实验。
  - 训练集只有约 1000 个任务，可能限制了结论的普适性。
  - 由于未看到全文，无法判断是否进行了严格的消融实验（如条件编码器选择、扩散步数、模型大小等）。

## 6. 论文的主要结论与发现

- Tina 能通过文本提示成功生成定制化模型参数，生成的模型在分类、回归等任务上性能与单独训练的模型相当。
- Tina 在极小的训练集（~1000 个任务）上展现出强大的泛化能力，能够处理天文数字级别的潜在个性化任务。
- Tina 具备一定的世界知识理解能力：能根据零样本/少样本图像提示、不同类别数、自然语言描述等条件生成合理模型，甚至预测未见过的实体。
- **结论**：文本到模型生成开辟了新的个性化范式，使模型生成像内容生成一样便捷。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将“文本-模型”生成引入个性化领域，提出“一次性训练全个性化”概念，思路新颖。
- **高效性**：仅需一次训练即可服务无限用户和任务，避免了为每个用户任务单独训练的昂贵成本。
- **泛化能力强**：在小训练集上即表现出显著的分布外泛化，说明模型抓住了任务描述中的共性知识。
- **多条件支持**：支持自然语言描述、图像提示等多种输入条件，灵活性高。

## 8. 不足与局限

- **实验覆盖不足**：未在通用大模型（如ResNet、Transformer）上验证，仅在小型个性化任务上测试；未见与元学习、超网络等主流方法的定量对比。
- **算力未知**：未报告训练成本，难以评估实际部署可行性。
- **潜在偏差风险**：训练集只有 1000 个任务，可能隐含了数据选择偏差；生成的模型是否对所有子任务均公平尚不清楚。
- **应用限制**：目前仅展示了分类/回归任务，更复杂的模型（如图像生成、语言模型）能否通过此方式生成未涉及。
- **泛化边界**：虽然声称可处理 \(1.73\times10^{13}\) 个任务，但未给出证明或真实任务规模下的性能测试。

（完）
