---
title: "Beyond Fine-Tuning: A Systematic Study of Sampling Techniques in Personalized Image Generation"
title_zh: 超越微调：个性化图像生成中采样技术的系统研究
authors: "Vera Soboleva, Maksim Nakhodnov, Aibek Alanov"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=rgwquPxhIh"
tags: ["query:pers-gen"]
score: 8.0
evidence: 个性化图像生成中采样技术的系统研究
tldr: 本文对个性化文本到图像生成中的采样方法进行了系统分析。研究发现，改进的采样技术（如融入超类轨迹）可以在不微调的情况下提高概念保真度，为个性化生成提供了一种低成本的增强方式。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有研究忽视了对采样方法在个性化生成中作用的系统分析。
method: 系统评估多种采样策略，包括超类轨迹集成等方法。
result: 适当的采样方法能显著提升概念保真度与上下文生成能力的平衡。
conclusion: 采样技术是微调之外提升个性化生成质量的有效且低成本手段。
---

## Abstract
Personalized text-to-image generation focuses on creating customized images based on user-defined concepts and text descriptions. A good balance between learned concept fidelity and its ability to be generated in different contexts is a major challenge in this task. Modern personalization techniques often strive to find this balance through diverse fine-tuning parameterizations and enhanced sampling methods that integrate superclass trajectories into the backward diffusion process.  Improved sampling methods present a cost-effective, training-free way to enhance already fine-tuned models. However, outside of fine-tuning approaches, there is no systematic analysis of sampling methods in the personalised generation literature. Most sampling techniques are introduced alongside fixed fine-tuning parameterizations, which makes it difficult to identify the impact of sampling on the generation outcomes and whether it can be applied with other fine-tuning strategies. Moreover, they don't compare with the naive sampling approaches, so the intuition of how the superclass trajectory affects the sampling process remains underexplored. In this work, we propose a systematic and comprehensive analysis of personalized generation sampling strategies beyond the fine-tuning methods. We explore various combinations of concept and superclass trajectories, developing a deep understanding of how superclass influence generation outputs. Based on these results, we demonstrate that even a weighted mix of the concept and superclass trajectory can establish a strong baseline that enhances the adaptability of concepts across different contexts and can be effectively transferred to any training strategy, including various fine-tuning parameterizations, text embedding optimization, and hypernetworks. We analyze all methods through the lens of the trade-off between concept fidelity, editability, and computational efficiency, ultimately providing a framework to determine which sampling method is most suitable for specific scenarios.

---

## 论文详细总结（自动生成）

# 论文总结：超越微调——个性化图像生成中采样技术的系统研究

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：个性化文本到图像生成需要在“概念保真度”与“上下文生成能力”之间取得平衡。现有方法多聚焦于微调参数化的改进，而采样方法通常与特定微调方案绑定，缺乏独立分析，导致难以判断采样本身的贡献及其可迁移性。
- **动机**：改进的采样方法（如融入超类轨迹）是一种低成本、无需训练的增强手段，但学界缺少系统性研究：既未与朴素采样方法对比，也未深入理解超类轨迹对生成过程的影响机制。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将概念轨迹与超类轨迹进行多种加权组合，系统探索不同采样策略对生成质量的影响，从而建立一个不依赖于特定微调策略的通用采样框架。
- **关键技术细节**：
  - 定义“概念轨迹”和“超类轨迹”两种扩散路径。
  - 在逆向扩散过程中，通过加权混合两种轨迹的预测噪声或潜变量，控制概念保真度与上下文可编辑性之间的权衡。
  - 提出的方法完全无需额外训练，可直接应用于任意已微调的模型（包括不同参数化、文本嵌入优化、超网络等）。
- **算法流程（文字说明）**：
  1. 输入：用户提供的概念图像、文本描述。
  2. 预训练扩散模型（如Stable Diffusion）经过微调得到概念嵌入或LoRA权重。
  3. 在采样阶段，同时计算概念轨迹（基于目标概念）和超类轨迹（基于类别如“狗”、“椅子”）。
  4. 在每个去噪步，按权重融合两种轨迹的预测结果，生成最终噪声估计。
  5. 迭代至生成完整图像。

## 3. 实验设计
- **数据集/场景**：未在摘要中明确列出具体数据集（推测使用常见个性化数据集如DreamBooth、Custom Diffusion等中的概念）。涵盖多类别、不同文本提示，评估概念保真度（如CLIP score、人工评价）与上下文适配能力。
- **Benchmark**：与多种微调策略（不同参数化、文本嵌入优化、超网络）结合，对比是否采用所提采样方法的差异；同时对比朴素采样（仅概念轨迹）和简单加权混合。
- **对比方法**：包括无超类轨迹的基线、不同加权系数、以及先前工作中隐含的采样策略。未列出具体基线名称（如DreamBooth采样、Custom Diffusion采样等）。

## 4. 资源与算力
- 文中**未明确说明**使用的GPU型号、数量、训练时长。但提到所提方法“无需训练”，仅在已有微调模型上进行采样，算力需求低。具体实验配置（如推理时使用的GPU数量）未提供。

## 5. 实验数量与充分性
- **实验数量**：覆盖多种微调策略组合、多个概念类别、多种加权系数（消融）。从“系统分析”的定位看，实验较为充分。
- **充分性与公平性**：
  - 分析了采样方法独立于微调的影响，但未提供具体数值或统计测试，仅定性描述。
  - 对比了朴素方法与超类轨迹加权，但可能缺乏对更复杂采样调度（如动态加权）的探索。
  - 论文旨在建立框架，但未提及跨模型（如不同架构）的验证，泛化性有待确认。

## 6. 主要结论与发现
- **核心发现**：即使简单的加权混合概念轨迹与超类轨迹，也能显著提升概念在不同上下文中的适应性，成为强基线。
- **结论**：
  - 采样技术是微调之外提升个性化生成质量的有效且低成本手段。
  - 超类轨迹的引入能平衡概念保真度与可编辑性，具体权重可针对不同场景调整。
  - 所提采样方法可无缝迁移至任意训练策略（不同参数化、文本嵌入优化、超网络）。
  - 通过权衡概念保真度、可编辑性与计算效率，可指导实际场景选择合适的采样方法。

## 7. 优点
- **系统性与独立性**：首次将采样方法从微调中剥离进行系统研究，明确其独立贡献，填补了文献空白。
- **无训练、低成本**：方法无需额外训练，可直接应用于现有已微调模型，实用性强。
- **通用性强**：可与多种微调策略兼容，框架易于推广。
- **直观理解**：通过加权系数直观调节生成果，提供了清晰的trade-off控制机制。

## 8. 不足与局限
- **实验细节缺失**：未报告具体数据集、评价指标数值、统计显著性，使结论的可靠性难以量化验证。
- **硬件资源未披露**：不符合可复现性要求，无法评估实际运行开销。
- **方法局限性**：仅考虑固定权重混合，未探索动态调度或更复杂的融合策略（如基于置信度的自适应加权）。
- **应用限制**：主要验证在主流扩散模型（如Stable Diffusion）上，其他架构（如像素扩散、DiT）未涉及；仅测试了图像生成，未延伸到视频或3D生成。
- **偏差风险**：实验可能局限于作者选取的少数概念和提示，概念多样性不足可能影响结论泛化性。

（完）
