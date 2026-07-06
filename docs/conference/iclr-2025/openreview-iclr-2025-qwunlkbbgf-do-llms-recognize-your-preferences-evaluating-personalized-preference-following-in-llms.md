---
title: Do LLMs Recognize Your Preferences? Evaluating Personalized Preference Following in LLMs
title_zh: LLM能识别你的偏好吗？评估LLM中的个性化偏好遵循能力
authors: "Siyan Zhao, Mingyi Hong, Yang Liu, Devamanyu Hazarika, Kaixiang Lin"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=QWunLKbBGF"
tags: ["query:pers-gen"]
score: 8.0
evidence: 评估LLM根据用户偏好进行个性化文本生成的基准
tldr: LLM作为聊天机器人时缺乏对用户偏好的个性化响应能力，本文提出PrefEval基准，包含3000对偏好-查询数据，评估LLM在长上下文对话中推断、记忆和遵循用户偏好的能力。实验揭示了现有模型在个性化文本生成中的不足。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: LLM在聊天中缺乏个性化响应能力，需标准化评估。
method: 构建PrefEval基准，包含显式和隐式偏好形式，采用生成和分类任务评估。
result: 10个开源和商业LLM在长达100k token的对话中表现有限。
conclusion: PrefEval为LLM个性化文本生成提供了重要评估工具。
---

## Abstract
Large Language Models (LLMs) are increasingly deployed as chatbots, yet their ability to personalize responses to user preferences remains limited. We introduce PrefEval, a benchmark for evaluating LLMs' ability to infer, memorize and adhere to user preferences in long-context conversational setting.
PrefEval comprises 3,000 manually curated user preference and query pairs spanning 20 topics. PrefEval contains user personalization or preference information in both explicit and implicit preference forms, and evaluates LLM performance using a generation and a classification task. With PrefEval, we have evaluated 10 open-sourced and
proprietary LLMs in multi-session conversations with varying context lengths up to 100k tokens. We benchmark with various prompting, iterative feedback, and retrieval-augmented generation methods. 
Our benchmarking effort reveals that state-of-the-art LLMs face significant challenges in following users' preference during conversations. In particular,  in zero-shot settings, preference following accuracy falls below 10\% at merely 10 turns (~3k tokens) across most evaluated models. Even with advanced prompting and retrieval methods, preference following still deteriorates in long-context conversations. Furthermore, we show that fine-tuning on PrefEval significantly improves performance. We believe PrefEval serves as a valuable resource for measuring, understanding, and enhancing LLMs' proactive preference following abilities, paving the way for personalized conversational agents.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文元数据与摘要，我将为您生成一份结构化、深入、客观的中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前大型语言模型（LLM）被广泛用作聊天机器人，但它们缺乏根据用户个性化偏好进行动态调整响应的能力。用户在与LLM的长期交互中会表达各种显式或隐式的偏好（如口味、写作风格、信息详略），而现有模型往往在对话上下文中“遗忘”这些偏好，导致回应不个性化，影响用户体验。
- **核心问题**：如何系统性地评估LLM在长对话中推断、记忆并遵循用户偏好的能力？现有评估缺乏标准化的基准，尤其缺乏涵盖多轮、长上下文的个性化场景。
- **整体含义**：本研究通过构建PrefEval基准，为衡量LLM在个性化文本生成中的“偏好遵循”能力提供了第一个标准化工具，揭示了当前顶尖模型在此任务上的严重不足，并探索了多种提升方法（如微调），为后续开发真正的个性化对话代理奠定了基础。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个包含**显式偏好**（用户直接陈述，如“我喜欢简洁的回答”）和**隐式偏好**（需从用户行为/历史中推断，如用户常追问细节暗示偏好深度）的评估基准，通过生成任务和分类任务量化LLM的偏好遵循能力。
- **关键技术细节**：
    - **PrefEval基准**：包含3,000个人工精标用户偏好-查询对，覆盖20个主题，确保领域多样性。
    - **评估形式**：
        - **生成任务**：给定用户偏好和当前查询，要求LLM生成符合偏好的响应。
        - **分类任务**：给定多个候选响应，判断哪个最符合用户偏好。
    - **实验框架**：
        - **多会话对话**：模拟真实场景，将偏好分布在多个会话（session）中，总上下文长度可达100k token（约10轮对话即3k token左右）。
        - **基线方法**：零样本直接提示、迭代反馈、检索增强生成（RAG）。
        - **性能指标**：偏好遵循准确率（Pref Acc）等。

### 3. 实验设计：使用的数据集/场景、基准、对比方法

- **数据集**：PrefEval（自建），3,000对偏好-查询对，20个主题。
- **场景**：多会话长上下文对话，上下文长度变化从短（数轮）到长（100k token）。
- **Benchmark**：对比了10个商业及开源LLM，包括：
    - 开源模型：LLaMA系列、Mistral等（具体名称未在摘要中列出，但提及了10个模型）
    - 商业模型：GPT-4、Claude等（推测）
- **对比方法**：
    - **零样本 (Zero-shot)**：无任何偏好记忆机制。
    - **改进提示 (Advanced Prompting)**：如Chain-of-Thought、角色设置。
    - **迭代反馈 (Iterative Feedback)**：基于历史错误进行纠正。
    - **检索增强生成 (RAG)**：从外部存储中检索相关偏好。
    - **微调 (Fine-tuning)**：在PrefEval数据上对模型进行指令微调。

### 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量及训练时长。摘要仅提及“fine-tuning on PrefEval significantly improves performance”，未披露具体计算资源细节。
- **推测**：由于涉及10个模型评估与微调，算力需求较高，但论文未公开具体硬件配置。这是本工作的一个可补充之处。

### 5. 实验数量与充分性

- **实验数量**：
    - 评估了10个模型（商业+开源），覆盖不同规模。
    - 上下文长度从短（3k tokens约10轮）到长（100k tokens）变化。
    - 测试了多种提示策略和RAG方法。
    - 进行了微调实验验证提升效果。
- **充分性评估**：
    - **优点**：多模型、多方法、多上下文长度的比较较为全面；使用了生成和分类双重任务，增强了评估稳健性。
    - **不足**：未提及消融实验（如偏好形式显式vs隐式的单独影响），也未进行跨领域的鲁棒性测试（如偏好冲突场景）。此外，微调实验仅展示了性能提升，未提供与基线的统计显著性检验。整体充分但可更深入。

### 6. 论文的主要结论与发现

- **主要发现**：
    1. **严重不足**：在零样本设置下，仅10轮对话（约3k tokens）后，大多数模型的偏好遵循准确率**低于10%**，反映了LLM在长上下文中严重遗忘用户偏好。
    2. **方法虽有效但有限**：使用高级提示策略（如迭代反馈）和RAG能部分提升性能，但在更长上下文中性能仍持续下降，无法根本解决问题。
    3. **微调提升显著**：在PrefEval上对模型进行微调后，偏好遵循能力大幅提升，表明该基准可作为有效的训练数据。
    4. **基准价值**：PrefEval成功揭示了现有LLM的个性化能力短板，为未来研究提供了标准化评估手段。

### 7. 优点：方法或实验设计上的亮点

- **创新性**：首次系统定义并评估LLM的“偏好遵循”能力，填补了个性化文本生成评估的空白。
- **数据质量**：手工构建3,000对高质量偏好-查询对，覆盖20个主题，保证了数据的真实性和多样性。
- **评估设计**：同时采用生成任务和分类任务，从开放式和封闭式两个角度衡量，更全面地反映模型能力。
- **上下文长度考量**：模拟多会话长对话（100k tokens），贴近真实应用场景，挑战模型的长上下文记忆与推理能力。
- **多种基线对比**：包括零样本、高级提示、RAG、微调等，提供了丰富的基线参考。

### 8. 不足与局限

- **实验覆盖**：仅测试了10个模型，未涵盖更广泛的开源模型（如Mamba、Gemma等）或不同参数规模变体。
- **偏差风险**：数据集偏好类型可能偏向日常对话场景（20个主题），未充分覆盖专业领域或文化差异。此外，偏好形式（显式/隐式）的区分可能不够细致。
- **应用限制**：基准专注于单用户静态偏好，未考虑偏好随时间动态变化或冲突情况。评估指标仅为准确率，缺乏对生成文本质量（如流畅性、安全性）的综合考量。
- **资源透明度**：未公开算力需求，不利于实验复现。
- **微调实验有限**：仅展示了微调带来提升，但未分析不同微调策略（如数据规模、学习率）的影响，也未讨论过拟合风险。

（完）
