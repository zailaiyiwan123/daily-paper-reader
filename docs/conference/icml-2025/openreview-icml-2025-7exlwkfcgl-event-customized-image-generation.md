---
title: Event-Customized Image Generation
title_zh: 事件定制图像生成
authors: "Zhen Wang, Yilei JIANG, Dong Zheng, Jun Xiao, Long Chen"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=7exLwkfcGL"
tags: ["query:pers-gen"]
score: 9.0
evidence: 用户指定概念的事件定制图像生成
tldr: 现有的定制图像生成主要集中在主体外观的定制上，忽略了复杂事件（如动作、交互）的定制。本文提出事件定制图像生成新任务，仅需一张参考图像即可定制图像中的事件内容，将定制化扩展到更复杂的场景，具有重要的实际应用价值。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有定制图像生成方法仅能处理简单的主体外观定制，无法应对复杂事件的定制需求，且依赖多张参考图像。
method: 提出事件定制图像生成任务，设计基于单张参考图像的事件提取与图像生成框架，捕捉用户的特定事件概念。
result: 在多个复杂事件场景上验证了方法的有效性，生成的图像能够准确反映用户指定的事件内容。
conclusion: 事件定制图像生成极大地扩展了图像定制化的范围，为更广泛的实际应用提供了可能。
---

## Abstract
Customized Image Generation, generating customized images with user-specified concepts, has raised significant attention due to its creativity and novelty. With impressive progress achieved in subject customization, some pioneer works further explored the customization of action and interaction beyond entity (i.e., human, animal, and object) appearance. However, these approaches only focus on basic actions and interactions between two entities, and their effects are limited by insufficient ''exactly same'' reference images. To extend customized image generation to more complex scenes for general real-world applications, we propose a new task: event-customized image generation. Given a single reference image, we define the ''event'' as all specific actions, poses, relations, or interactions between different entities in the scene. This task aims at accurately capturing the complex event and generating customized images with various target entities. To solve this task, we proposed a novel training-free event customization method: FreeEvent. Specifically, FreeEvent introduces two extra paths alongside the general diffusion denoising process: 1) Entity switching path: it applies cross-attention guidance and regulation for target entity generation. 2) Event transferring path: it injects the spatial feature and self-attention maps from the reference image to the target image for event generation. To further facilitate this new task, we collected two evaluation benchmarks: SWiG-Event and Real-Event. Extensive experiments and ablations have demonstrated the effectiveness of FreeEvent.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

现有的定制图像生成（Customized Image Generation）方法主要聚焦于主体外观的定制（如人物、动物、物体的外貌），虽然已有工作初步探索了动作和交互的定制，但仅限于**基本动作**和**两个实体之间的简单交互**，无法处理包含多个实体、复杂关系、动作和互动的复杂场景。此外，现有方法通常依赖**多张“完全相同”的参考图像**，限制了实际应用的灵活性。

为了解决上述局限性，本文提出了一项新任务——**事件定制图像生成（Event-Customized Image Generation）**。其核心动机是：仅需**一张参考图像**，就能准确捕捉图像中的“事件”（即场景中所有特定的动作、姿态、关系或实体间的交互），并基于用户指定的不同目标实体生成包含该事件的定制图像。这一任务将定制化图像生成从简单外观扩展到复杂事件场景，具有重要的实际应用价值（如电影制作、游戏设计、虚拟现实等）。

## 2. 论文提出的方法论：核心思想、关键技术细节

本文提出一种**无需训练**的事件定制方法：**FreeEvent**。其核心思想是在通用的扩散模型去噪过程中引入两条额外的路径，分别控制实体生成和事件迁移，从而实现事件与实体的解耦定制。

- **整体框架**：基于预训练的扩散模型（如Stable Diffusion），在标准去噪流程基础上增加：
  1. **Entity Switching Path（实体切换路径）**：通过交叉注意力引导（cross-attention guidance）和调节（regulation），强制目标图像中的实体按照用户指定的新实体生成，同时保留参考图像中实体间的空间关系和交互。
  2. **Event Transferring Path（事件迁移路径）**：将参考图像的空间特征（spatial feature）和自注意力图（self-attention maps）注入到目标图像的去噪过程中，从而在目标图像中复现参考图像中的事件（如动作、姿态、交互）。

- **关键技术细节**：
  - 无需额外训练，直接利用预训练扩散模型的先验知识。
  - 两条路径并行工作：实体切换路径通过修改交叉注意力图改变实体身份；事件迁移路径通过替换或混合自注意力图保持事件结构。
  - 通过注意力引导和特征注入的协同作用，实现事件与实体的解耦控制。

## 3. 实验设计

- **数据集/场景**：
  - **SWiG-Event**：基于SWiG数据集构建的事件定制基准，包含多种复杂事件（如人物互动、物体操作等）。
  - **Real-Event**：从真实世界中收集的复杂事件图像，包含多样化的场景和实体。

- **Benchmark**：论文为这两个数据集提供了定性和定量的评测指标，包括事件保真度（Event Fidelity）、实体一致性（Entity Consistency）等。

- **对比方法**：主要包括现有的定制图像生成方法（如DreamBooth、Custom Diffusion等）以及专注于动作/交互定制的方法。由于FreeEvent是首个解决该任务的训练免费方法，对比实验主要验证其相对于现有迁移方法的优势。

- **消融实验**：验证了实体切换路径和事件迁移路径各自的有效性，以及注意力引导强度等超参数的影响。

## 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量及训练时长。由于FreeEvent是**无需训练**的方法，仅在推理阶段利用预训练扩散模型（如Stable Diffusion），因此算力消耗主要取决于推理时的计算成本（如扩散步数、注意力操作等）。但具体硬件细节未被提及。

## 5. 实验数量与充分性

- 实验中包含了**两个不同来源的基准数据集**（SWiG-Event和Real-Event），覆盖了多种复杂事件类型。
- 进行了**定性和定量**评估，展示了生成的视觉结果和指标得分。
- 消融实验分析了关键组件的作用，验证了方法设计的合理性。
- 对比了多种基线方法，体现了FreeEvent的优势。
- 总体而言，实验设计较为充分，覆盖了任务提出的主要方面。但论文未提供更细粒度的统计（如重复实验次数、标准差等），可能存在一定的随机性未充分报告。

## 6. 论文的主要结论与发现

- 事件定制图像生成是可行的，仅需单张参考图像即可将复杂事件迁移到不同的目标实体上。
- FreeEvent作为无需训练的方法，通过引入实体切换路径和事件迁移路径，能够有效地解耦实体外观和事件内容，生成准确反映用户指定事件的图像。
- 在多个复杂事件场景上，FreeEvent生成的图像在事件保真度和实体一致性上优于现有定制方法。
- 实验证明，空间特征和自注意力图的注入对于事件迁移至关重要，而交叉注意力引导则有效控制了实体身份的变化。

## 7. 优点

- **任务创新性**：将定制图像生成从简单外观/动作扩展到复杂事件，填补了该方向的空白，具有广泛的应用前景。
- **方法简洁高效**：无需额外训练，直接基于预训练扩散模型修改推理过程，降低了计算资源需求。
- **设计直觉清晰**：两条路径分别负责实体切换和事件迁移，解耦效果好，且易于理解和复现。
- **评估基准完善**：构建了两个具有挑战性的基准数据集，为后续研究提供了标准评估平台。

## 8. 不足与局限

- **通用性验证不足**：实验仅在两个数据集上验证，未涵盖更多极端或罕见的事件类型，可能存在对特定事件类型的偏好。
- **未提供算力信息**：缺少推理速度、显存占用等资源消耗指标，不利于实际部署评估。
- **无训练方案的依赖**：FreeEvent依赖预训练扩散模型的知识，对于模型未见过的事件或实体组合，可能迁移效果下降。
- **忽略可编辑性**：当前方法只能将参考事件整体迁移，不支持用户对事件的部分属性（如动作幅度、交互对象等）进行精细编辑。
- **公平性风险**：未讨论当参考图像和目标实体差异过大时（如人类→非人形物体）的鲁棒性，可能存在概念偏差。

（完）
