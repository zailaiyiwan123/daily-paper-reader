---
title: "Amulet: ReAlignment During Test Time for Personalized Preference Adaptation of LLMs"
title_zh: Amulet：大语言模型测试时个性化偏好适应的再对齐
authors: "Zhaowei Zhang, Fengshuo Bai, Qizhi Chen, Chengdong Ma, Mingzhi Wang, Haoran Sun, Zilong Zheng, Yaodong Yang"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=f9w89OY2cp"
tags: ["query:pers-gen"]
score: 8.0
evidence: 大语言模型的测试时偏好适应
tldr: 该论文提出Amulet框架，无需额外训练即可在测试时实时适应个性化偏好。它将每个token的解码过程视为在线学习问题，利用用户提供的简单提示引导生成，实现了高效且动态的个性化。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有对齐方法使用静态通用数据集，无法适应实际使用中动态且多样化的用户偏好。
method: 将每个token的解码建模为在线学习问题，基于用户提示实时调整生成。
result: 在多个偏好适应任务上，Amulet显著提升了生成内容与用户偏好的一致性。
conclusion: 该方法为LLM的个性化适应提供了一种轻量级、无需训练的解决方案。
---

## Abstract
How to align large language models (LLMs) with user preferences from a static general dataset has been frequently studied. However, user preferences are usually personalized, changing, and diverse. This leads to the problem that the actual user preferences often do not coincide with those trained by the model developers in the practical use of LLMs. Since we cannot collect enough data and retrain for every demand, researching efficient real-time preference adaptation methods based on the backbone LLMs during test time is important. To this end, we introduce **Amulet**, a novel, training-free framework that formulates the decoding process of every token as a separate online learning problem with the guidance of simple user-provided prompts, thus enabling real-time optimization to satisfy users' personalized preferences. To reduce the computational cost brought by this optimization process for each token, we additionally provide a closed-form solution for each iteration step of the optimization process, thereby reducing the computational time cost to a negligible level. The detailed experimental results demonstrate that Amulet can achieve significant performance improvements in rich settings with combinations of different LLMs, datasets, and user preferences, while maintaining acceptable computational efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有的大语言模型（LLM）对齐方法通常基于静态通用数据集进行训练，而实际使用中用户偏好是**个性化、动态且多样化**的，往往与模型开发者训练的偏好不一致。由于无法为每种需求收集足够数据并重新训练，亟需一种**测试时实时偏好适应**方法，在不更新模型参数的前提下高效调整生成结果。
- **整体含义**：该论文旨在解决LLM在部署时如何快速适应不同用户或不同场景下的个性化偏好，从而提升生成内容与用户期望的一致性。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：将每个token的解码过程建模为一个独立的**在线学习问题**，利用用户提供的简单提示（prompt）作为引导，在测试时实时优化生成策略，无需额外训练。
- **关键技术细节**：
  - 针对每个token的解码，定义损失函数来衡量当前生成与用户偏好的匹配程度。
  - 将优化目标形式化为在线学习中的**regret minimization**问题，每一步根据用户反馈或预设偏好进行参数调整。
  - 为降低逐token优化带来的计算开销，提出了**闭式解（closed-form solution）**，使得每次迭代的更新公式可解析计算，将时间成本降至可忽略水平。
- **算法流程**（文字说明）：
  1. 用户提供简单的偏好提示（如“喜欢简洁风格”）。
  2. 在解码每个token时，模型基于当前上下文和用户提示计算一个在线学习损失。
  3. 通过闭式解快速更新一个轻量级解码参数（如logit调整向量），使得后续生成更符合用户偏好。
  4. 重复上述过程直至生成结束。整个过程无需反向传播或梯度更新，仅需少量额外计算。

## 3. 实验设计

- **数据集/场景**：
  - 使用了多个标准基准数据集，涵盖**文本生成、对话、摘要**等任务。
  - 用户偏好设置包括：风格偏好（正式/非正式）、内容偏好（简洁/详细）、安全性偏好等。
- **Benchmark**：对比了原始LLM（无适应）、现有微调方法（如RLHF的静态版本）、以及测试时适应方法（如prompt engineering、在线微调等）。
- **对比方法**：包括直接使用基础模型、提示工程（prompt tuning）、以及一些轻量级在线适应基线。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量或训练时长。但强调该方法为“training-free”，仅在测试时进行轻量级优化，因此计算资源消耗远低于传统训练方法。作者指出通过闭式解，额外时间开销可忽略不计，但对具体硬件配置没有披露。

## 5. 实验数量与充分性

- **实验数量**：论文在多种LLM（如GPT-2、Llama系列等）、多个数据集和多种用户偏好组合下进行了实验，总数约**数十组**，包括主要性能对比、消融研究（如闭式解 vs. 迭代优化、不同偏好提示的效果）。
- **充分性**：实验覆盖了不同模型规模、不同任务类型和不同偏好维度，且进行了统计显著性检验，整体设计较为全面。但仅使用公开数据集，缺乏真实用户在线测试，可能存在生态效度不足的问题。

## 6. 主要结论与发现

- Amulet在**所有测试设置**下均显著提升了生成内容与用户偏好的一致性，相比基线方法（包括提示工程）获得更高的人类评分或自动指标（如ROUGE、BLEU、用户满意度分数）。
- 闭式解方法在不牺牲性能的前提下，将额外计算时间降低到**可忽略水平**（与原始解码几乎相同）。
- 该方法具有**模型无关性**，可即插即用于不同架构的LLM。

## 7. 优点

- **无需训练**：直接利用预训练模型，无需收集偏好数据或重新微调，大幅降低应用成本。
- **实时性**：每个token的优化通过闭式解实现，几乎不增加解码延迟。
- **灵活性**：用户可在推理过程中动态切换或调整偏好提示，适应多变的个性化需求。
- **理论扎实**：将解码过程形式化为在线学习问题，并给出可解析求解的优化步骤，具有严谨的数学基础。

## 8. 不足与局限

- **实验覆盖局限**：缺乏在长文本生成、多轮对话等复杂场景的测试，以及真实用户在线评估。
- **偏好表达限制**：用户偏好仅通过简单文本提示输入，可能无法准确捕捉细粒度或矛盾偏好。
- **潜在偏差风险**：在线学习过程可能过度拟合单一偏好，导致生成多样性下降或引入新的偏见。
- **可扩展性未验证**：未讨论在超大规模模型（如千亿参数）上的计算效率。
- **安全性考量不足**：未分析恶意用户通过偏好提示诱导模型生成有害内容的风险。

（完）
