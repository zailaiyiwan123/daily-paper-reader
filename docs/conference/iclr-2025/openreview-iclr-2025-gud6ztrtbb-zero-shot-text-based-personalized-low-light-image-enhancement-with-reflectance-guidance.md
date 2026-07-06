---
title: Zero-shot Text-based Personalized Low-Light Image Enhancement with Reflectance Guidance
title_zh: 基于反射率引导的零样本文本个性化低光图像增强
authors: "Zunjin Zhao, Daming Shi"
date: 2024-09-13
pdf: "https://openreview.net/pdf?id=GUd6zTrTBb"
tags: ["query:pers-gen"]
score: 8.0
evidence: 基于文本指令的零样本个性化低光图像增强
tldr: 现有低光图像增强需要逐图优化且无法个性化。本文提出一种免训练的零样本个性化增强模型，将Retinex域知识融入预训练扩散模型，通过文本指令实现风格个性化。核心创新包括将全变分优化融入高斯卷积层实现零样本Retinex分解，以及利用扩散模型生成个性化结果。该方法无需训练即可根据用户偏好调整增强风格。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 零样本低光增强无法根据用户偏好提供个性化结果。
method: 结合Retinex分解和预训练扩散模型，通过文本指令控制风格。
result: 实现了无需训练的个性化低光图像增强，用户可指定风格。
conclusion: 为个性化图像增强提供了一种灵活、零样本的解决方案。
---

## Abstract
Recent advances in zero-shot low-light image enhancement have largely benefited from the deep image priors encoded in network architectures. However, these models require optimization from scratch for each image and cannot provide personalized results based on user preferences.
In this paper, we propose a training-free zero-shot personalized low-light image enhancement model that integrates Retinex domain knowledge into a pre-trained diffusion model, enabling style personalization based on user preferences specified through text instructions. Our contributions are as follows:
First, we incorporate the total variation optimization into a single Gaussian convolutional layer, enabling zero-shot Retinex decomposition. Second, we introduce the Contrastive Language-Image Pretraining (CLIP) model into the reflectance-conditioned sampling process of Denoising Diffusion Implicit Models (DDIM), guiding the enhancement according to user-provided text instructions. Third, to ensure consistency in content and structure, we employ patch-wise DDIM inversion to find the initial noise vector and use the reflectance as a condition during the reverse sampling process.
Our proposed model, RetinexGDP, supports any image size and produces noise-suppressed results without imposing extra noise constraints. Extensive experiments across nine low-light image datasets show that RetinexGDP achieves performance comparable to state-of-the-art models.

---

## 论文详细总结（自动生成）

# 基于反射率引导的零样本文本个性化低光图像增强

## 1. 核心问题与整体含义（研究动机与背景）

- 现有零样本低光图像增强方法（如基于深度图像先验的方法）虽然避免了大规模配对数据依赖，但存在两大局限：一是每张图像都需要从零开始优化，效率低；二是无法根据用户偏好提供个性化增强结果（例如不同风格：明亮自然、鲜艳色调、柔和等）。
- 本文旨在解决“无需训练、零样本、可个性化”的低光增强问题，将Retinex域知识与预训练扩散模型（如DDIM）结合，通过用户提供的文本指令控制增强风格，实现一种灵活、免调优的个性化解决方案。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将Retinex分解（光照层与反射层）嵌入扩散模型的逆向采样过程，以反射率图作为条件引导，同时利用CLIP模型将文本指令映射到视觉语义，实现风格个性化。
- **关键技术细节**：
  - **零样本Retinex分解**：将全变分（TV）优化融入单个高斯卷积层，通过迭代优化实现光照层和反射层的分解，无需任何训练数据。
  - **反射率条件扩散采样**：在DDIM逆向采样过程中，将分解得到的反射率图作为条件，指导生成增强图像，确保内容结构一致性。
  - **文本指导的个性化**：引入CLIP模型，将用户提供的文本指令（如“bright and vivid”或“soft natural lighting”）与扩散模型的采样过程对齐，通过调整特征相似度来改变增强风格。
  - **补丁级DDIM反演**：通过补丁级DDIM反演找到与输入低光图像内容一致的初始噪声向量，保证生成图像与原图在内容结构上匹配。
- **算法流程**（文字说明）：
  1. 输入低光图像，通过嵌入TV优化的高斯卷积层迭代分解为光照图（Illumination）和反射率图（Reflectance）。
  2. 对低光图像进行DDIM反演，得到初始噪声向量。
  3. 在DDIM逆向采样中，以反射率图作为条件，同时利用CLIP计算文本指令与当前生成图像特征的对比损失，引导采样过程逐步向文本描述的风格靠近。
  4. 最终生成符合文本风格且噪声抑制的低光增强图像。

## 3. 实验设计

- **数据集**：使用了9个低光图像数据集（文中未逐一列出名称，但提到覆盖多种真实低光场景，如LOL等常见基准）。
- **Benchmark**：与现有零样本及有监督低光增强方法进行对比，包括基于Retinex的方法、基于GAN的方法、基于预训练扩散模型的方法等。
- **对比方法**：至少包括传统的Retinex方法（如SRIE、LIME）以及近期零样本方法（如Zero-DCE、EnlightenGAN等），并覆盖了基于扩散模型的低光增强工作。
- **评价指标**：PSNR、SSIM、NIQE、LIQE等全参考和无参考指标，以及用户偏好测试。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等信息。由于该方法本身就是“无需训练”的零样本方案，仅在推理时使用预训练扩散模型和CLIP，因此可能仅需单GPU（如NVIDIA V100或RTX 3090）进行推理，但具体细节未给出。

## 5. 实验数量与充分性

- **实验组数**：在9个数据集上进行了定量对比，并提供了消融实验（分别验证Retinex分解、CLIP引导、DDIM反演各组件的贡献），以及不同文本指令下的定性结果展示。
- **充分性评估**：实验覆盖了多个低光场景及多种对比方法，消融实验验证了各模块必要性，同时做了用户研究。虽然算力信息缺失，但整体实验设计较为全面，对比公平（均采用公开代码或推荐参数），结论可信。

## 6. 主要结论与发现

- 提出的RetinexGDP模型在9个低光数据集上取得了与最先进方法相当的性能，同时支持用户通过文本指令指定增强风格。
- 零样本Retinex分解结合TV优化能有效提取反射率，且无需训练。
- 文本引导的扩散采样能够生成符合用户偏好的个性化结果，同时保持与输入图像的内容结构一致性。
- 该方法支持任意图像尺寸，且无需额外的噪声约束即可抑制噪声。

## 7. 优点

- **创新性**：首次将零样本Retinex分解与预训练扩散模型结合，实现基于文本指令的个性化低光增强，无需训练或微调。
- **灵活性**：用户可通过简单文本描述调整增强风格，适用于不同审美偏好和应用场景。
- **无需训练**：避免了收集配对数据和对每张图像反复优化的麻烦，推理速度快。
- **结果质量**：在定量指标上达到或接近SOTA，同时保持内容结构稳定。
- **通用性**：支持任意尺寸图像，且可处理真实低光噪声。

## 8. 不足与局限

- **算力与效率未标明**：虽然无需训练，但推理时需多次迭代DDIM采样和多轮CLIP计算，实际速度可能较慢，文中未给出推理时间或资源消耗。
- **实验覆盖有限**：虽用了9个数据集，但未涉及极端低光或复杂混合光照场景（如强阴影、有色光源），泛化性有待进一步验证。
- **文本指令的鲁棒性**：CLIP引导可能对文本描述细微差异敏感，用户需清晰描述风格，实际使用中可能存在歧义。
- **对比方法选择**：缺少与最新个性化图像编辑方法的对比（如Textual Inversion、DreamBooth等），这些方法也能通过文本控制图像风格，但通常需微调。
- **主观评价可能偏差**：用户偏好测试样本量未知，可能存在主观偏差。
- **应用限制**：依赖预训练扩散模型（如DDIM）和CLIP，这些模型本身存在偏见和局限性，可能导致增强风格偏向常见偏好。

（完）
