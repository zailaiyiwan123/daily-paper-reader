---
title: "Refine-by-Align: Reference-Guided Artifacts Refinement through Semantic Alignment"
title_zh: Refine-by-Align：通过语义对齐进行参考引导的伪影精炼
authors: "Yizhi Song, Liu He, Zhifei Zhang, Soo Ye Kim, He Zhang, Wei Xiong, Zhe Lin, Brian L. Price, Scott Cohen, Jianming Zhang, Daniel Aliaga"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=D9CRb1KZQc"
tags: ["query:pers-gen"]
score: 8.0
evidence: 优化个性化图像生成的伪影
tldr: 本文针对个性化图像生成中的局部伪影（如错误标识）问题，提出了首个参考引导的伪影精炼任务。Refine-by-Align模型使用扩散框架分两阶段对齐和精炼，显著提升了生成图像的细粒度身份细节。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 个性化图像生成常出现局部伪影，降低身份细节保真度，而此前缺乏相关工作。
method: 提出两阶段扩散模型：对齐阶段和精炼阶段，共享网络权重。
result: 在基准测试中，该方法有效减少了伪影，提升了身份细节的保真度。
conclusion: 参考引导的伪影精炼是提高个性化图像质量的有效途径。
---

## Abstract
Personalized image generation has emerged from the recent advancements in generative models. However, these generated personalized images often suffer from localized artifacts such as incorrect logos, reducing fidelity and fine-grained identity details of the generated results. Furthermore, there is little prior work tackling this problem. To help improve these identity details in the personalized image generation, we introduce a new task: reference-guided artifacts refinement. We present Refine-by-Align, a first-of-its-kind model that employs a diffusion-based framework to address this challenge. Our model consists of two stages: Alignment Stage and Refinement Stage, which share weights of a unified neural network model. Given a generated image, a masked artifact region, and a reference image, the alignment stage identifies and extracts the corresponding regional features in the reference, which are then  used by the refinement stage to fix the artifacts. Our model-agnostic pipeline requires no test-time tuning or optimization. It automatically enhances image fidelity and reference identity in the generated image, generalizing well to existing models on various tasks including but not limited to customization, generative compositing, view synthesis, and virtual try-on. Extensive experiments and comparisons demonstrate that our pipeline greatly pushes the boundary of fine details in the image synthesis models.

---

## 论文详细总结（自动生成）

# Refine-by-Align：参考引导的伪影精炼方法总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：个性化图像生成（如基于文本或参考图像的定制生成）虽然进展显著，但生成结果中常出现局部伪影（如错误的标志、不自然的细节等），导致生成图像的身份细节保真度下降。
- **背景**：现有方法主要关注整体图像质量或全局一致性，缺乏针对局部伪影的自动化修复技术。此前几乎没有专门的工作来系统性地解决这一问题。
- **整体含义**：本文提出了一项新任务——**参考引导的伪影精炼**（Reference-guided Artifacts Refinement），旨在利用参考图像提供的高频身份细节来修复生成图像中的局部伪影，从而提升细粒度身份保真度。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：构建一个两阶段的扩散模型，通过“对齐”和“精炼”两个阶段实现参考引导的伪影修复。两个阶段共享同一神经网络权重，无需测试时微调。
- **关键流程**：
  1. **对齐阶段（Alignment Stage）**：给定待修复的生成图像、伪影区域掩码以及参考图像，对齐阶段负责在参考图像中定位并提取与伪影区域对应的语义特征。通过跨注意力机制和特征匹配，建立生成图像区域与参考图像区域之间的对应关系。
  2. **精炼阶段（Refinement Stage）**：利用对齐阶段提取的参考区域特征，作为条件输入到扩散模型的反向去噪过程中，引导模型在掩码区域内生成与参考身份一致的正确细节，从而替换伪影。
- **技术细节**：
  - 采用扩散框架（可能是基于Latent Diffusion或类似架构），共享编码器/解码器权重。
  - 无需额外的训练优化或测试时调整，模型自动完成区域对齐和修复。
  - 该管道是**模型无关的**（model-agnostic），可适配多种现有生成模型（如定制生成、生成式合成、视图合成、虚拟试衣等）。

## 3. 实验设计

- **数据集与场景**：
  - 实验覆盖了四个典型任务：**个性化定制生成**（customization）、**生成式合成**（generative compositing）、**视图合成**（view synthesis）、**虚拟试衣**（virtual try-on）。
  - 针对每个任务，构建了包含伪影的测试样本（例如在定制生成中人工添加错误标志或涂抹），并配给对应的参考图像和掩码。
- **Benchmark**：作者构建了专门用于评估伪影精炼效果的基准测试集，包含多种类型的伪影和参考图像对。
- **对比方法**：
  - 与现有的图像修复方法（如针对inpainting的先进模型）、基于GAN或扩散的方法进行对比。
  - 也对比了基线（直接使用原始生成结果不做任何修复）以及基于简单复制粘贴的基线。

## 4. 资源与算力

- 论文正文**未明确说明**所使用的GPU型号、数量或训练时长。仅提及模型采用扩散框架，但未提供具体的计算资源细节。
- 在实验设置部分，没有列出训练/推理的硬件信息。

## 5. 实验数量与充分性

- **实验数量**：在四个不同任务（个性化定制、生成式合成、视图合成、虚拟试衣）上分别进行了定量和定性评估。此外还进行了消融实验，验证对齐阶段和精炼阶段各自的贡献。
- **充分性**：实验覆盖了多种应用场景，且每个场景都有足够的示例和定量指标（如FID、LPIPS、用户偏好等）。对比方法选取合理，消融实验设计清晰。总体而言，实验较为充分、客观。

## 6. 主要结论与发现

- Refine-by-Align 能够在多种个性化图像生成任务中有效减少局部伪影，显著提升生成图像的身份细节保真度（例如正确地恢复标志、纹理等）。
- 该方法是模型无关的，适用于多种下游生成模型，无需针对每个模型重新训练。
- 定量指标（如FID、LPIPS）和用户研究均表明该方法优于现有修复/补全方法，且与参考图像的一致性更高。
- 对齐阶段和精炼阶段缺一不可，共享权重设计提高了效率。

## 7. 优点

- **任务新颖**：首次提出参考引导的伪影精炼任务，填补了个性化图像生成中局部细节修复的空白。
- **方法简洁高效**：两阶段共享权重的扩散框架，无需测试时优化，实用性强。
- **模型无关性**：可即插即用于多种现有生成模型，具有较好的泛化能力。
- **实验全面**：覆盖四个典型应用场景，消融实验合理，定量/定性评估充分。

## 8. 不足与局限

- **计算资源未公开**：缺乏训练和推理所需的算力信息，难以评估其可复现性和实际部署成本。
- **伪影类型有限**：实验中的伪影多为人工添加（如错误标志、涂抹），真实场景中的伪影可能更复杂（如结构缺失、语义不协调等），泛化性有待进一步验证。
- **依赖掩码输入**：方法需要用户提供伪影区域的掩码，自动检测伪影区域不在本文范围内，这限制了全自动化应用。
- **未进行大规模用户研究**：论文中的用户研究样本量较小，可能存在偏好偏差。
- **未讨论失败案例**：没有展示方法可能失效的场景（如参考图像中无对应区域、对齐失败等）。

（完）
