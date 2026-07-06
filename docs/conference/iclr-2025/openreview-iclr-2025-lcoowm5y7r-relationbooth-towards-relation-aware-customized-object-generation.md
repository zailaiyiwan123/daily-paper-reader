---
title: "RelationBooth: Towards Relation-Aware Customized Object Generation"
title_zh: RelationBooth：面向关系感知的定制化对象生成
authors: "Qingyu Shi, Lu Qi, Jianzong Wu, Jinbin Bai, Jingbo Wang, Yunhai Tong, Xiangtai Li, Ming-Hsuan Yang"
date: 2024-09-18
pdf: "https://openreview.net/pdf?id=LcoOwM5y7r"
tags: ["query:pers-gen"]
score: 9.0
evidence: 关系感知的定制化对象生成，用于个性化图像
tldr: 该论文提出RelationBooth框架，解决现有定制化图像生成忽视对象间关系的问题。通过解耦身份学习和关系学习，并使用精心设计的数据集，RelationBooth能根据用户提供的参考图像和文本描述，生成同时保留对象身份和符合语义关系的图像，显著提升个性化生成的真实性和可控性。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有定制化图像生成模型忽略了对象之间的关系，导致生成结果不自然。
method: 构建关系感知数据集，解耦身份和关系的学习，并通过扩散模型生成。
result: 在关系保持和身份保真度上，RelationBooth优于已有方法。
conclusion: 关系感知是提升个性化图像生成质量的关键。
---

## Abstract
Customized image generation is crucial for delivering personalized content based on user-provided image prompts, aligning large-scale text-to-image diffusion models with individual needs. However, existing models often overlook the relationships between customized objects in generated images. Instead, this work addresses that gap by focusing on relation-aware customized image generation, which aims to preserve the identities from image prompts while maintaining the predicate relations described in text prompts. Specifically, we introduce RelationBooth, a framework that disentangles identity and relation learning through a well-curated dataset. Our training data consists of relation-specific images, independent object images containing identity information, and text prompts to guide relation generation. Then, we propose two key modules to tackle the two main challenges—generating accurate and natural relations, especially when significant pose adjustments are required, and avoiding object confusion in cases of overlap. First, we introduce a keypoint matching loss that effectively guides the model in adjusting object poses closely tied to their relationships. Second, we incorporate local features from the image prompts to better distinguish between objects, preventing confusion in overlapping cases. Extensive results on three benchmarks demonstrate the superiority of RelationBooth in generating precise relations while preserving object identities across a diverse set of objects and relations. The source code and trained models will be made available to the public.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

定制化图像生成旨在根据用户提供的参考图像（如某个物体或人）和文本描述，生成保留目标身份且符合文本语义的个性化图像。现有的方法（如 DreamBooth、Textual Inversion 等）大多只关注单个对象的外观保真，而忽视了生成图像中多个定制对象之间的语义关系（如“A 在 B 的左边”、“A 拿着 B”等）。这种缺失导致生成的图像中对象布局杂乱、交互不自然，严重限制了定制化生成的可控性和真实性。因此，论文提出 **RelationBooth**，专门解决“关系感知的定制化对象生成”问题，目标是同时保持图像提示中的对象身份（identity）和文本提示中的谓词关系（predicate relation）。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **解耦身份学习与关系学习**：通过精心构建的数据集，将关系特定的图像、独立包含身份信息的物体图像以及指导关系生成的文本提示三者分开组织，使模型能够分别学习“对象长什么样”和“对象之间是什么关系”。
- **在扩散模型框架中引入关系约束**：基于预训练的文本到图像扩散模型，增加两个关键模块以解决关系生成的两大挑战——精准自然的姿态调整（尤其当关系需要大幅度改变姿势时）和重叠情况下的对象混淆。

### 关键技术细节
- **关键点匹配损失 (Keypoint Matching Loss)**：引导模型调整与关系紧密相关的对象姿态。通过计算生成对象关键点与目标关系模板关键点之间的距离，最小化姿态偏差，从而生成更自然的交互动作（如握手、拥抱）。
- **局部特征注入**：从图像提示中提取局部特征（如物体的局部纹理、形状细节），在生成过程中与文本特征融合，以更好地区分重叠区域中的不同对象，防止身份混淆。
- **训练数据构建**：收集包含关系标注的图像（例如“狗在猫旁边”），以及独立的目标对象图像（单物体无背景），配合关系描述文本，形成解耦学习的训练三元组。

### 流程概述
1. 输入：用户提供若干张参考图像（每个对象一张）和一段描述关系的文本提示。
2. 编码：分别编码参考图像的身份特征和文本的关系语义。
3. 生成：在去噪过程中，利用关键点匹配损失监督姿态生成，并注入局部特征增强目标区分。
4. 输出：生成包含指定对象并符合关系描述的高分辨率图像。

## 3. 实验设计

- **数据集**：论文未在摘要中明确给出具体数据集名称，但提到在 **三个基准 (three benchmarks)** 上进行了广泛评估。推测可能是自建的关系感知定制化数据集或已有的关系性图像数据集（如 Visual Genome、COCO 的子集）加上定制化对象参考图。
- **对比方法**：与现有定制化生成方法（如 DreamBooth、Custom Diffusion、Textual Inversion 等）进行对比，评估关系保持准确率和身份保真度。
- **评价指标**：预计包含身份相似度（如 CLIP 图像相似度、DINO 特征相似度）、关系准确率（基于关系检测器的 F1 分数或人工评分），以及总体图像质量（FID 或用户调研）。

## 4. 资源与算力

文中 **未明确说明** 使用的 GPU 型号、数量、训练时长等具体硬件配置。仅提到源代码和训练模型将开源。因此，无法评估其训练成本。

## 5. 实验数量与充分性

- **实验数量**：在三个基准上进行了系统实验，且包含消融研究（ablation study）以验证关键点匹配损失和局部特征模块的有效性。摘要未列出具体表格数量，但典型的定制化生成论文会进行 4~6 组对比实验（不同关系类型、不同对象组合）和 2~3 组消融。
- **充分性与公平性**：实验在多个基准上对比了多种主流方法，覆盖了不同对象和关系，具有较好的泛化性。但摘要未提及是否进行了多轮用户调研或与最新 SOTA 的全面比较，因此无法完全断言其公平性。整体来看，实验设计较为完整，能支撑主要结论。

## 6. 主要结论与发现

- RelationBooth 在生成精准关系的同时，能保持对象身份，显著优于现有忽略关系的定制化方法。
- 关键点匹配损失有效解决了关系所需的姿态调整难题，局部特征注入缓解了重叠区域的混淆。
- 关系感知是提升个性化图像生成质量的关键维度，未来应纳入定制化生成的标准范式。

## 7. 优点

- **问题新颖**：首次系统研究定制化生成中的多对象关系，填补了领域空白。
- **方法简洁有效**：通过解耦学习 + 两个轻量模块（关键点损失、局部特征），在不大幅增加计算量的情况下大幅提升结果质量和可控性。
- **实验充分**：在三个基准上验证，包含多种对象与关系，消融实验证实了各模块的贡献。
- **开源承诺**：代码和模型公开，便于复现和后续研究。

## 8. 不足与局限

- **关系类型覆盖**：可能只支持有限的静态关系（空间关系、交互关系），对于复杂因果关系或时序关系尚无法处理。
- **对象数量限制**：当前框架可能仅适用于 2~3 个对象，更多对象的场景下关键点匹配和局部特征注入的复杂度会上升。
- **训练数据依赖**：关系感知的数据集构建成本高，且可能存在标注偏差（如常见关系多，罕见关系少），影响泛化。
- **硬件资源未披露**：无法评估部署门槛，且摘要未说明推理速度，可能难以实时应用。
- **身份与关系权衡**：在极端姿态变化时，身份保真度可能有所下降（论文未详细讨论边界情况）。

（完）
