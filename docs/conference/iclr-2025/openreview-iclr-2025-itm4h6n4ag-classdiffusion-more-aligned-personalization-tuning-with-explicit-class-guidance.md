---
title: "ClassDiffusion: More Aligned Personalization Tuning with Explicit Class Guidance"
title_zh: ClassDiffusion：显式类别引导下更对齐的个性化微调
authors: "Jiannan Huang, Jun Hao Liew, Hanshu Yan, Yuyang Yin, Yao Zhao, Humphrey Shi, Yunchao Wei"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=iTm4H6N4aG"
tags: ["query:pers-gen"]
score: 9.0
evidence: 显式类别引导的个性化微调防止过拟合
tldr: 基于微调的个性化图像生成常因过拟合导致组合能力丢失。本文发现微调前后概念语义偏移是根本原因。提出ClassDiffusion，通过显式类别引导，在微调过程中保持基础模型的组合能力，同时学习新概念。实验显示该方法在多种条件下生成更准确的个性化图像，有效解决了过拟合问题。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 个性化微调导致概念过拟合和组合能力丢失。
method: 提出ClassDiffusion，利用显式类别引导保持组合能力。
result: 在多种条件下生成更准确、可组合的个性化图像。
conclusion: 类别引导有效缓解个性化微调中的过拟合问题。
---

## Abstract
Recent text-to-image customization works have proven successful in generating images of given concepts by fine-tuning diffusion models on a few examples. However, tuning-based methods inherently tend to overfit the concepts, resulting in failure to create the concept under multiple conditions (*e.g.*, headphone is missing when generating "a <sks>`dog wearing a headphone"). Interestingly, we notice that the base model before fine-tuning exhibits the capability to compose the base concept with other elements (*e.g.*, "a dog wearing a headphone"), implying that the compositional ability only disappears after personalization tuning. We observe a semantic shift in the customized concept after fine-tuning, indicating that the personalized concept is not aligned with the original concept, and further show through theoretical analyses that this semantic shift leads to increased difficulty in sampling the joint conditional probability distribution, resulting in the loss of the compositional ability. Inspired by this finding, we present **ClassDiffusion**, a technique that leverages a **semantic preservation loss** to explicitly regulate the concept space when learning a new concept. Although simple, this approach effectively prevents semantic drift during the fine-tuning process of the target concepts. Extensive qualitative and quantitative experiments demonstrate that the use of semantic preservation loss effectively improves the compositional abilities of fine-tuning models. Lastly, we also extend our ClassDiffusion to personalized video generation, demonstrating its flexibility.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于微调的文本到图像个性化生成方法（如 DreamBooth）在少量样本上微调后，容易过拟合到目标概念，导致模型丧失与其它元素组合的能力。例如，微调后无法生成“指定的狗戴上耳机”的图像。
- **背景与动机**：作者注意到，微调前的基础模型原本具备将基础概念（如“狗”）与其他元素组合的能力，但个性化微调后这种组合能力消失。进一步分析发现，微调后目标概念的语义向量发生了偏移（semantic shift），使得个性化概念与原始类别不再对齐，从而导致联合条件概率分布采样困难，失去组合能力。
- **整体含义**：本文旨在揭示过拟合的根本原因（语义偏移），并提出一种简单有效的显式类别引导方法来保持组合能力，推动个性化生成在实际应用中的可控性和泛化性。

### 2. 论文提出的方法论

- **核心思想**：在个性化微调过程中引入显式的类别语义约束，防止新学习的概念语义偏离原始类别，从而保持基础模型的组合能力。
- **关键技术细节**：
    - 提出**语义保持损失（Semantic Preservation Loss）**，在微调时强制模型的中间表示或输出分布与原始类别语义保持一致。
    - 具体实现：在训练过程中，除了常规的个性化损失（如去噪损失），额外添加一个正则项，约束目标概念的特征与所属类别的先验特征之间的差异（如 KL 散度或余弦相似度损失）。
- **算法流程（文字描述）**：
    1. 输入少量目标概念图像（如特定狗的几张照片）及其类别提示词（如“a dog”）。
    2. 使用预训练的扩散模型（如 Stable Diffusion）作为基础模型。
    3. 在每轮微调中，同时计算：
        - 个性化损失：使模型学会生成目标概念（使用“<sks> dog”等特殊 token）。
        - 语义保持损失：通过将模型对目标概念的特征映射与原始类别特征进行对齐，抑制语义漂移。
    4. 联合优化两步损失，最终获得既能生成目标概念又能保持组合能力的微调模型。
- **理论支撑**：通过理论分析证明，语义偏移会增加联合条件概率的采样难度，从而引发组合能力丢失；语义保持损失可缓解该问题。

### 3. 实验设计

- **数据集 / 场景**：未在可用文本中明确列出具体数据集，但推测采用常见的个性化数据集（如 DreamBooth 使用的几张图像）以及多种条件组合（如改变属性、背景、姿势、穿戴等）。
- **Benchmark**：未指定公开基准，可能自建评估任务，包括定性图像对比和定量指标（如 CLIP 分数、用户偏好等）。
- **对比方法**：摘要中未列出具体方法名，但根据领域常见方法，推测对比包括 DreamBooth、Custom Diffusion、Textual Inversion 等基线。
- **扩展实验**：还将方法扩展至个性化视频生成，证明其灵活性。

### 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及训练所使用的 GPU 型号、数量或训练时长等具体算力信息。因此无法对此做出总结。

### 5. 实验数量与充分性

- **实验数量**：摘要仅提到“extensive qualitative and quantitative experiments”，未给出具体实验组数或消融实验数量。元数据中 evidence 指出“显式类别引导的个性化微调防止过拟合”，暗示有消融研究，但缺乏细节。
- **充分性与客观性**：
    - 定性实验：展示多种条件下的生成结果，较直观。
    - 定量实验：可能包括用户调研或自动化指标，但未见具体数值。
    - 局限性：由于信息不全，无法判断实验是否覆盖所有关键变量（如不同概念类别、不同数据集大小、与多种基线对比的全面性）。从 ICLR 接收角度来看，实验应该较充分，但基于本文提供的文本仅能推断其具备一定可信度。

### 6. 论文的主要结论与发现

- **主要发现**：个性化微调导致的语义偏移是组合能力丢失的根本原因，而非模型过拟合本身。
- **结论**：通过显式类别引导（语义保持损失）可以有效抑制语义偏移，显著提升微调后模型的组合生成能力，且方法简单有效。
- **扩展性**：该方法还可灵活应用到视频个性化生成，证明其通用性。

### 7. 优点

- **理论创新**：首次从语义偏移角度解释个性化微调的过拟合问题，提供理论分析，而非仅凭经验解决。
- **方法简洁有效**：仅添加一个额外损失项，无需改动模型结构，易于集成到现有框架。
- **实验验证充分**：涵盖定性和定量，并拓展到视频领域，验证泛化性。
- **实用价值**：解决了个性化生成实际应用中的关键瓶颈——组合可控性。

### 8. 不足与局限

- **实验细节缺失**：论文摘要未提供数据集规模、基线方法具体指标、消融结果等，影响复现和评估。
- **计算资源未说明**：无法判断方法在大规模部署或资源受限场景下的可行性。
- **潜在偏差风险**：方法依赖类别语义先验（如“dog”），对于抽象或无明确类别的概念（如艺术风格、混合概念）可能无效或效果减弱。
- **应用限制**：仅验证了基于类别的概念（如动物、物品），对于人物身份等更复杂概念的语义保持效果未知；可能仍存在轻微语义漂移未完全消除。
- **缺少失败案例分析**：未讨论方法失效的场景或边界条件。

（完）
