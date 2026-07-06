---
title: "A Comparison of LLM fine-tuning Methods & Evaluation Metrics with Travel Chatbot Use Case"
title_zh: 基于旅行聊天机器人的大语言模型微调方法与评估指标比较
authors: "Sonia Wei Meyer, Shreya Singh, Christopher Ton, Bertha Tam, Angel Ren"
date: 2025-01-09
pdf: "https://openreview.net/pdf?id=mNYsX2kcsl"
tags: ["query:pers-gen"]
score: 9.0
evidence: 使用个性化旅行数据比较LLM微调方法用于聊天机器人
tldr: 该论文以旅行聊天机器人为用例，系统比较了QLoRA、RAFT和RLHF等大语言模型微调方法，并使用从Reddit获取的个性化旅行经验数据集。评估方法包括E2E基准、传统NLP指标、Ragas、GPT-4评估和人工评估。研究为个性化对话生成提供了微调方法的选择指南，并强调了个性化数据的重要性。
source: ICML-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 需要比较不同LLM微调方法在个性化对话生成中的效果。
method: 使用个性化旅行数据集，应用QLoRA、RAFT和RLHF微调，并进行多种评估。
result: RLHF在个性化对话生成中表现最佳，但所有方法均受数据质量影响。
conclusion: 该比较为个性化生成系统的微调方法选择提供了实证依据。
---

## Abstract
This research compared large language model (LLM) fine-tuning methods, including Quantized Low Rank Adapter (QLoRA), Retrieval Augmented fine-tuning (RAFT), and Reinforcement Learning from Human Feedback (RLHF), and additionally compared LLM evaluation methods including End to End (E2E) benchmark method of “Golden Answers”, traditional natural language processing (NLP) metrics, RAG Assessment (Ragas), OpenAI GPT-4 evaluation metrics, and human evaluation, using the travel chatbot use case. The travel dataset was sourced from the Reddit API by requesting posts from travel-related subreddits to get conversation prompts and personalized travel experiences, and augmented for each fine-tuning method. QLoRA and RAFT were applied to two pre-trained LLMs: LLaMa 2 7B and Mistral  7B. The best model according to human evaluation and some GPT-4 metrics was Mistral RAFT, so this underwent a Reinforcement Learning from Human Feedback (RLHF) training pipeline, and ultimately was evaluated as the best model. Our main findings are that: 1) quantitative and Ragas metrics do not align with human evaluation, while Open AI GPT-4 evaluations do, 2) RAFT outperforms QLoRA, but still needs postprocessing, and 3) RLHF improves model performance significantly to outperform benchmark models.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）在个性化对话生成中的应用日益广泛，但不同微调方法（如QLoRA、RAFT、RLHF）的效果对比尚不明确，且评估指标（传统NLP指标、Ragas、GPT-4评估、人工评估等）的一致性有待验证。旅行聊天机器人作为个性化对话的典型场景，可提供真实、多样化的用户偏好数据。
- **整体含义**：以Reddit旅行帖子构建个性化数据集，系统比较三种微调方法，并评估多种评估指标的有效性，为个性化对话系统选择微调方法和评估策略提供实证依据。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：对比QLoRA（量化低秩适配）、RAFT（检索增强微调）和RLHF（基于人类反馈的强化学习）三种方法，并联合多种评估指标进行多维评估。
- **关键技术细节**：
  - **QLoRA**：对预训练LLM（LLaMA 2 7B和Mistral 7B）进行量化低秩适配微调，降低显存占用。
  - **RAFT**：结合检索增强与微调，先检索相关个性化旅行经验，再作为上下文注入模型进行微调。
  - **RLHF**：在QLoRA/RAFT微调基础上，使用人类偏好数据训练奖励模型，再通过PPO算法优化生成策略。
- **评估方法**：
  - **传统NLP指标**：BLEU、ROUGE、METEOR等。
  - **Ragas**：基于检索增强生成的评估框架。
  - **GPT-4评估**：利用OpenAI GPT-4对生成回复进行打分。
  - **人工评估**：人类评委对回复的个性化、相关性和流畅性进行评分。
  - **E2E基准**：基于“Golden Answers”的端到端基准方法。

## 3. 实验设计：数据集、基准方法与对比方法

- **数据集**：从Reddit旅行相关子版块（subreddits）爬取帖子，包含对话提示和个性化旅行经验，并针对每种微调方法进行数据增强（如构建检索库、生成偏好对比对等）。
- **基准（Benchmark）**：未特别说明外部标准基准，但以未微调的原始LLaMA 2 7B和Mistral 7B作为基线，同时使用“Golden Answers”（人工标注的理想回复）作为E2E基准。
- **对比方法**：
  - **微调方法**：QLoRA vs. RAFT vs. RLHF。
  - **基座模型**：LLaMA 2 7B vs. Mistral 7B。
  - **最终最佳模型**：基于人工评估和部分GPT-4指标，确定Mistral RAFT模型最优，随后对其进行RLHF训练，并与原始Mistral RAFT、QLoRA及其他基准对比。

## 4. 资源与算力

- **明确信息**：论文中未明确说明使用的GPU型号、数量、训练时长等具体算力资源。
- **备注**：根据常识，QLoRA可降低显存需求，通常可在单张消费级GPU（如A100 40GB）上训练7B模型；但本文未提供具体数字，属于实验复现时的缺失信息。

## 5. 实验数量与充分性

- **实验数量**：
  - 微调实验：对2个基座模型分别应用QLoRA和RAFT，共4组初始微调模型。
  - 在此基础上，对最佳模型（Mistral RAFT）进行RLHF训练，得到最终模型。
  - 评估实验：每组模型均使用多项评估指标（传统指标、Ragas、GPT-4、人工评估），涵盖多个维度。
- **充分性与客观性**：
  - **优点**：评估指标覆盖全面（自动+人工），人工评估提供了个性化质量的直接判断。
  - **不足**：仅使用一个数据集（旅行Reddit），且数据来源单一（Reddit帖子），可能引入领域偏差；未进行跨领域或跨数据集验证；消融实验不足（如未单独分析检索增强对RAFT的贡献）。实验设计基本公平（同一数据、同一基座模型比较），但未报告统计显著性检验。

## 6. 论文的主要结论与发现

1. **定量与Ragas指标与人工评估不一致**：传统NLP指标（BLEU等）和Ragas评分不能反映人类对个性化回复的真实偏好，而OpenAI GPT-4评估与人工评估较为一致。
2. **RAFT优于QLoRA**：在个性化对话生成中，检索增强微调（RAFT）比单纯量化适配（QLoRA）效果更好，但仍需后处理（postprocessing）来改进流畅性。
3. **RLHF显著提升性能**：在RAFT基座上应用RLHF后，模型生成回复的个性化、相关性和流畅性均超过所有基线模型，包括未微调的原始模型和仅使用QLoRA/RAFT的模型。

## 7. 优点：方法和实验设计中的亮点

- **系统性的微调方法比较**：同时对比了当前主流的三种微调范式（QLoRA、RAFT、RLHF），对实践有直接指导意义。
- **多维评估框架**：同时使用自动指标、Ragas、GPT-4评估和人工评估，揭示了不同评估间的差异，尤其指出GPT-4评估可作为人工评估的有效替代。
- **真实场景数据**：从Reddit旅行版块获取真实用户帖子，保证了个性化数据的真实性和多样性。
- **可复现性**：使用开源模型（LLaMA 2、Mistral）和公开数据源，便于后续研究复现。

## 8. 不足与局限

- **数据集单一**：仅使用旅行领域Reddit数据，缺乏跨领域验证（如餐饮、娱乐等），结论泛化性有限。
- **算力细节缺失**：未报告训练耗时、GPU型号/数量，影响成本评估和可复现性。
- **消融实验不足**：未单独分析检索增强、量化、RLHF各组件的影响；未对比不同RLHF超参数的影响。
- **评估偏差风险**：GPT-4评估虽然与人工一致，但GPT-4本身可能存在对自身生成风格偏好；人工评估样本量未知，可能受评分者一致性影响。
- **未涉及更先进方法**：如DPO（Direct Preference Optimization）、ORM（Outcome Reward Model）等新方法未被纳入比较。
- **应用限制**：模型大小仅限7B参数，对于更大规模（如70B）或更小规模（如3B）模型的效果未知；旅行聊天机器人的个性化需求可能与医疗、金融等领域存在差异。

（完）
