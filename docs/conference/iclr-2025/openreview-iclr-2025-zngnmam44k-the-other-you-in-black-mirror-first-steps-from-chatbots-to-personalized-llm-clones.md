---
title: "The other you in black mirror: first steps from chatbots to personalized LLM clones"
title_zh: 黑镜中的另一个你：从聊天机器人到个性化LLM克隆的第一步
authors: "Mingzhong Sun, Mengmi Zhang, Gabriel Kreiman"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=znGnmAM44K"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过微调生成个性化LLM克隆
tldr: 本文研究通过个人数据微调大语言模型来部分复制个体认知的方法，构建了名为A-clone的模型。通过图灵测试评估，结果表明克隆能在问答任务中模仿个体的回答，为个性化聊天机器人奠定了基础。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 探索是否能用个人数据微调大型语言模型来复制个体的认知特征。
method: 基于Llama3-70B，使用志愿者A的个人数据集进行微调，构建A-clone模型。
result: 在701个开放式问题的图灵测试中，31名参与者难以区分A和A-clone的回答。
conclusion: 个性化LLM克隆可行，为更精细的个性化模型提供了初步证据。
---

## Abstract
Large language models (LLMs) have demonstrated remarkable abilities in a wide
variety of generic tasks. Here we investigate whether it is possible to use LLMs
to partially replicate cognitive aspects of an individual by fine-tuning an LLM
with personal data. Our model, A-clone, built on the pretrained Llama3-70B, was
fine-tuned with a private English dataset from one volunteer referred to as A throughout. We
evaluated A-clone in two ways. First, using 701 open-ended questions, we gathered
responses from A, A-clone, other LLMs, and A’s family members imitating A.
We conducted a Turing-like test where 31 participants with varying degrees of
familiarity with A attempted to identify A’s real answers in a question-and-answer
task. Human participants identified the genuine responses from A 55% ± 7%
of the time, just over chance levels. A-clone outperformed all other baselines
in mimicking adequate responses from A. Second, we compared the outputs
of A-Clone with the ground truth from A in 10 psychological, moral, career,
political tendency, and general knowledge tests, containing 484 questions altogether.
A-Clone demonstrated a strong correlation with A’s responses. This work provides
an initial, proof-of-principle, evaluation of the possibility of mimicking the
responses of an individual, opening doors to many real-world applications but
also raising potential privacy and safety concerns about digital clones. The code
and data can be found in this link.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：探索是否能够利用大型语言模型（LLMs）通过个人数据微调，部分复制一个人类个体的认知特征，从而生成个性化的“数字克隆”。这一方向源自对《黑镜》等科幻作品设想的现实化尝试，旨在为个性化聊天机器人、虚拟助手等应用奠定基础。
- **背景**：现有LLMs在通用任务上表现优异，但如何根据个人独特性（如语言风格、价值观、知识偏好）进行精准建模仍是一个开放问题。本文首次通过图灵测试和多项心理/倾向测试，系统评估了个性化克隆的可行性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：基于一个预训练的大语言模型（Llama3-70B），使用目标个体（称为“A”）的大量私人对话、问答等文本数据进行监督微调（fine-tuning），使模型模仿该个体的回答模式。
- **关键技术细节**：
  - 基础模型：Llama3-70B（70B参数）。
  - 微调数据：来自志愿者A的私有英语数据集，包含其日常对话、邮件、社交媒体等文本（具体数据量未在摘要中说明，但推测足够覆盖其回答风格）。
  - 流程：采用标准的LM微调方式（如使用交叉熵损失优化预测下一个token），未提及其他特殊技巧（如LoRA、Prompt Engineering）。论文未给出具体算法伪代码或公式。
  - 模型名：A-clone。

### 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法

- **数据集/场景**：
  - **场景1（图灵测试）**：使用701个开放式问答题，收集了来自A、A-clone、其他LLMs、以及A的家人模仿A的回答。参与者的任务是判断哪个答案来自真实个体A。
  - **场景2（标准化测试）**：从10个心理、道德、职业、政治倾向和常识测试中提取共484个问题，对比A-clone与A的真实回答的关联性。
- **Benchmark**：以A的真实回答为地面真实（ground truth），以A的家人模仿回答、其他LLMs（未明确具体模型）作为基线。
- **对比方法**：包括（1）其他LLMs（通用模型，未微调），（2）A的家人（主观模仿）。未提及与类似个性化微调方法的对比。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **资源与算力**：论文摘要及元数据中**未明确说明**训练时使用的GPU型号、数量或训练时长。仅提及模型为Llama3-70B，暗示需要较大算力（例如多卡A100或H100），但具体配置未公开。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- **实验数量**：
  - 图灵测试：1组主要实验（701道题，31名参与者，每道题判断）。
  - 标准化测试：10组测试（不同维度，共484题）。
  - 未提及消融实验（如不同训练数据量、不同模型大小、不同微调方法的对比）。
- **充分性评价**：
  - **优点**：图灵测试规模较大（31人、701题），参与者熟悉程度不同，设计合理；标准化测试覆盖多维度。
  - **不足**：
    - 仅针对单个个体（A），缺乏跨个体验证（数据来自唯一志愿者），泛化性存疑。
    - 基线方法较简单（家人模仿、通用LLM），未与其他个性化微调方法（如P-Tuning、AdaLoRA）或更多SOTA模型对比。
    - 缺少对数据量和来源的透明描述，可能影响可重复性。
    - 未进行消融实验（如去掉某类数据的影响），难以判断哪些个人特征对微调最有效。

### 6. 论文的主要结论与发现

- **图灵测试结果**：31名参与者正确识别真实A的回答的概率仅为55% ± 7%，接近随机水平。A-clone在所有基线中表现最佳（模仿成功率最高），说明克隆模型能有效“欺骗”人类评判者。
- **标准化测试**：A-clone与A的真实回答在心理、道德、政治倾向等维度呈现强相关性（具体相关系数未在摘要中给出，但描述为“strong correlation”）。
- **总体结论**：利用个人数据微调LLM来部分复制个体认知**可行**，为个性化聊天机器人提供了初步证据。同时警示了隐私和安全风险。

### 7. 优点：方法或实验设计上有哪些亮点

- **问题新颖性**：首次将“数字克隆”概念引入LLM微调，并采用图灵测试作为核心评估手段，契合主题。
- **评估方式全面**：不仅使用开放式问答，还包含多维度标准化测试，从不同侧面验证一致性。
- **参与者多样性**：31名参与者具有不同熟悉程度（从家人到陌生人），增加了评估的客观性。
- **证明门槛**：仅达到55%准确率，但已显著高于随机猜测（50%），证实了微调的有效性（尽管仍不完美）。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **个体覆盖不足**：仅使用一个志愿者（A）的数据，无法评估模型对其他人（不同文化、语言、认知风格）的适用性。
- **数据披露不全**：未公开数据集规模、来源、预处理方式，影响可复现性和数据偏差风险（如A是否代表典型人群）。
- **基线对比薄弱**：仅与“家人模仿”和“通用LLM”对比，未与更先进的个性化微调方法（如LoRA、Prefix-Tuning）或基于检索的生成方法比较。
- **评估指标单一**：图灵测试只统计识别正确率，未分析模型回答在语气、知识准确性、逻辑一致性等细粒度上的差异。
- **未考虑时效性**：个人认知会随时间变化，实验是否涉及跨时间段数据未知。
- **安全和伦理问题**：论文虽提及隐私风险，但未讨论如何防止克隆模型被滥用（如冒充、欺诈）。

（完）
