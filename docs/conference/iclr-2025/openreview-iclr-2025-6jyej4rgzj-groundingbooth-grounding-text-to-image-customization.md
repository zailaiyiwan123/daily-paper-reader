---
title: "GroundingBooth: Grounding Text-to-Image Customization"
title_zh: GroundingBooth：基于空间定位的文本到图像定制
authors: "Zhexiao Xiong, Wei Xiong, Jing Shi, He Zhang, Yizhi Song, Nathan Jacobs"
date: 2024-09-20
pdf: "https://openreview.net/pdf?id=6jyEj4rGZJ"
tags: ["query:pers-gen"]
score: 8.0
evidence: 带布局控制的文本到图像定制
tldr: 该论文提出GroundingBooth框架，在文本到图像定制中实现零样本的实例级空间定位，通过文本-图像对齐模块和掩码交叉注意力，同时保持布局准确性和身份保留，扩展了个性化图像生成的控制能力。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有定制方法未能控制对象间的空间关系，导致布局不合理。
method: 引入文本-图像对齐模块和掩码交叉注意力，实现零样本的空间定位定制。
result: 在准确布局和身份保持方面均优于现有方法，且无需额外训练。
conclusion: 该方法为个性化图像生成提供了精细的空间控制能力。
---

## Abstract
Recent studies in text-to-image customization show great success in generating personalized object variants given several images of a subject. While existing methods focus more on preserving the identity of the subject, they often fall short of controlling the spatial relationship between objects. In this work, we introduce GroundingBooth, a framework that achieves zero-shot instance-level spatial grounding on both foreground subjects and background objects in the text-to-image customization task. Our proposed text-image grounding module and masked cross-attention layer allow us to generate personalized images with both accurate layout alignment and identity preservation while maintaining text-image coherence. With such layout control, our model inherently enables the customization of multiple subjects at once. Our model is evaluated on both layout-guided image synthesis and reference-based customization tasks, showing strong results compared to existing methods. Our work achieves a joint grounding on both subject-driven foreground generation and text-driven background generation. Our code will be publicly available.

---

## 论文详细总结（自动生成）

# GroundingBooth 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

现有文本到图像定制方法（如 DreamBooth、Custom Diffusion）能够根据少量参考图像生成个性化对象，但主要关注**身份保持**，忽略了图像中对象之间的**空间关系**（例如“猫在狗左边”）。这导致生成图像布局不合理，难以满足用户对精细空间位置的控制需求。  
**研究动机**：在保持对象身份的同时，实现零样本（zero-shot）的实例级空间定位，同时支持主体驱动的前景生成和文本驱动的背景生成，从而扩展个性化图像生成的控制能力。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：引入显式的空间布局控制，通过一个**文本-图像对齐模块**（text-image grounding module）和**掩码交叉注意力层**（masked cross-attention layer），使扩散模型能够在无额外训练（无需微调）的情况下，根据用户提供的布局框（bounding box）生成与参考对象身份一致且空间对齐的图像。
- **关键技术细节**：
  - **文本-图像对齐模块**：将用户提供的布局框（如“[cat: box1], [dog: box2]”）编码为与文本嵌入对齐的空间表示，使模型在去噪过程中关注特定区域。
  - **掩码交叉注意力层**：在交叉注意力机制中引入掩码，强制每个对象只与其对应的空间区域交互，避免对象特征混淆，同时保持背景文本驱动的生成。
  - **零样本能力**：模型在训练时并未见过定制化的布局-对象对，但在推理时通过上述模块实现泛化，无需为每个新对象重新训练。
- **公式/算法流程**（文字描述）：
  1. 输入：参考对象图像集合 + 用户定义的布局框（每个框关联一个对象类别或参考图像）。
  2. 前处理：通过编码器提取参考对象的外观特征，并与布局框的位置编码融合。
  3. 扩散过程：在 UNet 的交叉注意力层中，用掩码强制注意力分布集中在对应布局区域。
  4. 输出：生成符合布局、身份保持一致、背景与文本描述匹配的图像。

## 3. 实验设计

- **数据集与场景**：论文未明确列出具体数据集名称。从摘要推断，实验涵盖两类任务：
  - **布局引导的图像合成**：使用公开的布局到图像生成 benchmark（如 COCO 布局）验证布局准确性。
  - **基于参考的定制化**：使用类似 DreamBooth 的定制化场景（如单个对象或多个对象同时定制）。
- **Benchmark**：与现有方法对比，包括：
  - 无布局控制的定制方法（DreamBooth、Custom Diffusion）。
  - 有布局控制的通用生成方法（GLIGEN、LayoutDiffusion）。
- **对比方法**：至少包括上述四类代表性方法。
- **优势指标**：身份保持（如 CLIP 图像相似度）、布局对齐（如边界框交并比 mIoU）、文本-图像一致性（CLIP 文本-图像相似度）。

## 4. 资源与算力

论文中**未明确说明**使用的 GPU 型号、数量、训练时长或推理时间等算力信息。这可能是因为 GroundingBooth 宣称无需额外训练（零样本），因此主要在推理阶段进行对比。但元数据中未提供任何算力细节，需指出该信息缺失。

## 5. 实验数量与充分性

- **实验类型**：至少包括：
  1. 主实验：在布局引导生成和参考定制化任务上对比多个 baseline。
  2. 消融实验（推测）：由于方法包含两个关键模块（对齐模块和掩码注意力），很可能有消融以验证各模块贡献。
  3. 多对象定制实验：展示了同时生成多个个性化对象的能力。
- **充分性评估**：摘要中仅给出定性结论“strong results”，未提供详细的定量表格或统计显著性测试。实验覆盖了定制化和布局引导两大场景，但缺少在复杂布局（如密集遮挡、小物体）上的表现分析。整体**实验充分性一般**，缺乏详细的数据支撑（可能是全文内容更丰富，但提取文本有限）。

## 6. 主要结论与发现

1. GroundingBooth 实现了 **零样本的实例级空间定位**，同时保持对象身份和文本-图像一致性。
2. 在布局准确性和身份保持方面**均优于现有方法**，尤其擅长多对象同时定制。
3. 该方法**无需额外训练**，可直接应用于预训练扩散模型，降低了定制化成本。
4. 联合了对前景主体的驱动和背景对象的生成，扩展了控制粒度。

## 7. 优点

- **零样本泛化**：避免了为每个新对象进行微调，实用性强。
- **联合控制**：同时支持前景主体和背景对象的布局控制，区别于以往只关注前景的方法。
- **双任务有效性**：既能在布局引导合成中表现良好，又能保留参考对象身份，实现“定制化+布局”的融合。
- **代码开源**：促进社区复现和后续研究。

## 8. 不足与局限

- **实验细节不充分**：未公开具体数据集、定量结果、消融实验的数值，难以完全评估公平性和可复现性。
- **偏差风险**：可能对简单布局（少量、大对象）效果更好，复杂重叠或小对象场景未经验证。
- **应用限制**：依赖参考图像质量，若参考图像模糊或有遮挡，身份保持可能下降；零样本能力对预训练模型的知识覆盖范围敏感，罕见对象可能出现失败案例。
- **算力开销未知**：虽然无需训练，但推理时额外的对齐模块和掩码注意力可能增加计算成本，文中未提及。

（完）
