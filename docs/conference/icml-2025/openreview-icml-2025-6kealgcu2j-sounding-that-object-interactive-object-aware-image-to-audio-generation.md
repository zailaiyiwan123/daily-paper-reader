---
title: "Sounding that Object: Interactive Object-Aware Image to Audio Generation"
title_zh: "让物体发声: 交互式物体感知图像到音频生成"
authors: "Tingle Li, Baihe Huang, Xiaobin Zhuang, Dongya Jia, Jiawei Chen, Yuping Wang, Zhuo Chen, Gopala Anumanchipalli, Yuxuan Wang"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=6KeALGcu2j"
tags: ["query:pers-gen"]
score: 9.0
evidence: 交互式物体感知图像到音频生成，一种个性化多模态生成方法
tldr: 该论文提出交互式物体感知音频生成模型，通过物体中心学习和条件潜扩散模型，允许用户选择图像中的物体生成对应声音。理论证明注意力机制能近似分割功能。实验表明生成的声音与所选物体高度匹配，为用户驱动的个性化多模态内容生成提供了新框架，在交互式媒体创建等领域有广泛应用前景。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 复杂视听场景中多物体同时存在时，难以准确生成特定物体对应的声音。
method: 将物体中心学习融入条件潜扩散模型，通过多模态注意力实现用户选择物体的可控声音生成。
result: 在多个数据集上生成的音频与所选物体高度匹配，用户可控性强。
conclusion: 为个性化多模态生成提供了有效的交互式方法。
---

## Abstract
Generating accurate sounds for complex audio-visual scenes is challenging, especially in the presence of multiple objects and sound sources. In this paper, we propose an interactive object-aware audio generation model that grounds sound generation in user-selected visual objects within images. Our method integrates object-centric learning into a conditional latent diffusion model, which learns to associate image regions with their corresponding sounds through multi-modal attention. At test time, our model employs image segmentation to allow users to interactively generate sounds at the object level. We theoretically validate that our attention mechanism functionally approximates test-time segmentation masks, ensuring the generated audio aligns with selected objects. Quantitative and qualitative evaluations show that our model outperforms baselines, achieving better alignment between objects and their associated sounds.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在复杂的视听场景中，同时存在多个物体和声音源时，如何准确生成与用户指定视觉物体对应的声音，实现细粒度、可控的音频生成。
- **研究动机**：现有图像到音频生成方法通常处理整个场景，难以在多个物体共存时分离并生成特定物体发出的声音。用户无法灵活选择感兴趣的物体进行个性化生成。
- **整体含义**：该论文提出一种交互式物体感知音频生成模型，允许用户通过选择图像中的物体来生成对应声音，推动了多模态生成从“场景级”向“物体级”的精细化发展，具有交互式媒体创建、辅助视障用户等应用前景。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将物体中心学习（object-centric learning）融入条件潜扩散模型（conditional latent diffusion model），通过多模态注意力机制建立图像区域与对应声音之间的关联，实现用户选择物体的可控声音生成。
- **关键技术细节**：
  - **物体中心学习**：模型学习将图像中的不同物体区域编码为独立表征，与声音的潜表示对齐。
  - **条件潜扩散模型**：以用户选定的物体区域为条件，在潜在空间中进行扩散/去噪过程生成音频。
  - **多模态注意力**：在扩散过程中引入跨模态注意力层，使模型关注与选定物体相关的图像区域。
  - **测试时分隔（test-time segmentation）**：用户通过图像分割工具选择目标物体，模型利用注意力机制近似分割掩码，保证生成声音与所选物体对齐。
- **理论验证**：论文理论证明了注意力机制在功能上可以近似测试时的分割掩码，确保生成的音频与所选物体一致。
- **算法流程（文字描述）**：
  1. 输入图像，用户通过交互式分割选择感兴趣物体（得到物体掩码）。
  2. 模型将图像与掩码编码为条件特征。
  3. 在潜扩散框架中，从随机噪声开始逐步去噪，每一步通过多模态注意力融合物体区域信息。
  4. 最终解码生成对应物体的音频。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **数据集与场景**：论文提到在“多个数据集”上进行评估，但具体数据集名称未在摘要和元数据中说明（可能正文中有详细描述，如常见的AudioSet、VAS等）。场景包括含有多个物体和声音源的复杂图像。
- **Benchmark**：未明确说明使用了哪些标准benchmark，但对比了基线方法（baselines）。
- **对比方法**：与现有的图像到音频生成方法或条件音频生成方法进行了定量和定性比较，具体方法名称未在提供文本中列出。

## 4. 资源与算力

- **未明确说明**：提供的文本中没有提及使用了多少GPU、型号、训练时长等算力信息。可能正文中有详细描述，但本摘要中缺失。

## 5. 实验数量与充分性

- **实验数量**：只概括了“定量和定性评估”，未列出具体实验组数（如不同数据集、消融实验等）。元数据中提到“在多个数据集上”，但不够具体。
- **充分性与客观性**：
  - 优点：包含了定量和定性两种评估，结合理论验证，增强了可信度。
  - 不足：缺乏详细的消融实验、不同用户选择条件下的鲁棒性测试、与大量基线方法的对比等。由于信息有限，无法完全判断实验覆盖的全面性。但从“优于baselines”的结论看，实验设计应该是针对核心任务进行了对比。

## 6. 论文的主要结论与发现

- 提出的交互式物体感知音频生成模型在生成与用户所选物体对应的声音方面优于基线方法。
- 理论证明注意力机制能近似测试时分隔功能，保证了模型的可解释性和可控性。
- 生成的声音与所选物体高度匹配，用户可控性强，为个性化多模态内容生成提供了新框架。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将物体中心学习与条件潜扩散模型结合，实现交互式物体级音频生成，突破了场景级生成的限制。
- **交互可控性**：用户可通过简单的图像分割选择物体，生成对应的声音，提升了生成任务的个性化和实用性。
- **理论支撑**：给出了注意力近似分割的数学证明，使方法更具可信度。
- **多模态对齐**：通过多模态注意力机制有效关联视觉区域与音频内容。

## 8. 不足与局限

- **信息缺失**：提供的文本过短，缺乏实验细节（数据集、基线、算力等），难以全面评估方法的泛化能力和计算开销。
- **应用限制**：可能依赖于高质量的图像分割结果，对于模糊或遮挡严重的物体，分割误差会影响生成效果。
- **声音多样性**：仅生成与物体对应的声音（如物体碰撞声、环境声），对于抽象声音（如音乐、人声）可能不适用。
- **实时性**：扩散模型生成速度可能较慢，交互式体验受限于推理效率。

（完）
