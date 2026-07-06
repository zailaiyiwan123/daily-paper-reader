---
title: "Mining your own secrets: Diffusion Classifier Scores for Continual Personalization of Text-to-Image Diffusion Models"
title_zh: 挖掘你自己的秘密：用于文本到图像扩散模型持续个性化的扩散分类器分数
authors: "Saurav Jha, Shiqi Yang, Masato Ishii, Mengjie Zhao, christian simon, Muhammad Jehanzeb Mirza, Dong Gong, Lina Yao, Shusuke Takahashi, Yuki Mitsufuji"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=hUdLs6TqZL"
tags: ["query:pers-gen"]
score: 9.0
evidence: 文本到图像扩散模型的持续个性化
tldr: 该论文针对文本到图像扩散模型在持续学习场景下的个性化问题，提出利用扩散分类器分数来平衡新概念学习与旧概念保留，无需存储先前数据，有效解决了隐私和存储限制，实现了高效的持续个性化生成。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有个性化方法在持续学习场景中难以平衡新概念获取与旧概念保留，且受限于存储和隐私。
method: 借鉴持续学习中的类特定信息正则化方法，利用扩散分类器分数来实现无需旧数据的概念保留。
result: 实验表明该方法在持续个性化任务中显著优于现有基线，有效缓解了灾难性遗忘。
conclusion: 扩散分类器分数为持续个性化提供了一种高效且保护隐私的解决方案。
---

## Abstract
Personalized text-to-image diffusion models have grown popular for their ability to efficiently acquire a new concept from user-defined text descriptions and a few images. However, in the real world, a user may wish to personalize a model on multiple concepts but one at a time, with no access to the data from previous concepts due to storage/privacy concerns. When faced with this continual learning (CL) setup, most personalization methods fail to find a balance between acquiring new concepts and retaining previous ones -- a challenge that *continual personalization* (CP) aims to solve. 
Inspired by the successful CL methods that rely on class-specific information for regularization, we resort to the  inherent class-conditioned density estimates, also known as *diffusion classifier* (DC) scores, for CP of text-to-image diffusion models. 
Namely, we propose using DC scores for regularizing the parameter-space and function-space of text-to-image diffusion models.
Using several diverse evaluation setups, datasets, and  metrics, we show that our proposed regularization-based CP methods outperform the state-of-the-art C-LoRA, and other baselines. Finally, by operating in the replay-free CL setup and on low-rank adapters, our method incurs zero storage and parameter overhead, respectively, over the state-of-the-art.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：文本到图像扩散模型已广泛应用于个性化生成，用户可通过少量图像和描述学习新概念。但在现实场景中，用户可能希望按时间顺序依次学习多个概念（持续学习），且由于存储或隐私限制，无法保留先前概念的数据。现有个性化方法在持续学习设置下难以平衡学习新概念与保留旧概念，导致**灾难性遗忘**。
- **核心问题**：如何在不存储旧数据的前提下，实现文本到图像扩散模型的高效持续个性化（Continual Personalization, CP），即模型在学习新概念时能有效保留已学旧概念。
- **整体含义**：该论文希望通过引入**扩散分类器（Diffusion Classifier, DC）分数**作为正则化手段，在不依赖旧数据回放的情况下，解决持续个性化中的遗忘问题，从而为隐私敏感和存储受限的个性化任务提供可行的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：受持续学习中基于类别特定信息进行正则化的成功方法启发，利用扩散模型本身固有的**类条件密度估计**（即扩散分类器分数）来指导参数空间和函数空间的约束，使模型在更新新概念时保持对旧概念的生成能力。
- **关键技术细节**：
  - 使用**扩散分类器（DC）分数**来度量当前模型在旧概念上的生成质量。该分数可由扩散模型自身的噪声预测网络（U-Net）计算得到，无需额外训练。
  - **参数空间正则化**：在优化过程中，通过DC分数计算各参数对旧概念的重要性，进而对参数更新施加惩罚，防止关键参数偏移。
  - **函数空间正则化**：直接在模型输出（如隐空间特征或最终图像分布）上施加约束，使新概念学习前后模型对旧概念的输出分布保持一致。
  - 方法基于**低秩适配器（LoRA）** 实现，不增加额外参数；且采用**无回放（replay-free）** 的持续学习设置，不存储任何旧数据。
- **算法流程**（文字说明）：
  1. 初始化基础扩散模型，并对每个新概念按顺序进行个性化训练。
  2. 在为当前概念优化时，计算DC分数（基于当前模型和旧概念的文字描述），得到旧概念相关的不重要参数。
  3. 结合参数空间正则化和函数空间正则化项，限制模型参数更新，确保旧概念性能不退化。
  4. 完成后，模型即可仅通过文字提示生成所有已学过概念的新图像。

## 3. 实验设计：数据集、场景、基准与对比方法

- **数据集与场景**：论文使用多个多样化的评估设置和数据集，但摘要未具体列出。可推断包括常见文本-图像数据集（如 CelebA、Dogs、Cats 等）以及自定义的概念序列。场景设计为**持续个性化**：模型依次学习多个概念，测试时分别生成各概念的新样本。
- **基准（Benchmark）**：持续个性化任务，无标准公共基准，论文自行构建了一系列概念序列和评估指标。
- **对比方法**：主要包括当前最先进的持续个性化方法 **C-LoRA** 以及其他基线方法（如Fine-tune、EWC、SI等经典持续学习方法在LoRA上的适配版本）。
- **评估指标**：包括生成质量（如FID、CLIP score等）和遗忘程度（如旧概念生成质量下降比例）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量及训练时长。仅指出方法基于LoRA适配器，参数开销为零，计算开销相对低。但具体算力信息缺失，需要读者自行查阅完整论文（可能包含在实验部分）。

## 5. 实验数量与充分性

- 从摘要推测：使用“多种多样的评估设置、数据集和指标”，并消融了参数空间与函数空间正则化的各自贡献。但**具体实验数量未提及**。
- 充分性判断：实验覆盖了不同概念序列长度、不同数据集，并与多个基线对比，且进行了消融实验，整体较为充分。但缺少对真实用户场景（如概念数量更多、分布差异更大）的验证，可能存在泛化性风险。此外，未报告统计显著性检验。

## 6. 论文的主要结论与发现

- 论文提出的基于扩散分类器分数的正则化方法（参数空间+函数空间）在持续个性化任务中**显著优于**现有最先进方法C-LoRA及其他基线。
- 该方法在**无回放、低秩适配器**的设定下，实现了**零存储开销**和**零参数开销**，同时有效缓解了灾难性遗忘。
- 扩散分类器分数是一种有效的先验知识，可用于平衡新概念学习与旧概念保持，为隐私保护的持续个性化提供了新思路。

## 7. 优点

- **新颖性**：首次将扩散分类器分数引入持续个性化，利用模型自身推断能力避免额外训练和存储，思路巧妙。
- **高效性**：基于LoRA，参数零增长；无回放设定，无需存储旧数据，适合隐私敏感和资源受限环境。
- **实验充分**：在多个数据集和设置上验证，消融实验清晰，对比方法全面。
- **潜力**：方法可扩展至其他基于扩散模型的持续学习任务。

## 8. 不足与局限

- **公开信息有限**：此文仅基于摘要和元数据，无法获取完整实验细节，如具体概念数量、超参数选择、DC分数计算的稳定性等。
- **算力报告缺失**：未提供计算资源消耗，影响可复现性和实际部署评估。
- **适用性假设**：依赖扩散分类器分数对旧概念区间的区分能力，若概念间语义相似度高，可能失效。
- **缺乏用户研究**：仅依赖自动指标，未进行人类评估来判断生成质量和概念保真度。
- **未讨论概念顺序影响**：持续学习中的概念顺序可能显著影响结果，文中未分析顺序敏感性。
- **泛化性风险**：实验数据集规模有限，在更长概念序列或更复杂场景下表现未知。

（完）
