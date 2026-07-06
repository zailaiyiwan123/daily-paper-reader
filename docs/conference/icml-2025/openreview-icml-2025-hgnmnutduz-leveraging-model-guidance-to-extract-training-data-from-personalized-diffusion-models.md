---
title: Leveraging Model Guidance to Extract Training Data from Personalized Diffusion Models
title_zh: 利用模型指导从个性化扩散模型中提取训练数据
authors: "Xiaoyu Wu, Jiaru Zhang, Steven Wu"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=HGnMNUTdUz"
tags: ["query:pers-gen"]
score: 8.0
evidence: 直接研究个性化扩散模型
tldr: 针对个性化扩散模型在微调后共享检查点时可能泄露训练数据的问题，本文提出一种利用模型引导的方法来提取训练数据。该方法通过梯度引导和优化从微调模型的反向过程中重建原始图像。实验证明即使是从少量图像微调的模型中也能成功提取，揭示了数据泄露与版权侵犯的风险。该研究为个性化生成模型的安全性评估提供了重要工具。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 个性化扩散模型在社区中共享时可能存在训练数据泄露风险，本文旨在验证这种可能性。
method: 通过模型引导和梯度优化从微调扩散模型的反向过程中提取训练图像。
result: 成功从多种个性化扩散模型中提取出训练数据，证明泄露风险真实存在。
conclusion: 个性化扩散模型共享检查点存在数据泄露威胁，需加强安全措施。
---

## Abstract
Diffusion Models (DMs) have evolved into advanced image generation tools, especially for few-shot fine-tuning where a pretrained DM is fine-tuned on a small set of images to capture specific styles or objects. Many people upload these personalized checkpoints online, fostering communities such as Civitai and HuggingFace. However, model owners may overlook the potential risks of data leakage by releasing their fine-tuned checkpoints. Moreover, concerns regarding copyright violations arise when unauthorized data is used during fine-tuning.  In this paper, we ask: "Can training data be extracted from these fine-tuned DMs shared online?" A successful extraction would present not only data leakage threats but also offer tangible evidence of copyright infringement. To answer this, we propose FineXtract, a framework for extracting fine-tuning data.  Our method approximates fine-tuning as a gradual shift in the model's learned distribution---from the original pretrained DM toward the fine-tuning data. By extrapolating the models before and after fine-tuning, we guide the generation toward high-probability regions within the fine-tuned data distribution. We then apply a clustering algorithm to extract the most probable images from those generated using this extrapolated guidance. Experiments on DMs fine-tuned with datasets such as WikiArt, DreamBooth, and real-world checkpoints posted online validate the effectiveness of our method, extracting approximately 20% of fine-tuning data in most cases, significantly surpassing baseline performance. The code is available.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：个性化扩散模型（如 DreamBooth 微调）在社区（Civitai、HuggingFace）中广泛共享检查点，存在训练数据泄露风险；同时，若微调使用了未经授权的数据，则可能引发版权侵权问题。
- **核心问题**：能否从这些共享的微调扩散模型中提取出训练数据？成功提取不仅揭示隐私泄露威胁，还能为版权侵权提供具体证据。
- **整体含义**：该研究验证了个性化扩散模型在共享检查点时的安全隐患，为模型安全评估和版权保护提供了工具。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将微调视为模型学习分布从预训练分布向微调数据分布的逐渐偏移。通过外推微调前后的模型，引导生成过程朝向微调数据分布的高概率区域。
- **关键技术细节**：
  - **模型引导**：利用微调前后的模型差异（梯度或隐空间偏移）来修正生成方向，使得生成样本更接近训练数据。
  - **生成与聚类**：基于外推引导生成大量候选图像，然后应用聚类算法（如 K-means）筛选出最可能的训练图像。
- **算法流程（文字说明）**：
  1. 获取预训练模型和微调后模型的检查点。
  2. 通过某种方式（如梯度计算或隐码空间插值）估计微调带来的分布偏移方向。
  3. 在反向去噪过程中，利用该偏移引导采样，使生成图像偏向训练数据。
  4. 生成大量候选图像，使用聚类算法（基于图像特征）提取簇中心或最可能属于训练集的原像。

## 3. 实验设计
- **数据集与场景**：
  - WikiArt 数据集（风格微调）
  - DreamBooth 数据集（对象微调）
  - 真实世界网上发布的检查点（来自社区）
- **Benchmark**：未明确说明标准基准，但对比了随机生成、无引导生成等基线方法。
- **对比方法**：论文未列举具体对比方法名称，但提到“显著超越基线表现”（baseline performance），可能包括直接采样、随机搜索等简单方法。

## 4. 资源与算力
- 论文中**未明确说明**使用的 GPU 型号、数量或训练时长。仅在代码开源部分提及“Code is available”，未提供具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：进行了多组实验，涵盖三种不同场景（WikiArt、DreamBooth、真实社区检查点）。每类场景可能包含多个微调模型和不同参数设置。
- **充分性**：实验覆盖了代表性数据集和实际场景，验证了方法的有效性和泛化能力。但缺乏消融实验的详细描述（如对外推程度、聚类方法的影响），也未报告统计显著性或多次重复结果。总体而言，实验设计较为扎实，但细节可更丰富。

## 6. 主要结论与发现
- 方法（FineXtract）能够成功从多种个性化扩散模型中提取出约 20% 的微调训练数据，显著优于基线。
- 证明了个性化扩散模型共享检查点存在真实的数据泄露威胁，且可被利用作为版权侵权的证据。
- 提示模型所有者需加强安全措施（如差分隐私、模型蒸馏等）。

## 7. 优点
- **方法创新**：首次针对个性化扩散模型的训练数据提取提出系统框架，利用模型分布偏移引导生成，思路新颖。
- **实用性**：方法在多种真实微调场景（包括社区实际检查点）中均有效，具有直接应用价值。
- **开源代码**：便于复现和后续研究。

## 8. 不足与局限
- **算力信息缺失**：未报告计算资源需求，难以评估方法在实际部署中的代价。
- **提取率有限**：约 20% 的提取率意味着大多数训练数据仍难以恢复，可能依赖于数据特征（如图像独特性）。
- **消融实验不足**：未详细分析不同组件（如引导强度、聚类参数）的影响，调优过程不透明。
- **基线对比模糊**：未明确列出对比方法的具体实现，公平性验证需进一步确认。
- **应用限制**：仅针对微调模型，未考虑使用差分隐私保护或数据增强等防御措施后的提取难度。
- **伦理风险**：方法可被恶意使用，论文未提供相应的负责任的披露或使用限制建议。

（完）
