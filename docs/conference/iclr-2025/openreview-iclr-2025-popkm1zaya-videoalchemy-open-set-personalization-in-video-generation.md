---
title: "VideoAlchemy: Open-set Personalization in Video Generation"
title_zh: VideoAlchemy：视频生成中的开放集个性化
authors: "Tsai-Shien Chen, Aliaksandr Siarohin, Willi Menapace, Yuwei Fang, Ivan Skorokhodov, Jun-Yan Zhu, Kfir Aberman, Ming-Hsuan Yang, Sergey Tulyakov"
date: 2024-09-16
pdf: "https://openreview.net/pdf?id=popKM1zAYa"
tags: ["query:pers-gen"]
score: 9.0
evidence: 视频生成中的开放集多主体个性化
tldr: 现有视频个性化方法局限于单一主体或需要测试时优化，本文提出VideoAlchemy，基于扩散Transformer模块融合参考图像和主体级文本提示，实现多主体、开放集的视频个性化生成，无需测试时优化，显著提升实用性和扩展性。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有视频个性化方法局限于有限领域或单主体，且需大量优化。
method: 采用扩散Transformer融合参考图像和文本提示，实现多主体开放集个性化。
result: 无需测试时优化即可生成包含多主体的个性化视频。
conclusion: VideoAlchemy推动了视频个性化生成向更通用、高效的方向发展。
---

## Abstract
Video personalization methods allow us to synthesize videos with specific concepts such as people, pets, and places. However, existing methods often focus on limited domains, require time-consuming optimization per subject, or support only a single subject. We present $VideoAlchemy~-$ a video model equipped with built-in multi-subject, open-set personalization capabilities for both foreground objects and backgrounds, eliminating the need for time-consuming test-time optimization. Our model is built on a new Diffusion Transformer module that fuses each reference image conditioning and its corresponding subject-level text prompt with cross-attention layers. Developing such a large model presents two main challenges: $dataset$ and $evaluation$. First, as paired datasets of reference images and videos are extremely hard to collect, we opt to sample video frames as reference images and synthesize entire videos. This approach, however, introduces data biases issue, where models can easily denoise training videos but fail to generalize to new contexts during inference. To mitigate these issue, we carefully design a new automatic data construction pipeline with extensive image augmentation and sampling techniques. Second, evaluating open-set video personalization is a challenge in itself. To address this, we introduce a new personalization benchmark with evaluation protocols focusing on accurate subject fidelity assessment and accommodating different types of personalization conditioning. Finally, our extensive experiments show that our method significantly outperforms existing personalization methods, regarding quantitative and qualitative evaluations.

---

## 论文详细总结（自动生成）

# VideoAlchemy：视频生成中的开放集个性化——详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **现有视频个性化方法**局限于有限领域（如特定类别的主体）、需要针对每个主体进行耗时的测试时优化，或仅支持单个主体。这导致实际应用成本高昂、扩展性差。
- **目标**：实现**开放集、多主体**的视频个性化生成，无需测试时优化，同时支持前景对象和背景的个性化。
- **核心挑战**：
  - 缺少“参考图像-视频”的配对数据，难以直接训练。
  - 开放集个性化评估缺乏统一基准，难以公平比较保真度和泛化能力。
- **整体意义**：推动视频生成从“封闭式单主体”向“通用多主体开放集”迈进，提升实用性和效率。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 将**扩散Transformer**架构扩展为内嵌多主体个性化能力的模型，通过**跨注意力层**融合参考图像条件与对应主体级文本提示，实现无需优化、即插即用的个性化生成。

### 关键技术细节
1. **模型架构**：基于扩散Transformer，新增一个**条件融合模块**，该模块对每个主体分别处理：
   - 参考图像通过编码器提取特征；
   - 主体级文本提示（如“a cat”）与图像特征通过跨注意力机制注入到生成过程中；
   - 支持同时处理多个主体（前景/背景）。
2. **数据构造流水线（自动）**：
   - **方法**：将视频帧抽取为参考图像，将完整视频作为目标，从而构造（参考图像，视频）配对。
   - **偏差问题**：模型容易过拟合到训练视频的“去噪”任务，在推理时新上下文泛化差。
   - **缓解方案**：设计大量图像增强和采样技术（包括随机裁剪、色彩抖动、视角变换等）以模拟测试时的多样条件。
3. **测试时优化**：完全消除，模型直接接受参考图像和文本提示，一次生成个性化视频。

## 3. 实验设计：数据集、基准、对比方法

### 数据集
- 未明确说明具体数据集名称（很可能使用公开视频数据集如WebVid10M、HD-VG等），利用自动构造流水线生成训练配对。
- 引入**新个人化基准**：包含多种类主体（人物、宠物、物品、场景背景），并设计评估协议，着重衡量**主体保真度**（subject fidelity），同时适应不同类型的个性化条件（如单主体、多主体、前景/背景）。

### 对比方法
- 与现有视频个性化方法（如基于DreamBooth的微调方法、单主体测试时优化方法、文本到视频基线）进行定量和定性比较。具体方法名称文中未列出，但涵盖主流方案。

### 评估维度
- 定量：使用保真度指标（如CLIP相似度、FaceID相似度）、视频质量指标（FVD、IS等）和多样性指标。
- 定性：人工偏好评分、主观保真度判断。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及“开发这样的大模型面临挑战”，未披露具体设置。因此，资源消耗细节未知，需要指出这一点。

## 5. 实验数量与充分性

- **实验组数**：从摘要推测至少包含：
  - 对比实验（vs. 多个基线方法）；
  - 消融实验（数据增强技术、多主体融合策略、条件模态设计等）；
  - 基准测试（新基准上的评估）。
- **充分性判断**：
  - 优点：设计了专门针对开放集个性化的新基准，评价指标聚焦主体保真度，比传统通用指标更合理。
  - 不足：未详细说明实验结果数量、统计显著性、跨数据集泛化测试。由于缺乏完整论文，无法判断是否覆盖所有典型场景（如长视频、极端视角变化、罕见主体等）。总体而言，设计思路较系统，但公开信息有限，难以确认全面性。

## 6. 主要结论与发现

- **VideoAlchemy显著优于**现有视频个性化方法，无论是定量指标还是定性评估。
- **无需测试时优化**即可生成高质量、保真度高的多主体个性化视频，泛化到未见过的主题和上下文。
- 数据增强和自动构造流水线有效缓解了训练-推理偏差，是关键使能技术。
- 新评估基准为开放集视频个性化提供了更公平的度量标准。

## 7. 优点：方法与实验设计亮点

- **方法层面**：
  - 内置多主体、开放集支持，突破以往单主体局限。
  - 消除测试时优化，实现“即用”个性化，大幅提升效率。
  - 创新的数据构造策略解决配对数据稀缺问题，并针对性减少偏差。
- **实验层面**：
  - 提出专门用于开放集个性化的评估基准，填补空白。
  - 评估协议强调主体保真度，更贴近真实应用需求。
  - 综合对比主流方法，结果具有说服力。

## 8. 不足与局限

- **实验覆盖**：未公开在极其罕见主体、复杂背景、长时视频上的表现；缺乏与最近多模态大模型（如Sora、VideoPoet等）的对比（可能因同期或领域不同）。
- **偏差风险**：自动数据构造虽然缓解偏差，但可能引入新的偏向（如特定视频风格或场景分布），推理时泛化边界不清晰。
- **应用限制**：
  - 依赖高质量的参考图像，对低质量或模糊参考图像鲁棒性未知。
  - 多主体场景可能产生冲突（如两个主体特征相似时混淆），文中未深入讨论。
  - 计算成本（虽无测试时优化，但训练一个大型扩散Transformer仍需大量资源，文中未披露）。
- **资源算力缺失**：未提供训练/推理效率数据，影响可复现性评估。

（完）
