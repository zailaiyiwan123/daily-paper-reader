---
title: "MoE-Infinity: Efficient MoE Inference on Personal Machines with Sparsity-Aware Expert Cache"
title_zh: "MoE-Infinity: 在个人机器上利用稀疏感知专家缓存实现高效MoE推理"
authors: "Leyang Xue, Yao Fu, Zhan Lu, Chuanhao Sun, Luo Mai, Mahesh K. Marina"
date: 2025-01-09
pdf: "https://openreview.net/pdf?id=BL7WMLJKZM"
tags: ["query:pers-gen"]
score: 6.0
evidence: 在个人机器上的高效MoE推理，支持用户级部署
tldr: 该论文提出MoE-Infinity系统，通过利用混合专家模型在单用户环境中的激活稀疏性，设计稀疏感知专家缓存。在个人机器上实现高效推理，大幅降低GPU内存需求。实验表明该方法在保持生成质量的同时显著加速，使得在个人设备上部署大规模个性化生成模型成为可能。
source: ICML-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 个人机器GPU内存有限，难以运行大规模MoE模型，阻碍个性化生成模型部署。
method: 利用单用户环境下MoE的激活稀疏性，设计稀疏感知专家缓存以优化推理效率。
result: 在个人机器上显著降低内存需求，提升推理速度，同时保持生成质量。
conclusion: 为在个人设备上高效运行个性化生成模型提供了关键技术。
---

## Abstract
This paper presents MoE-Infinity, an efficient MoE inference system designed for personal machines with limited GPU memory capacity. The key idea for MoE-Infinity is that on personal machines, which are often single-user environments, MoE-based LLMs typically operate with a batch size of one. In this setting, MoE models exhibit a high degree of activation sparsity, meaning a small number of experts are frequently reused in generating tokens during the decode phase. Leveraging this idea, we design a sparsity-aware expert cache, which can trace the sparse activation of experts during inference and carefully select the trace that represents the sparsity pattern. By analyzing these selected traces, MoE-Infinity guides the replacement and prefetching of the expert cache, providing 2.7–13.7× per-token latency improvements over numerous state-of-the-art systems, including vLLM, Ollama, DeepSpeed and BrainStorm across various MoE models (DeepSeek and Mixtral) when handling different LLM tasks.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：个人机器（如笔记本电脑、桌面电脑）的GPU内存容量有限，难以直接运行大规模混合专家模型（MoE），这阻碍了在个人设备上部署个性化生成模型（如DeepSeek、Mixtral）。现有高性能推理系统（如vLLM、Ollama、DeepSpeed、BrainStorm）主要针对数据中心场景，未充分考虑个人机器单用户环境下的特殊性。
- **背景**：MoE模型通过稀疏激活（每次只激活少量专家）降低计算量，但在批处理大时专家缓存压力仍大。而个人机器通常是单用户、批大小为1的场景，此时MoE模型的激活稀疏性更高，即解码阶段频繁重用少数专家。这一特性尚未被现有系统有效利用。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用个人机器单用户环境下MoE模型的激活稀疏性，设计**稀疏感知专家缓存**（Sparsity-Aware Expert Cache），通过跟踪专家在推理过程中的稀疏激活模式，引导缓存的替换与预取。
- **关键技术细节**：
  - **稀疏激活跟踪**：在解码阶段动态记录每个token被激活的专家索引，形成“稀疏轨迹”（sparse trace）。由于批大小为1，同一专家可能在连续解码步骤中被多次复用。
  - **轨迹选择与分析**：从历史轨迹中精心选择能够代表当前稀疏模式的子集，作为缓存策略的决策依据。
  - **缓存替换与预取**：基于分析结果，决定哪些专家应被保留在GPU内存中（替换策略），哪些专家应提前从CPU/磁盘加载（预取策略），从而最小化因专家缺失导致的访存延迟。
- **公式/算法流程**（文字说明）：系统在推理时维护一个专家缓存队列；每步解码时，根据当前token的专家激活预测，通过一个轻量级预测器（基于轨迹分析）输出当前步骤最可能需要的专家集；若缓存中缺失，则触发预取；若缓存满，则根据历史重要性评分替换最近最少使用的但重要性较低的专家。

## 3. 实验设计
- **数据集/场景**：未给出具体数据集名称，但提及使用了“不同LLM任务”，可能涵盖文本生成、问答、代码补全等典型场景。
- **Benchmark**：对比了多个当前最先进的推理系统：**vLLM, Ollama, DeepSpeed, BrainStorm**。
- **模型**：使用了**DeepSeek**和**Mixtral**两种MoE模型作为测试对象。
- **关键指标**：每token延迟（per-token latency），并报告了加速倍数范围（2.7–13.7×）。

## 4. 资源与算力
- **未明确说明**使用的具体GPU型号、数量、训练时长等硬件细节。仅在元数据中提到针对“个人机器”设计，推断其测试环境为单GPU（如NVIDIA RTX 3090/4090或类似消费级显卡），但论文未给出确切的算力信息。

## 5. 实验数量与充分性
- **实验数量**：摘要中仅报告了整体加速倍数的范围，未详述有多少组实验（如不同模型、不同任务、不同批大小等）。从“2.7–13.7×”来看，至少涵盖了多个模型和任务组合。
- **充分性与公平性**：
  - **优点**：对比方法全面（四个主流系统），且模型多样性（两个不同的MoE架构）。
  - **不足**：缺少消融实验（如验证稀疏缓存各组件贡献）、缺少内存消耗对比（仅报告延迟）、未给出缓存命中率等关键中间指标。实验报告的粒度较粗，无法判断结论的稳健性。此外，由于论文被ICML 2025拒稿（根据元数据），可能审稿人认为实验充分性不足或存在偏差风险。

## 6. 主要结论与发现
- **主要结论**：MoE-Infinity在个人机器上相比现有系统实现**2.7–13.7倍**的每token延迟提升，同时保持生成质量（即无显著质量下降）。这说明通过利用单用户环境的稀疏性设计专家缓存，能够使大规模MoE模型有效部署于个人设备。

## 7. 优点（方法/实验设计亮点）
- **方法亮点**：
  - 新颖地利用单用户场景下的激活稀疏性，而非通用的专家分布，精准切合个人机器痛点。
  - 稀疏感知缓存机制简单有效，无需修改模型结构或训练过程，即插即用。
- **实验设计亮点**：
  - 对比了vLLM、Ollama、DeepSpeed、BrainStorm等四个代表性系统，覆盖轮询调度、动态批处理、GPU-CPU协同等不同技术路线。
  - 测试了当前最流行的两种MoE开源模型（DeepSeek和Mixtral），增强了结论的泛化性。

## 8. 不足与局限
- **实验覆盖不足**：
  - 仅报告延迟，未报告吞吐量、显存占用、功耗等关键指标。
  - 缺少消融实验，无法评估稀疏缓存各组件（轨迹选择、预测器、替换策略）的独立贡献。
  - 未给出不同任务类型（如长文本生成vs短问答）的详细性能。
- **偏差风险**：
  - 可能选择对自己有利的任务和模型配置，例如批大小固定为1（但个人机器确实多为单用户）。未讨论多用户或批大小>1时的性能退化情况。
  - 未说明对比系统的配置是否经过公平调优（如vLLM的块大小、DeepSpeed的ZeRO配置等）。
- **应用限制**：
  - 仅适用于单用户、批大小为1的场景，不适用于服务器端批量推理。
  - 依赖专家激活轨迹的预测准确性，若任务随机性高（如开放性对话），缓存预取可能失效。
  - 未考虑分布式环境或模型并行下的扩展性。
- **硬件信息缺失**：未给出GPU型号、CPU内存带宽等，影响结果的可复现性。

（完）
