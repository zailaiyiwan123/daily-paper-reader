---
title: Aligning LLMs by Predicting Preferences from User Writing Samples
title_zh: 通过预测用户写作样本的偏好来对齐大型语言模型
authors: "Stéphane Aroca-Ouellette, Natalie Mackraz, Barry-John Theobald, Katherine Metcalf"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=eUMGCipgtE"
tags: ["query:pers-gen"]
score: 9.0
evidence: 通过用户写作样本推断偏好来实现大型语言模型的个性化
tldr: 现有方法从用户写作样本推断偏好描述时往往过于泛化，缺乏个性化。PROSE提出通过迭代优化和跨多用户验证来提升偏好描述的精确性。实验表明，PROSE能够显著提高生成内容与用户个体偏好的匹配度，使LLM代理提供更个性化的交互体验。该方法为无需额外标注即可实现精细的模型个性化对齐提供了新思路。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有偏好推断方法生成通用描述，无法捕捉用户独特偏好。
method: 提出PROSE方法，包含迭代优化和跨用户验证两个核心步骤。
result: PROSE显著提升偏好描述精确度，使LLM对齐更个性化。
conclusion: PROSE有效增强了个性化对齐，无需额外标注数据。
---

## Abstract
Accommodating human preferences is essential for creating aligned LLM agents that deliver personalized and effective interactions. Recent work has shown the potential for LLMs acting as writing agents to infer a description of user preferences. Agent alignment then comes from conditioning on the inferred preference description. However, existing methods often produce generic preference descriptions that fail to capture the unique and individualized nature of human preferences. This paper introduces PROSE, a method designed to enhance the precision of preference descriptions inferred from user writing samples. PROSE incorporates two key elements: (1) iterative refinement of inferred preferences, and (2) verification of inferred preferences across multiple user writing samples. We evaluate PROSE with several LLMs (i.e., Qwen2.5 7B and 72B Instruct, GPT-mini, and GPT-4o) on a summarization and an email writing task. We find that PROSE more accurately infers nuanced human preferences, improving the quality of the writing agent's generations over CIPHER (a state-of-the-art method for inferring preferences) by 33\%. Lastly, we demonstrate that ICL and PROSE are complementary methods, and combining them provides up to a 9\% improvement over ICL alone. Code: https://github.com/apple/ml-predict

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有基于用户写作样本推断偏好描述的方法（如CIPHER）往往生成过于泛化、缺乏个性化的偏好描述，无法准确捕捉用户独特、细微的偏好差异，导致LLM代理在个性化交互中效果不佳。
- **研究动机**：为了使LLM代理能够提供真正个性化的交互体验，需要一种能精确从用户写作样本中推断个性化偏好的方法，从而基于该偏好描述对LLM进行对齐。
- **背景意义**：该研究属于大模型个性化对齐（personalized alignment）领域，旨在无需额外人工标注，仅利用用户已有的写作数据即可实现模型行为与用户偏好的匹配。

## 2. 论文提出的方法论：核心思想、关键技术细节

论文提出 **PROSE** 方法，核心思想是提升从用户写作样本推断偏好描述的精确性，包含两个关键元素：

- **迭代优化（iterative refinement）**：对初始推断出的偏好描述进行反复修正和细化，逐步逼近用户真正在意的高频、细微偏好特征，避免停留在通用层面。
- **跨用户写作样本验证（verification across multiple writing samples）**：将推断出的偏好描述在不同用户写作样本上进行一致性检验，确保偏好描述不是偶然匹配单个样本，而是能稳定复现于用户的其他写作风格中。
- **算法流程**（基于文字描述）：
  1. 给定用户若干写作样本，首先利用LLM（如GPT-4o）生成初始的偏好描述（如写作风格、语气、结构偏好等）。
  2. 通过迭代过程，LLM根据偏好描述与写作样本的匹配程度进行调整，剔除不准确或过泛化的部分，增加更细粒度的描述。
  3. 在多个独立写作样本上验证偏好描述的预测能力：若偏好描述能一致地解释样本中的写作决策，则保留；否则丢弃或修改。
  4. 最终得到一组高精度的个性化偏好描述，用于条件生成时的对齐。

## 3. 实验设计

- **数据集/场景**：
  - 摘要任务（summarization）：用户对摘要风格（简练、详细、侧重某方面等）有不同偏好。
  - 邮件写作任务（email writing）：用户对邮件语气、格式、长度等有个人习惯。
- **Benchmark**：未明确提及公开基准，但采用与真实用户偏好匹配程度（如生成质量由用户或人工评估）作为指标。
- **对比方法**：
  - **CIPHER**：当前最先进的偏好推断方法。
  - **ICL（In-Context Learning）**：直接利用用户写作样本作为上下文示例进行生成。
  - **PROSE**：本文方法。
  - **PROSE + ICL**：将PROSE推断的偏好描述与ICL结合。
- **使用的LLM**：Qwen2.5 7B Instruct、Qwen2.5 72B Instruct、GPT-mini、GPT-4o，涵盖不同规模与来源。

## 4. 资源与算力

- 论文摘要及提供的元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- 需指出：由于方法主要依赖LLM推理而非大规模训练，算力需求可能较低，但具体资源消耗未知。

## 5. 实验数量与充分性

- **实验组数**：摘要中提及在两种任务（摘要、邮件写作）上，使用四种LLM，对比了CIPHER、PROSE、ICL、PROSE+ICL。推测至少包含4（模型）×2（任务）×4（方法）=32组主要对比实验，但未明确列出消融实验数量。
- **充分性与公平性**：
  - 优点：覆盖了多种主流LLM（开源与闭源）、两种典型文本生成任务，且与SOTA方法对比，具有一定的代表性。
  - 不足：缺乏对更多领域（如对话、创意写作）的验证；未与其他个性化对齐方法（如RLHF、prompt tuning）对比；未公开人类评估的详细流程与统计显著性检验；未提供非英语场景的测试。

## 6. 论文的主要结论与发现

- PROSE相比CIPHER，推断的偏好描述更精准，使得写作代理的生成质量提升**33%**。
- ICL和PROSE是互补方法：PROSE侧重于偏好描述，ICL侧重于示例模仿，两者结合可进一步提升质量，最多比单独ICL提高**9%**。
- PROSE方法无需额外标注数据，仅利用用户已有的写作样本即可实现有效个性化，体现了良好的实用性与可扩展性。

## 7. 优点

- **方法创新**：提出了迭代优化+跨样本验证的组合策略，有效解决了偏好推断泛化的问题，技术思路简洁且易于实现。
- **无需额外标注**：完全基于用户自然写作数据，无需人工标注偏好，降低了应用门槛。
- **兼容性强**：可与ICL等常见技术结合使用，易于集成到现有LLM工作流中。
- **实验覆盖多样**：在多个LLM和任务上验证，体现了方法的鲁棒性。

## 8. 不足与局限

- **实验覆盖有限**：仅测试了摘要和邮件写作两种任务，未涵盖更复杂的交互场景（如对话、代码生成、多模态任务）。
- **任务单一风险**：偏好推断效果可能高度依赖写作样本的丰富性和代表性，对样本量少或风格不稳定的用户可能失效。
- **未与其他主流个性化方法比较**：缺少与基于RLHF、个人提示微调（personalized prompt tuning）等方法的对比，无法判断PROSE在完整个性化对齐生态中的相对优势。
- **偏好描述的可解释性与隐私**：未讨论用户是否认同或理解推断出的偏好描述，也未涉及写作样本的隐私保护问题。
- **消融实验不透明**：未在摘要中详细展示迭代次数、跨样本数量等超参数的影响，科学严谨性有待补充。

（完）
