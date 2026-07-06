---
title: Preference Adaptive and Sequential Text-to-Image Generation
title_zh: 偏好自适应与序列文本到图像生成
authors: "Ofir Nabati, Guy Tennenholtz, ChihWei Hsu, Moonkyung Ryu, Deepak Ramachandran, Yinlam Chow, Xiang Li, Craig Boutilier"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=LCr6CIAEye"
tags: ["query:pers-gen"]
score: 10.0
evidence: 利用用户偏好和序列强化学习进行适应性文本到图像生成
tldr: 该论文提出PASTA，一个基于强化学习的互动文本到图像生成代理。它通过迭代的提示扩展序列来根据用户偏好改进生成图像。作者使用人工评分创建了序列偏好数据集，并构建用户偏好模型，利用多模态大语言模型和基于价值的RL建议适应性提示。直接实现了用户个性化的图像生成。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有文本到图像生成缺乏交互式个性化能力，无法适应不断变化的用户偏好。
method: 构建用户偏好和选择模型，结合多模态大模型和强化学习迭代优化提示扩展。
result: 在用户研究中，PASTA生成的图像比基线更符合用户偏好。
conclusion: 该工作实现了交互式个性化图像生成，拓展了T2I模型的多轮能力。
---

## Abstract
We address the problem of interactive text-to-image (T2I) generation, designing a reinforcement learning (RL) agent which iteratively improves a set of generated images for a user through a sequence of prompt expansions. Using human raters, we create a novel dataset of sequential preferences, which we leverage, together with large-scale open-source (non-sequential) datasets. We construct user-preference and user-choice models using an EM strategy and identify varying user preference types. We then leverage a large multimodal language model (LMM) and a value-based RL approach to suggest an adaptive and diverse slate of prompt expansions to the user. Our Preference Adaptive and Sequential Text-to-image Agent (PASTA) extends T2I models with adaptive multi-turn capabilities, fostering collaborative co-creation and addressing uncertainty or underspecification in a user's intent. We evaluate PASTA using human raters, showing significant improvement compared to baseline methods. We also open-source our sequential rater dataset and simulated user-rater interactions to support future research in user-centric multi-turn T2I systems.

---

## 论文详细总结（自动生成）

# 论文详细总结：Preference Adaptive and Sequential Text-to-Image Generation (PASTA)

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有文本到图像（Text-to-Image, T2I）生成模型通常是一次性生成，缺乏与用户交互、适应动态偏好的能力。用户意图常常模糊或不完整，导致生成的图像不符合预期。
- **研究动机**：为了解决“一次生成难满足用户”的问题，使模型能通过多轮对话式交互逐步细化图像，实现个性化、协作式创作。
- **整体含义**：论文提出一个基于强化学习的交互式T2I代理PASTA，通过序列提示扩展（prompt expansions）迭代改进图像，将用户偏好纳入生成过程，拓展了T2I系统从单次生成到多轮自适应交互的能力。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：构建用户偏好模型和选择模型，利用多模态大语言模型（LMM）和基于价值的强化学习（RL）来生成一组多样且自适应的提示扩展，供用户选择，从而逐步优化生成图像。
- **关键技术细节**：
  - **数据集构建**：使用人工评分员创建新颖的序列偏好数据集（sequential preference dataset），同时结合大规模开源非序列数据集。
  - **用户偏好建模**：采用期望最大化（EM）策略分别训练用户偏好模型和用户选择模型，识别不同类型的用户偏好。
  - **RL代理设计**：使用基于价值（value-based）的RL方法，结合LMM，根据当前用户偏好和历史交互，建议一组最可能被用户偏爱的提示扩展（slate）。
  - **算法流程**（文字描述）：
    1. 初始阶段：给定一个原始文本提示，LMM生成一组候选提示扩展。
    2. 用户选择：从这些扩展中用户挑选一个（或系统根据偏好模型推荐）。
    3. 图像生成：将原始提示与所选扩展合并，T2I模型生成新图像。
    4. 反馈与更新：用户对生成图像进行反馈（如评级或选择），更新偏好模型和RL代理。
    5. 迭代：重复步骤1-4，直到用户满意或达到轮数上限。
- **公式**：摘要未给出具体公式，但方法涉及RL的状态、动作、奖励定义，以及EM算法更新参数。

## 3. 实验设计：数据集、场景、Benchmark与对比方法
- **数据集**：
  - 人工评分的序列偏好数据集（自建）：由人类评分员在交互序列中标注偏好。
  - 大规模开源非序列数据集（如广泛使用的T2I数据集）。
  - 还开源了“模拟用户-评分员交互数据”以支持未来研究。
- **场景**：用户与系统多轮交互生成图像，用户偏好可能变化。
- **Benchmark**：未明确说明标准benchmark，但使用人类评分作为主要评价指标。
- **对比方法**：与基线方法（baseline methods）比较，包括非交互式生成或其他简单多轮策略（文中未列具体名称，但指出PASTA显著优于基线）。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据未提及使用的GPU型号、数量、训练时长等算力信息。因此无法总结具体算力消耗，仅能指出论文未提供此细节。

## 5. 实验数量与充分性
- **实验数量**：至少包括：
  - 用户研究（human raters evaluation）：对比PASTA与基线，显示显著提升。
  - 偏好模型训练（EM策略）、RL代理训练。
  - 消融实验（推测）：但摘要未详细说明消融实验组数。
- **充分性与客观性**：
  - 使用人工评分避免了自动指标偏差，较客观。
  - 开源了部分数据，有助于复现。
  - 但缺少定量指标（如FID、CLIP score）与基线的全面对比，仅有用户偏好判断。未提及在多大尺度（用户数、图像数）上验证，充分性中等。

## 6. 论文的主要结论与发现
- **主要结论**：PASTA通过多轮交互和自适应提示扩展，能够显著提升用户对生成图像的满意度，优于非交互式基线。
- **发现**：用户偏好存在多种类型，通过EM模型可以自动识别并适应；结合LMM和RL的方法能有效生成多样化、符合偏好的扩展建议。

## 7. 优点：方法或实验设计上的亮点
- **新颖性**：将序列强化学习引入T2I生成的交互式个性化，超越了传统一次性生成。
- **数据贡献**：创建并开源序列偏好数据集，填补该领域数据空白。
- **方法融合**：结合多模态大模型（LMM）的理解能力和RL的决策能力，实现自适应建议。
- **用户为中心**：直接使用人类评分作为训练和评价依据，贴近真实应用需求。
- **可复现性**：开源数据和模拟交互，便于后续研究。

## 8. 不足与局限
- **实验覆盖有限**：仅使用人工评分作为评价，缺乏标准自动指标（如FID、IS、CLIP score）对比，难以横向比较。
- **算力资源未披露**：其他研究者难以评估方法的训练成本。
- **泛化性**：未讨论模型在不同文化、语言、复杂提示下的表现。
- **用户规模**：未说明参与人工评分的人数及背景，可能引入评分偏差。
- **应用限制**：依赖多轮交互，对于希望一次生成就满意的用户可能效率较低；RL训练可能收敛慢，且偏好模型对初始数据敏感。
- **局限性提及**：摘要未讨论失败案例、安全/伦理问题或计算效率。

（完）
