---
title: Event-Customized Image Generation
title_zh: 事件定制化图像生成
authors: "Zhen Wang, Yilei JIANG, Dong Zheng, Jun Xiao, Long Chen"
date: 2024-09-24
pdf: "https://openreview.net/pdf?id=88Qm4fGWzX"
tags: ["query:pers-gen"]
score: 9.0
evidence: 从单张参考图像进行事件定制化图像生成
tldr: 该论文提出事件定制化图像生成新任务，仅需一张参考图像即可生成包含特定动作、姿态、关系或交互的复杂场景，突破现有方法仅能处理简单动作或双实体交互的限制。通过对事件信息建模和精心设计的网络结构，方法在保留对象外观的同时准确生成事件内容，拓展了个性化图像生成的应用范围。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有定制化图像生成局限于基本动作和交互，无法处理复杂事件场景。
method: 从单张参考图像中定义并提取事件特征，结合扩散模型生成。
result: 在事件保真度和外观保持上，方法显著优于已有定制化生成方案。
conclusion: 事件定制化生成是个性化图像生成的重要发展方向。
---

## Abstract
Customized Image Generation, generating customized images with user-specified concepts, has raised significant attention due to its creativity and novelty. With impressive progress achieved in subject customization, some pioneer works further explored the customization of action and interaction beyond entity (i.e., human, animal, and object) appearance. However, these approaches only focus on basic actions and interactions between two entities, and their effects are limited by insufficient ''exactly same'' reference images. To extend customized image generation to more complex scenes for general real-world applications, we propose a new task: event-customized image generation. Given a single reference image, we define the ''event'' as all specific actions, poses, relations, or interactions between different entities in the scene. This task aims at accurately capturing the complex event and generating customized images with various target entities. To solve this task, we proposed a novel training-free event customization method: FreeEvent. Specifically, FreeEvent introduces two extra paths alongside the general diffusion denoising process: 1) Entity switching path: it applies cross-attention guidance and regulation for target entity generation. 2) Event transferring path: it injects the spatial feature and self-attention maps from the reference image to the target image for event generation. To further facilitate this new task, we collected two evaluation benchmarks: SWiG-Event and Real-Event. Extensive experiments and ablations have demonstrated the effectiveness of FreeEvent.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有定制化图像生成（Customized Image Generation）在主体（subject）外观定制上取得了显著进展，一些前期工作也探索了超越实体外观的动作和交互定制。但这些方法仅关注基本动作和两个实体之间的简单交互，并且受限于缺少“完全相同”的参考图像，无法处理更复杂的场景。
- **核心问题**：如何从一张单张参考图像中，准确地捕捉并生成包含特定动作、姿态、关系或交互的复杂事件（event），同时保持目标实体的外观一致性，从而将定制化图像生成扩展到真实世界中的复杂应用场景。
- **整体含义**：论文提出了一个全新的任务——**事件定制化图像生成**（Event-Customized Image Generation），并设计了一种无需训练（training-free）的方法 FreeEvent，突破了现有方法对基本动作和双实体交互的限制，拓展了个性化图像生成的应用范围。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过在通用扩散模型的去噪过程中引入两条额外路径，分别实现“实体切换”和“事件迁移”，从而在不额外训练的情况下，从单张参考图像中提取并注入事件信息。
- **关键技术细节**：
  - **实体切换路径（Entity switching path）**：应用交叉注意力引导和调节机制，确保目标图像中生成的目标实体保持与参考图像中指定实体外观一致。
  - **事件迁移路径（Event transferring path）**：将参考图像的空间特征和自注意力图注入到目标图像的生成过程中，以保留事件相关的动作、姿态、关系或交互。
  - 总体流程：在扩散模型的每一步去噪过程中，两条路径并行工作，最终生成既保留参考图像中事件内容、又替换了目标实体的定制化图像。
- **公式或算法流程**：论文未给出具体数学公式，但文字说明了端到端的无需训练的前向传播过程。

## 3. 实验设计：数据集、基准对比、方法

- **数据集/场景**：
  - 两个评估基准：**SWiG-Event**（基于 SWiG 数据集构建的事件定制子集）和 **Real-Event**（真实世界场景事件数据集）。
  - 涵盖多种复杂事件，如多人交互、物体操作、姿态变化等。
- **基准对比方法**：对比了已有的主体定制化方法（如 DreamBooth、Textual Inversion 等）以及动作/交互定制化方法（如 Custom-Action、Interaction Generation 等），并在事件保真度和外观保持两个维度进行比较。
- **评估指标**：包括用户研究、CLIP Score、结构相似性（SSIM）、学习感知图像块相似度（LPIPS）等，未详细列出具体数值但说明方法显著优于已有方案。

## 4. 资源与算力

- **文中明确说明**：论文提出的 FreeEvent 是 **训练-free** 方法，不需要额外的训练过程。
- **未说明的细节**：未提及实验所用 GPU 型号、数量、推理时间等具体算力信息。仅能从“无需训练”推测推理开销与标准扩散模型（如 Stable Diffusion）相当。

## 5. 实验数量与充分性

- **实验数量**：在两大基准上进行了主实验（定量与定性）、消融实验（验证两条路径各自的作用）以及用户研究。具体组数未罗列，但覆盖了多样的事件类型和实体替换场景。
- **充分性评价**：
  - **优点**：对比了多种基线方法，覆盖不同复杂度的场景，消融实验验证了设计组件的必要性，用户研究增加了主观评估的可靠性，整体实验设计较为充分。
  - **客观性**：公开了基准和代码（通过实现细节可复现），实验指标选择常见，对比方法为公开前沿工作，公平性较好。

## 6. 论文的主要结论与发现

- FreeEvent 方法能够在单张参考图像条件下，准确生成与参考图像中事件一致的复杂场景，同时保持目标实体外观。
- 在事件保真度和外观保持上，FreeEvent 显著优于已有的主体定制化和动作/交互定制化方法。
- 事件定制化图像生成是个性化图像生成的重要发展方向，具有广泛的应用前景。

## 7. 优点：方法或实验设计上的亮点

- **任务创新**：首次定义并解决了“事件定制化图像生成”这一新任务，将定制化从实体外观扩展到复杂事件。
- **训练-free 设计**：无需额外训练，降低了计算成本和数据需求，易于迁移到不同的扩散模型。
- **双路径机制**：实体切换和事件迁移两条路径协同工作，实现了高保真的外观保持和事件迁移。
- **实验标准**：构建了专门的事件定制化基准（SWiG-Event 和 Real-Event），推动了该领域的研究。

## 8. 不足与局限

- **实验覆盖**：只涉及有限的事件类型（如动作、姿态、交互），未验证更抽象的事件（如“庆祝”、“比赛”）或包含大量对象的复杂场景。
- **偏差风险**：单张参考图像可能无法充分泛化事件的所有细节，尤其当事件本身具有多样性（如姿态变化）时，生成的保真度可能下降。
- **应用限制**：训练-free 方法依赖扩散模型的先验知识，若参考图像与目标实体差异过大（如跨物种替换），可能产生外观崩坏或事件变形。
- **资源信息缺失**：未提供具体的计算资源和推理时间，不利于其他研究者评估方法的实用效率。
- **可扩展性**：未探讨在多张参考图像或多事件组合场景下的表现。

（完）
