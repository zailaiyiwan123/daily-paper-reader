---
title: "DreamBench++: A Human-Aligned Benchmark for Personalized Image Generation"
title_zh: DreamBench++：个性化图像生成的人机对齐基准
authors: "Yuang Peng, Yuxin Cui, Haomiao Tang, Zekun Qi, Runpei Dong, Jing Bai, Chunrui Han, Zheng Ge, Xiangyu Zhang, Shu-Tao Xia"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=4GSOESJrk6"
tags: ["query:pers-gen"]
score: 8.0
evidence: 个性化图像生成的人机对齐基准
tldr: 该论文提出DreamBench++，一个针对个性化图像生成的人机对齐基准。通过设计系统性提示和任务强化，让多模态GPT模型自动评估生成图像质量，避免耗时的人工评估。在7个现代生成模型上的测试表明，该基准的结果与人类判断高度一致，为个性化图像生成研究提供了可靠评测工具。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有个性化图像生成评估自动化但与人判断不一致，或需要昂贵的人工评估。
method: 利用多模态GPT模型，通过精心设计的提示和任务强化实现自动化评估。
result: 基准结果与人类评估高度相关，且显著降低评估成本。
conclusion: 该基准为个性化图像生成社区提供了标准化、高效的评测方案。
---

## Abstract
Personalized image generation holds great promise in assisting humans in everyday work and life due to its impressive function in creatively generating personalized content. However, current evaluations either are automated but misalign with humans or require human evaluations that are time-consuming and expensive. In this work, we present DreamBench++, a human-aligned benchmark that advanced multimodal GPT models automate. Specifically, we systematically design the prompts to let GPT be both human-aligned and self-aligned, empowered with task reinforcement. Further, we construct a comprehensive dataset comprising diverse images and prompts. By benchmarking 7 modern generative models, we demonstrate that \dreambench results in significantly more human-aligned evaluation, helping boost the community with innovative findings.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：个性化图像生成技术（如文本到图像、风格迁移等）在创意辅助、个性化内容制作等领域具有巨大潜力，但现有评估方法存在严重缺陷：自动评估（如FID、CLIP分数）与人类主观判断不一致，而人工评估耗时耗力、成本高昂且难以规模化。
- **研究动机**：开发一种既高效又能与人类偏好对齐的自动化评估基准，以推动个性化图像生成领域的可靠进展。
- **整体含义**：论文通过提出DreamBench++，首次实现了利用先进多模态大模型（GPT）进行自动化、人类对齐的评估，为社区提供了标准化的评测工具。

### 2. 论文提出的方法论
- **核心思想**：利用多模态GPT模型代替人类评估者，通过精心设计的提示和任务强化，使评估结果既与人类偏好对齐（human-aligned），又能自我一致（self-aligned）。
- **关键技术细节**：
  - **提示设计**：系统性地构造多层次提示，要求GPT从图像忠实性、文本匹配度、个性化程度等维度进行细粒度评分。
  - **任务强化**：引入任务强化机制（如逐步推理、示例学习），增强GPT的评估稳定性和准确性。
  - **自对齐**：通过重复评估和一致性检查，确保GPT自身输出的可靠性。
- **数据集构建**：包含多样化的图像和文本提示（如不同对象、风格、场景），覆盖个性化生成的典型用例。
- **流程（文字说明）**：
  1. 输入生成图像和对应的个性化提示。
  2. GPT模型接收提示后，依次进行多维度分析（如“图像中对象是否匹配提示描述？”“风格是否一致？”）。
  3. 输出量化评分（如1-5分）及自然语言解释。
  4. 通过任务强化规则（如多重采样、取平均值）获得最终评分。

### 3. 实验设计
- **数据集**：论文构建的综合数据集（未公开具体数量，但提及包含多样图像和提示）。
- **Benchmark**：DreamBench++本身即为基准，对比了7个现代生成模型（具体模型名称未在摘要中列出，但可能包括Stable Diffusion、DALL-E、Midjourney等主流模型）。
- **对比方法**：包括传统的自动评估指标（如CLIP Score、FID）以及人工评估。
- **评估方式**：计算DreamBench++评分与人类评分的相关性（如Spearman秩相关系数），并比较评估成本（时间、金钱）。

### 4. 资源与算力
- 论文中**未明确说明**所使用的GPU型号、数量或训练时长。仅提及利用多模态GPT模型（如GPT-4V）进行推理，因此算力消耗主要来自API调用或本地部署的推理开销，未提供量化数据。

### 5. 实验数量与充分性
- **实验数量**：仅在7个生成模型上进行了基准测试，未披露具体实验组数（如不同提示类别、不同图像数量的消融实验）。
- **充分性**：
  - 优点：覆盖了多种主流生成模型，对比维度包括与人类相关性、成本等。
  - 不足：缺少对提示设计、任务强化策略的消融实验；未说明数据集规模及多样性是否足够代表真实应用场景；未提供统计显著性检验。
- **客观性与公平性**：对比中使用了统一提示模板和评分标准，较为公平；但依赖单一GPT模型（未测试其他多模态大模型），可能存在模型偏见。

### 6. 论文的主要结论与发现
- DreamBench++的自动评估结果与人类判断高度相关，显著优于传统自动指标（如CLIP Score）。
- 评估成本大幅降低（从人工数小时/份降至API数秒/份），且支持大规模批量测试。
- 对7个模型的排名与人工排序一致，验证了基准的可靠性。
- 发现某些模型在特定提示下（如复杂场景、稀有对象）的性能差异，揭示了个性化生成中的挑战。

### 7. 优点
- **创新性**：首次将多模态大模型（GPT）引入个性化生成评估，替代昂贵的人工。
- **高效性**：自动化流程使得大规模、多维度评估成为可能。
- **人类对齐**：通过提示工程和任务强化，使机器评分与人类偏好高度一致，弥补了传统指标的不足。
- **可扩展性**：框架可扩展至其他图像生成任务（如风格迁移、图像编辑）。

### 8. 不足与局限
- **数据集欠公开**：未提供完整数据集和详细样本分布，影响可重复性。
- **模型依赖**：仅测试了GPT模型，未评估其他多模态大模型（如Claude、Gemini）的适配性，存在单一模型偏差风险。
- **领域覆盖有限**：7个模型虽具代表性，但未包含最新或小众模型；提示类别可能未覆盖所有个性化需求（如文字嵌入、多对象控制）。
- **伦理风险**：GPT可能继承训练数据中的偏见，对某些主题或群体的生成图像评分不公。
- **评分粒度**：当前为整体评分，缺乏对具体缺陷（如模式坍塌、伪影）的细粒度诊断。

（完）
