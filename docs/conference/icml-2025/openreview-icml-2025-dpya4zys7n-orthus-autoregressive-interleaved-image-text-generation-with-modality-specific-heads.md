---
title: "Orthus: Autoregressive Interleaved Image-Text Generation with Modality-Specific Heads"
title_zh: Orthus：带有模态特定头的自回归交错图像-文本生成
authors: "Siqi Kou, Jiachun Jin, Zhihong Liu, Chang Liu, Ye Ma, Jian Jia, Quan Chen, Peng Jiang, Zhijie Deng"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=dPyA4ZYs7n"
tags: ["query:pers-gen"]
score: 4.0
evidence: 自回归交错多模态生成方法，可用于个性化多模态内容
tldr: 统一的多模态生成模型通常面临模态间信息损失和建模复杂的问题。Orthus通过模态特定头（语言头预测文本、扩散头生成图像）在自回归框架下同时处理离散文本和连续图像特征，最小化信息损失。实验表明，该方法在交错图像-文本生成任务上表现优异，其架构可直接作为个性化多模态生成的基础模型。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有统一多模态模型在交错图像-文本生成中信息损失大，模态关联建模复杂。
method: Orthus使用语言头和扩散头分别处理文本和图像，在自回归框架下通过连续图像特征保留信息。
result: 在交错生成任务上取得优异性能，验证了连续图像特征和模态特定头的有效性。
conclusion: Orthus为多模态生成提供了高效统一框架，可支撑个性化多模态内容生成。
---

## Abstract
We introduce Orthus, a unified multimodal model that excels in generating interleaved images and text from mixed-modality inputs by simultaneously handling discrete text tokens and continuous image features under the \textbf{AR} modeling principle. The continuous treatment of visual signals minimizes the information loss while the fully AR formulation renders the characterization of the correlation between modalities straightforward. Orthus leverages these advantages through its modality-specific heads---one regular language modeling (LM) head predicts discrete text tokens and one diffusion head generates continuous image features. We devise an efficient strategy for building Orthus---by substituting the Vector Quantization (VQ) operation in the existing unified AR model with a soft alternative, introducing a diffusion head, and tuning the added modules to reconstruct images, we can create an Orthus-base model effortlessly (e.g., within 72 A100 GPU hours). Orthus-base can further embrace post-training to craft lengthy interleaved image-text, reflecting the potential for handling intricate real-world tasks. For visual understanding and generation, Orthus achieves a GenEval score of 0.58 and an MME-P score of 1265.8 using 7B parameters, outperforming competing baselines including Show-o and Chameleon.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有统一多模态模型在交错图像-文本生成任务中，由于对视觉信号采用离散化处理（如向量量化），导致模态间信息损失严重，且难以高效建模文本与图像之间的复杂关联。
- **研究动机**：提出一种既能保留视觉连续信息又能利用自回归（AR）框架简洁建模模态关联的统一多模态生成模型。
- **整体含义**：Orthus通过模态特定头（语言头处理文本、扩散头处理图像）在纯自回归框架下同时生成离散文本和连续图像特征，旨在实现低信息损失、高灵活性的交错生成，并为个性化多模态内容生成提供基础模型。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：在自回归建模原则下，使用连续图像特征替代离散化（如VQ）表示，并设计两种模态专用头——语言建模头（预测离散文本token）和扩散头（生成连续图像特征）。这样既保留了视觉信息的完整性，又保持了自回归框架的简洁性，使模态间依赖关系易于建模。
- **关键技术细节**：
  - 将现有统一AR模型中的向量量化（Vector Quantization, VQ）操作用软替代（soft alternative）替换，避免信息瓶颈。
  - 引入扩散头（diffusion head）用于生成连续图像特征，扩散过程与自回归顺序结合。
  - 仅对新增模块（扩散头及相关参数）进行图像重建微调，即可高效创建Orthus-base模型。
  - 预训练后可通过后训练（post-training）进一步生成更长的交错图像-文本序列。
- **公式或算法流程**（文字说明）：
  - 输入混合模态序列（文本token与图像patch特征）；每个位置根据模态选择对应头预测输出。
  - 若当前位置为文本，则语言头给出下一个token的概率分布；若为图像，则扩散头基于当前上下文生成连续图像特征。
  - 训练时联合优化语言建模损失和扩散损失。
  - 推理时依次进行文本token的自回归预测和图像特征的扩散采样。

## 3. 实验设计：使用了哪些数据集/场景，其benchmark是什么，对比了哪些方法
- **数据集/场景**：文中未明确列出训练数据集名称，但提及在交错图像-文本生成任务上进行评估。使用GenEval和MME-P两个基准测试（benchmark）分别评估图像生成质量和多模态理解能力。
- **对比方法**：主要基线为Show-o和Chameleon。Orthus以7B参数量在GenEval得分为0.58、MME-P得分为1265.8，均超越这两个基线。

## 4. 资源与算力
- 文中明确说明：创建Orthus-base模型可在**72 A100 GPU小时**内完成（即仅微调新增模块）。未提及完整预训练所需算力及具体GPU数量；但此高效训练策略是一大亮点。

## 5. 实验数量与充分性
- **实验数量**：文中仅报告了GenEval和MME-P两个基准的最终分数，且仅与两个基线比较。未展示消融实验、不同配置对比或更多数据集结果。
- **充分性与客观性**：实验数量较少，缺乏系统性（如对软替代头、扩散头、模型规模等因素的消融分析）。对比基线虽为最新方法，但未涵盖更多同类模型（如Emu、CM3leon等）。实验结果信息有限，难以判断方法的泛化能力和控制变量公平性。

## 6. 论文的主要结论与发现
- 使用连续图像特征替代离散化表示可以显著减少信息损失，提升统一多模态生成性能。
- 模态特定头（语言头+扩散头）结合自回归框架的设计简洁有效，能够同时处理文本和图像生成。
- 所提Orthus模型（7B参数）在GenEval和MME-P上达到state-of-the-art水平，验证了连续特征与专用扩散头的优势。
- 该架构可直接作为个性化多模态内容生成（如交错图像-文本）的基础模型。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次在纯自回归框架内结合语言头与扩散头，避免VQ的信息损失，同时保持自回归的简单性质。
- **高效训练**：通过软替代VQ+只调新模块（扩散头）的方式，仅需72 A100 GPU小时即可从现有AR模型衍生出Orthus-base，资源需求极低。
- **模型通用性**：支持交错输入与输出，可直接用于个性化多模态内容生成任务，具备实际应用潜力。

## 8. 不足与局限
- **实验覆盖不足**：仅报告两个基准的单一分数，缺乏更全面的评估（如不同数据集、生成多样性、长序列能力、多轮交互等）。无消融实验，无法单独验证各组件的贡献。
- **偏差风险**：未说明训练数据来源和分布，可能存在数据偏差。对比基线较少，且未进行统计显著性检验。
- **应用限制**：模型基于7B参数，未讨论更大或更小规模的效果；扩散头在自回归中的推理效率问题未提及；长序列生成时累积误差可能较严重。

（完）
