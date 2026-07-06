---
title: Automated Black-box Prompt Engineering for Personalized Text-to-Image Generation
title_zh: 个性化文本到图像生成的自动化黑盒提示工程
authors: "Yutong He, Alexander Robey, Naoki Murata, Yiding Jiang, Joshua Nathaniel Williams, George J. Pappas, Hamed Hassani, Yuki Mitsufuji, Russ Salakhutdinov, J Zico Kolter"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=hIKsem01M5"
tags: ["query:pers-gen"]
score: 9.0
evidence: 黑盒条件下个性化文本到图像生成的自动化提示工程
tldr: 提示工程个性化文本到图像生成既有效但费力。本文提出PRISM算法，利用大语言模型的上下文学习能力，自动优化可解释且可迁移的提示。算法在仅黑盒访问T2I模型的情况下，迭代优化提示内容，生成符合用户期望的概念。该方法避免了白盒需求和非直观提示，大幅降低人工成本。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 手动提示工程费时，现有自动方法需要白盒访问或生成不直观提示。
method: 利用LLM的上下文学习能力在黑盒下迭代优化提示。
result: 生成可解释、可迁移的提示，有效生成个性化概念。
conclusion: 为个性化T2I生成提供了自动化、用户友好的提示工程方案。
---

## Abstract
Prompt engineering is an effective but labor-intensive way to control text-to-image (T2I) generative models. Its time-intensive nature and complexity have spurred the development of algorithms for automated prompt generation. However, these methods often struggle with transferability across T2I models, require white-box access to the underlying model, or produce non-intuitive prompts. In this work, we introduce PRISM, an algorithm that automatically produces human-interpretable and transferable prompts that can effectively generate desired concepts given only black-box access to T2I models. Inspired by large language model (LLM) jailbreaking, PRISM leverages the in-context learning ability of LLMs to iteratively refine the candidate prompt distribution built upon the reference images. Our experiments demonstrate the versatility and effectiveness of PRISM in generating accurate prompts for objects, styles, and images across multiple T2I models, including Stable Diffusion, DALL-E, and Midjourney.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：文本到图像生成（T2I）模型的控制依赖手动提示工程，该过程虽有效但费时费力，且现有自动化提示生成方法存在局限性：要么需要白盒访问底层模型，要么生成不直观、难以解释的提示，要么缺乏跨模型的可迁移性。
- **研究动机**：为个性化T2I生成提供一种自动化、用户友好且高效的提示工程方案，降低人工成本，同时保持提示的可解释性和跨模型迁移能力。
- **整体含义**：本文提出PRISM算法，利用大语言模型（LLM）的上下文学习能力，在仅黑盒访问T2I模型的情况下，自动优化可解释且可迁移的提示，能够有效生成用户指定的概念（物体、风格、图像等）。

## 2. 论文提出的方法论

- **核心思想**：受LLM越狱（jailbreaking）技术启发，PRISM通过迭代优化候选提示分布，让LLM基于参考图像和当前提示的反馈，自动生成和改进提示文本。
- **关键技术细节**：
  - 仅需黑盒访问T2I模型（如Stable Diffusion、DALL-E、Midjourney），无需模型内部信息或梯度。
  - 利用LLM的上下文学习能力，将参考图像、当前提示以及生成的图像质量反馈作为上下文，指导LLM生成新的候选提示。
  - 迭代过程：初始提示集合→用T2I模型生成图像→评估生成图像与参考图像的匹配程度（通过某种相似度指标）→LLM根据评估结果调整提示分布→生成下一轮候选提示。
  - 最终输出一个可解释、可迁移的文本提示，能够复现参考图像中的概念。
- **公式/算法流程**（文字说明）：
  1. 输入：用户提供参考图像集（描述同一概念）。
  2. 初始化：使用LLM生成一批候选提示（例如若干描述性短语）。
  3. 循环迭代：
     - 对每个候选提示，使用目标T2I模型（黑盒）生成图像。
     - 计算生成图像与参考图像之间的语义相似度（如CLIP score或人工反馈）。
     - 将相似度得分作为反馈，连同参考图像和生成图像（或特征）输入LLM。
     - LLM根据上下文学习，更新候选提示分布，生成新的候选提示。
  4. 收敛后，选择最优提示作为输出。
- 注意：由于原文只有摘要，具体细节（如相似度度量、迭代次数、LLM选择）未展开，以上为基于摘要和元数据的合理推断。

## 3. 实验设计

- **数据集/场景**：未明确列出具体数据集，但文中提到在物体（objects）、风格（styles）、图像（images）三种个性化概念上进行了测试。可能使用了多种参考图像集（如常见物体、艺术风格、特定照片）。
- **Benchmark**：没有明确对比基准，但隐式对比了现有自动化提示生成方法（需要白盒或生成非直观提示）和手动提示工程。
- **对比方法**：未具体命名，但提到现有方法“struggle with transferability”、“require white-box access”、“produce non-intuitive prompts”。可能对比了如Prompt-to-Prompt、Textual Inversion、DreamBooth等需要白盒或微调的方法，以及手动提示。
- **评估指标**：未明确，可能包括生成图像与参考图像的相似度（如CLIP score）、用户评估、可解释性评分、跨模型迁移成功率等。

## 4. 资源与算力

- 文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提到PRISM在黑盒条件下运行，不需要训练模型参数，但需要调用T2I模型和LLM进行推理。因此算力消耗取决于T2I模型和LLM的推理成本。
- 元数据中也没有提及算力细节。

## 5. 实验数量与充分性

- 根据摘要，实验覆盖了物体、风格、图像三种类型的概念，并在多个T2I模型（Stable Diffusion、DALL-E、Midjourney）上验证。但未提及具体实验组数、消融实验（如不同LLM选择、迭代次数影响、初始化策略）。
- **充分性评价**：由于仅公开了摘要，无法判断实验是否充分。从元数据“score: 9.0”和论文集（ICLR-2025-Rejected-Public）看，该论文可能被拒稿，实验不足可能是一方面原因。但摘要声称“versatility and effectiveness”，需要进一步查看全文才能判断。
- **客观公平性**：未提供详细的对比方法设置、随机种子、统计显著性等，因此难以评估。

## 6. 论文的主要结论与发现

- PRISM算法能够自动生成人类可解释（human-interpretable）且可迁移（transferable）的提示，有效生成用户期望的个性化概念，仅需黑盒访问T2I模型。
- 该方法在多个T2I模型和多种概念（物体、风格、图像）上均表现有效，避免了白盒访问和非直观提示的缺陷，大幅降低人工成本。
- 利用LLM的上下文学习能力迭代优化提示分布是可行的自动化提示工程方案。

## 7. 优点

- **黑盒性**：仅需黑盒访问，适用于无法获得模型内部信息的场景（如商业API）。
- **可解释性**：生成的提示是自然语言文本，用户可以理解并进一步调整。
- **可迁移性**：同一提示可直接用于不同T2I模型，无需重新优化。
- **自动化**：完全自动化，无需人工反复试错。
- **灵感新颖**：借鉴LLM越狱技术的思路，将提示优化视为迭代搜索问题。

## 8. 不足与局限

- **实验细节缺失**：摘要中未提供足够的实验设置、数据集、对比方法、消融研究，难以评估实际性能的可靠性和泛化能力。
- **可能依赖高质量的LLM与反馈信号**：LLM的上下文学习能力是关键，若LLM能力不足或反馈信号不准确，可能效果下降。
- **收敛性未讨论**：迭代过程的收敛性和稳定性未知，可能陷入局部最优。
- **计算成本**：虽然无需训练，但重复调用LLM和T2I模型可能产生较高的推理成本（尤其对大规模部署）。
- **应用限制**：仅针对个性化概念生成，对于复杂场景或需要精确控制的生成可能效果有限。
- **缺乏与其他自动化方法的直接对比**：未列出具体数字指标，说服力不足。

（完）
