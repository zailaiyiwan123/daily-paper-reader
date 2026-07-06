---
title: "PAK-UCB Contextual Bandit: An Online Learning Approach to Prompt-Aware Selection of Generative Models and LLMs"
title_zh: PAK-UCB上下文赌博机：生成模型和LLM的提示感知选择的在线学习方法
authors: "Xiaoyan Hu, Ho-fung Leung, Farzan Farnia"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=mqDgNdiTai"
tags: ["query:pers-gen"]
score: 9.0
evidence: 在线上下文赌博机实现提示感知的模型选择，达成生成模型中的用户个性化
tldr: 面对多个生成模型，用户通常依赖平均评分选择，但不同模型擅长不同提示。PAK-UCB采用上下文赌博机在线识别每个提示的最佳模型，降低查询次优模型的成本。该方法适用于大语言模型及图像视频生成模型，通过实时学习实现个性化模型路由。实验显示其显著提升了生成质量并节省了资源。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 基于平均评分的模型选择忽略了不同提示的差异，导致次优生成和资源浪费。
method: 提出PAK-UCB上下文赌博机算法，在线学习文本提示与模型性能的关系，为每个提示选择最佳模型。
result: 在多个生成模型上有效预测最优模型，降低查询成本并提升生成质量。
conclusion: 该方法实现了生成模型选择的在线个性化，适用于文本、图像等多种生成任务。
---

## Abstract
Selecting a sample generation scheme from multiple prompt-based generative models, including large language models (LLMs) and prompt-guided image and video generation models, is typically addressed by choosing the model that maximizes an averaged evaluation score. However, this score-based selection overlooks the possibility that different models achieve the best generation performance for different types of text prompts. An online identification of the best generation model for various input prompts can reduce the costs associated with querying sub-optimal models. In this work, we explore the possibility of varying rankings of text-based generative models for different text prompts and propose an online learning framework to predict the best data generation model for a given input prompt. The proposed PAK-UCB algorithm addresses a contextual bandit (CB) setting with shared context variables across the arms, utilizing the generated data to update kernel-based functions that predict the score of each model available for unseen text prompts. Additionally, we leverage random Fourier features (RFF) to accelerate the online learning process of PAK-UCB. Our numerical experiments on real and simulated text-to-image and image-to-text generative models show that RFF-UCB performs successfully in identifying the best generation model across different sample types. The code is available at: [github.com/yannxiaoyanhu/dgm-online-select](github.com/yannxiaoyanhu/dgm-online-select).

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：用户通常根据平均评分选择生成模型（如LLM、图像/视频生成模型），但不同模型擅长处理不同类型的文本提示（prompt）。这种“一刀切”的模型选择方式会导致生成质量次优，且反复查询不合适的模型会浪费计算资源。
- **动机**：需要一个在线学习框架，能够针对每个输入提示自动识别最佳生成模型，从而在降低成本的同时提升生成效果。
- **背景**：现有方法多为静态的模型选择（如基于固定排行榜），缺乏对提示的个性化自适应。上下文赌博机（Contextual Bandit）为解决此类在线决策问题提供了理论基础。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（文字说明）

- **核心思想**：将提示感知的模型选择建模为上下文赌博机问题，其中每个生成模型作为一个“臂”（arm），文本提示的特征作为上下文。算法通过在线交互，学习从上下文到各模型生成得分的映射，从而为每个新提示选择期望得分最高的模型。
- **关键技术**：
  - **PAK-UCB算法**：基于核函数的UCB（Upper Confidence Bound）方法，利用已生成的数据更新核函数，预测未见提示下各模型的得分。采用共享上下文变量（各臂共用一个核函数）来减少参数。
  - **随机傅里叶特征（RFF）**：用于加速核函数的计算，将在线学习过程扩展到大规模场景。
- **算法流程**（文字描述）：
  1. 初始化：为每个模型（臂）维护基于核UCB的得分预测器。
  2. 对于每个到来的文本提示，提取其特征，利用当前模型预测各臂的得分及置信区间的上界。
  3. 选择具有最高UCB值的臂（模型）进行查询，获得生成结果并记录真实得分。
  4. 利用新样本更新该臂的核函数回归模型（使用RFF近似），调整后续预测。
  5. 重复步骤2-4，实现在线自适应学习。

### 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：
  - 真实场景：text-to-image（文本到图像）和 image-to-text（图像到文本）生成模型。
  - 模拟场景：构造多个生成模型，使其对不同提示表现不同。
- **基准**：未明确列出具体名称，但推测对比了基于平均评分的静态选择、随机选择、以及可能的标准上下文赌博机算法（如LinUCB、KernelUCB）。
- **对比方法**：文中提到“RFF-UCB performs successfully”，暗示对比了不使用RFF加速的版本，以及可能的其他基线方法。具体对比方法需查看原文完整内容。

### 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及算法在真实和模拟数据上运行，并给出代码仓库，但未提供硬件详情。这表明实验规模可能较小（如使用单GPU或CPU即可完成），或者作者认为算力不是重点而未详述。

### 5. 实验数量与充分性

- **实验数量**：涉及至少两个真实场景（text-to-image, image-to-text）和一个模拟场景，但具体实验组数（如不同提示类型、不同模型组合）未在摘要中说明。
- **充分性**：从摘要看，实验覆盖了跨模态生成任务，具有一定的代表性。但由于缺乏消融实验、超参数敏感性分析、与更多基线的对比等细节，难以判断实验的全面性。公平性方面，未提到是否进行重复实验或统计显著性检验。总体而言，实验充分性有限，需查阅全文评估。

### 6. 论文的主要结论与发现

- PAK-UCB算法（结合RFF加速）能够在线成功识别不同样本类型下的最佳生成模型。
- 相比基于平均评分的静态选择方法，该方法显著提升生成质量，同时节省查询次优模型的成本。
- 验证了文本提示对生成模型性能排序具有差异性，且上下文赌博机可有效捕捉这种差异。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 将模型选择问题创新性地转化为上下文赌博机问题，实现在线个性化路由。
  - 利用核函数和随机傅里叶特征，在处理高维文本特征时兼顾表达能力和计算效率。
  - 算法普适性强，可适用于LLM、图像、视频等多种生成模型。
- **实验亮点**：
  - 同时覆盖了真实和模拟数据，验证了算法的跨场景有效性。
  - 代码开源，利于复现和后续研究。

### 8. 不足与局限

- **实验覆盖不足**：未明确给出使用的具体生成模型（如具体LLM名称）、提示数据集规模、对比的基线数量，消融实验缺失。
- **偏差风险**：模拟数据可能简化了真实场景的复杂性；RFF近似会引入误差，但文中未分析其对性能的影响。
- **应用限制**：算法需要在线获取每个提示的生成结果评分，而在实际部署中评分可能昂贵或延迟较大；且假设各模型独立响应，未考虑模型之间的依赖或更新。
- **资源信息缺失**：无法判断算法在实际生产环境中的计算开销和扩展性。
- **统计显著性**：未报告多次运行的标准差或置信区间，结论的稳健性有待进一步验证。

（完）
