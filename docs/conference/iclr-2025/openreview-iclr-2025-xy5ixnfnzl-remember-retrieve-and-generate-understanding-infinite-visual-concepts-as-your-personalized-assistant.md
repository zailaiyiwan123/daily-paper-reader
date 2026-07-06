---
title: "Remember, Retrieve and Generate: Understanding Infinite Visual Concepts as Your Personalized Assistant"
title_zh: 记住、检索与生成：作为个性化助手理解无限视觉概念
authors: "Haoran Hao, Jiaming Han, Changsheng Li, Yu-Feng Li, Xiangyu Yue"
date: 2024-09-23
pdf: "https://openreview.net/pdf?id=Xy5iXnFNzL"
tags: ["query:pers-gen"]
score: 9.0
evidence: 检索增强的多模态大语言模型个性化
tldr: 多模态大语言模型缺乏用户特定知识，本文提出RAP框架，通过构建键值数据库存储用户信息、多模态检索器检索相关概念，并结合生成步骤实现个性化助手。该框架无需额外训练即可在对话中利用无限视觉概念。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: MLLM作为通用助手缺乏用户特定知识，限制日常应用。
method: 设计RAP框架，包含记忆、检索和生成三步，使用键值数据库和多模态检索器。
result: 无需训练即可在对话中利用用户特定概念，提升个性化程度。
conclusion: RAP有效增强MLLM的个性化能力，具有实际部署价值。
---

## Abstract
The development of large language models (LLMs) has significantly enhanced the capabilities of multimodal LLMs (MLLMs) as general assistants. However, lack of user-specific knowledge still restricts their application in human's daily life. In this paper, we introduce the **R**etrieval **A**ugmented **P**ersonalization (RAP) framework for MLLMs' personalization. Starting from a general MLLM, we turn it into a personalized assistant in three steps. (a) Remember: We design a key-value database to store user-related information, *e.g.*, user's name, avatar and other attributes. (b) Retrieve: When the user initiates a conversation, RAP will retrieve relevant information from the database using a multimodal retriever. (c) Generate: The input query and retrieved concepts' information are fed into MLLMs to generate personalized, knowledge-augmented responses. Unlike previous methods, RAP allows real-time concept editing via updating the external database. To further improve generation quality and alignment with user-specific information, we design a pipeline for data collection and create a specialized dataset for personalized training of MLLMs. Based on the dataset, we train a series of MLLMs as personalized multimodal assistants. By pretraining on large-scale dataset, RAP-MLLMs can generalize to infinite visual concepts without additional finetuning. Our models demonstrate outstanding flexibility and generation quality across a variety of tasks, such as personalized image captioning, question answering and visual recognition. The code, data and models will be available.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：多模态大语言模型（MLLMs）作为通用助手已取得显著进步，但缺乏用户特定知识（如用户姓名、头像、属性等），限制了其在日常生活中的个性化应用。
- **核心问题**：如何在不重新训练模型的前提下，让MLLMs能够理解并利用无限视觉概念，成为真正个性化的助手。
- **整体含义**：本文提出一种检索增强的个性化框架（RAP），通过记忆、检索和生成三步，将通用MLLM转化为可实时编辑知识的个性化助手，显著提升人机交互的定制化程度。

## 2. 方法论

- **核心思想**：采用“记忆—检索—生成”三步法，借助外部键值数据库存储用户信息，并通过多模态检索器在对话时获取相关概念，无需额外训练或微调即可实现个性化。
- **关键技术细节**：
  - **记忆（Remember）**：设计键值数据库，存储用户相关信息（如姓名、头像、其他属性），支持实时编辑（增删改）。
  - **检索（Retrieve）**：用户发起对话时，利用多模态检索器从数据库中检索与当前查询相关的概念信息。
  - **生成（Generate）**：将用户输入和检索到的概念信息输入MLLMs，生成个性化、知识增强的回复。
- **训练流程**：构建专门的数据集，对MLLMs进行大规模预训练，使其能够泛化到无限视觉概念，无需额外微调即可应用于新用户/新概念。

## 3. 实验设计

- **数据集/场景**：文中提及“personalized image captioning, question answering and visual recognition”等多种任务，但未明确列出具体数据集名称或规模。
- **Benchmark**：未明确说明使用的标准基准（如MSCOCO、VQA等），仅提及在多种个性化任务上评估。
- **对比方法**：文中提到“Unlike previous methods”，但未列出具体对比模型名称或基线。根据元数据，该方法属于检索增强的多模态大语言模型个性化方向，可能对比了未使用数据库的MLLM或传统微调个性化方法。

## 4. 资源与算力

- 文中**未明确说明**训练或推理所使用的GPU型号、数量、训练时长等资源信息。
- 仅提到“pretraining on large-scale dataset”以及“train a series of MLLMs”，但未提供具体算力需求。

## 5. 实验数量与充分性

- **实验数量**：文中仅概括性提及在“a variety of tasks”上展示了灵活性和生成质量，未给出具体实验个数、消融实验设置或详细结果表格。
- **充分性评估**：由于缺乏定量结果和详细实验设计描述，难以判断实验的充分性和客观公平性。文中未报告与基线方法的性能比较或统计指标（如准确率、F1、用户评分等），实验部分显得不够充分。

## 6. 主要结论与发现

- RAP框架无需额外训练即可使MLLMs利用用户特定知识，实现实时概念编辑。
- 通过大规模预训练，RAP-MLLMs能够泛化到无限视觉概念，在个性化图像描述、问答、视觉识别等任务中表现出良好的灵活性和生成质量。
- 该框架有助于将通用MLLM转化为实际可部署的个性化助手。

## 7. 优点

- **无需训练即可个性化**：通过外部数据库和检索机制，避免了对每个用户重新微调的昂贵成本。
- **实时概念编辑**：支持动态更新用户信息，适应快速变化的需求。
- **泛化能力强**：大规模预训练后可直接处理未见过的视觉概念。
- **框架简洁**：记忆-检索-生成三步骤设计清晰，易于集成到现有MLLM系统中。

## 8. 不足与局限

- **实验覆盖不足**：未提供具体数据集、对比基线、定量指标，难以验证方法的实际性能优势。
- **偏差风险**：数据库的构建质量、检索器的准确性可能影响个性化结果；若用户信息存在偏见，可能被放大。
- **应用限制**：需要预先构建并维护用户知识数据库，对大用户群体可能带来存储和检索开销。
- **缺乏算力细节**：无法评估方法在资源受限场景下的部署可行性。
- **未讨论失败案例**：未分析检索失败或生成错误时的应对策略。

（完）
