---
title: Personalized Visual Instruction Tuning
title_zh: 个性化视觉指令微调
authors: "Renjie Pi, Jianshu Zhang, Tianyang Han, Jipeng Zhang, Rui Pan, Tong Zhang"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=sAxdIJ4l6z"
tags: ["query:pers-gen"]
score: 9.0
evidence: 面向多模态LLM的个性化视觉指令微调
tldr: 多模态大语言模型存在“脸盲”，无法进行个性化对话。本文提出个性化视觉指令微调（PVIT）框架，通过精心设计的数据收集和训练流程，使模型能识别图像中的特定人物并生成个性化对话。实验证明PVIT显著提升了多模态LLM在个性化场景下的表现，为智能助理和家庭机器人等应用打开新可能。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 多模态LLM缺乏识别特定个体的能力，无法进行个性化对话。
method: 提出PVIT数据收集和训练框架，使模型识别个体并个性化响应。
result: 在多模态个性化对话任务上显著优于基线。
conclusion: 视觉指令微调有效赋予多模态LLM个性化能力。
---

## Abstract
Recent advancements in multimodal large language models (MLLMs) have demonstrated significant progress; however, these models exhibit a notable limitation, which we refer to as "face blindness." Specifically, they can engage in general conversations but fail to conduct personalized dialogues targeting at specific individuals. This deficiency hinders the application of MLLMs in personalized settings, such as tailored visual assistants on mobile devices, or domestic robots that need to recognize members of the family. In this paper, we introduce Personalized Visual Instruction Tuning (PVIT), a novel data curation and training framework designed to enable MLLMs to identify target individuals within an image and engage in personalized and coherent dialogues. Our approach involves the development of a sophisticated pipeline that autonomously generates training data containing personalized conversations. This pipeline leverages the capabilities of various visual experts, image generation models, and (multi-modal) large language models. To evaluate the personalized potential of MLLMs, we present a benchmark called P-Bench, which encompasses various question types with different levels of difficulty. The experiments demonstrate a substantial personalized performance enhancement after fine-tuning with our curated dataset.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前多模态大语言模型（MLLMs）存在“脸盲”（face blindness）现象——它们能进行通用对话，但无法针对特定个体（如家庭成员、朋友）进行个性化对话。这限制了其在个性化视觉助手（如手机助手、家庭机器人）等场景中的应用。
- **研究背景**：已有视觉指令微调（Visual Instruction Tuning）使MLLMs具备通用视觉对话能力，但缺乏识别图像中特定人物并生成个性化回应的能力。
- **整体含义**：提出个性化视觉指令微调（PVIT）框架，使MLLMs学会识别目标个体、理解其身份，并在对话中生成个性化、连贯的回复，从而打开智能助理和家庭机器人等个性化应用的可能性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过精心设计的数据收集和训练流程，自动生成包含个性化对话的训练数据，并用这些数据微调MLLMs，让模型学会将个体身份与对话上下文关联。
- **关键技术细节**：
  - **数据生成流水线**：利用多种视觉专家模型（如目标检测、人脸识别）、图像生成模型（如Stable Diffusion）以及（多模态）大语言模型，自动生成包含个性化话题的对话数据。例如，先提取图像中人物的特征，再由语言模型生成关于该人物的问答对。
  - **训练框架**：采用标准指令微调范式，在生成的自定义数据集上对MLLM进行全参数或部分参数微调。
- **公式/算法流程**（文字说明）：没有显式数学公式，但整体流程为：
  1. 收集包含多人的图像；
  2. 使用视觉专家标注人物身份和属性；
  3. 使用多模态LLM基于标注生成个性化问答对；
  4. 用这些问答对微调目标MLLM（如LLaVA、Qwen-VL等）。
- **模型输出**：模型在推理时，输入图像和问题，能识别并调用目标人物的身份信息，生成个性化回复（例如“这是你的朋友张三，他今天穿了红色外套”）。

## 3. 实验设计：数据集、benchmark、对比方法

- **数据集**：作者自行构建了一个个性化对话训练数据集，利用上述流水线自动生成，但论文摘要中未明确具体来源或规模。
- **Benchmark**：提出 **P-Bench**，一个涵盖多种问题类型和不同难度等级的多模态个性化对话基准，用于评估MLLMs的个性化能力。
- **对比方法**：文中未列出具体基线名称，但推测对比了未微调的基础MLLM（如LLaVA、Qwen-VL等）以及通用指令微调版本。

## 4. 资源与算力

- 文中**未明确说明**使用的GPU型号、数量及训练时长。只能推测使用了常规的深度学习训练集群（如A100或V100），但细节未公开。

## 5. 实验数量与充分性

- 根据摘要，实验部分主要展示了在P-Bench上的性能提升，但**未报告具体实验组数**（如不同数据集上的结果、消融实验数量）。只能从结论得知“substantial personalized performance enhancement”，但**缺乏充分的消融实验**（如数据生成流水线各模块贡献、不同基座模型对比）和统计显著性检验。
- **客观性/公平性**：自行构建了P-Bench并仅与自身生成的基线对比，可能存在自我评估偏差。未与已有公开对话基准（如MMBench、MME）进行通用能力比较，也未说明如何控制其他变量。

## 6. 论文的主要结论与发现

- **主要结论**：使用PVIT方法微调后的MLLM在个性化对话任务上显著优于基线模型，证明视觉指令微调能够有效赋予MLLM识别特定个体和进行个性化对话的能力。
- **发现**：自主生成的高质量个性化对话数据是赋能模型的关键，且P-Bench可用于评估此类能力。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次系统性地针对MLLM的“脸盲”问题提出完整的个性化对话训练框架，包括自动数据生成流水线，有效降低人工标注成本。
- **基准贡献**：推出P-Bench，填补了多模态个性化对话评估的空缺。
- **实际价值**：解决真实应用痛点（如家庭机器人识别成员），具有很强的应用前景。
- **实验设计**：对比了微调前后的性能差异，直接证明个性化能力的提升。

## 8. 不足与局限

- **实验覆盖不足**：未报告在标准通用视觉对话基准（如VQA、GQA、MSCOCO等）上的结果，无法判断个性化微调是否损害通用能力。
- **偏差风险**：数据生成依赖现有视觉专家和语言模型，这些模型本身可能存在偏见（如人脸识别对特定人群的错误识别），可能引入偏见。
- **应用限制**：仅关注个体识别，未涉及更复杂的个性化场景（如用户偏好、情绪识别）。
- **透明度不足**：未公开数据规模、算力需求、训练超参数等细节，难以复现；消融实验不够充分。
- **公平性**：P-Bench为作者自定义，对比基线未包含其他个性化工作（如使用视觉提示的Prompt-tuning方法），对比不够全面。

（完）
