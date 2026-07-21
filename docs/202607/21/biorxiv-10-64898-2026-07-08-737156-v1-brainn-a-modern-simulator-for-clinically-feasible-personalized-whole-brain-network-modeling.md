---
title: "BraiNN: A Modern Simulator for Clinically Feasible Personalized Whole-Brain Network Modeling"
title_zh: BraiNN：一种面向临床可行的个性化全脑网络建模的现代模拟器
authors: "Fasse, A., Billi, C., Garvalov, V., Morvan, M., Newton, T., Kuster, N., Neufeld, E."
date: 2026-07-13
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.08.737156v1.full.pdf"
tags: ["query:pers-gen"]
score: 7.0
evidence: 个性化全脑网络建模框架，适应个体患者数据
tldr: 个性化全脑建模的计算成本阻碍其临床应用。现有框架无法在临床时间尺度内完成高细节网络拟合。本文提出BraiNN，基于JAX和GPU/TPU加速，结合贝叶斯优化与梯度优化，实现EEG驱动谱拟合。在单消费级GPU上2-3小时完成8维参数空间优化，速度提升2-3个数量级。BraiNN将个性化全脑模型构建从数天缩短至数小时，推动患者数字孪生和脑电引导神经调控的临床转化。
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 891, \"height\": 571, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1664, \"height\": 907, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 894, \"height\": 534, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 904, \"height\": 517, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 901, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 901, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 896, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1842, \"height\": 915, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 867, \"height\": 556, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-08-737156-v1/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 898, \"height\": 297, \"label\": \"Table\"}]"
motivation: 现有全脑网络建模框架计算效率低，无法在临床相关时间内完成高细节个性化参数拟合。
method: BraiNN基于JAX与XLA编译实现GPU加速，结合贝叶斯优化全局搜索和梯度优化局部精调，完成EEG驱动的8维参数空间拟合。
result: 单消费级GPU上2-3小时完成个性化全脑表面模型拟合，相比传统软件速度提升2-3个数量级，并准确复现Jansen-Rit网络动力学。
conclusion: BraiNN将个性化全脑建模时间从数天降至数小时，为临床数字孪生和脑电引导神经调控规划提供了可行基础。
---

## 摘要
个性化全脑建模旨在通过实现患者特异性的脑网络动态模拟来改变神经系统疾病的治疗方案。神经质量模型（NMMs）在生物物理细节与计算成本之间提供了可处理的折中方案，并可直接与EEG等宏观观测指标相关联。然而，将NMMs扩展到具有真实连接、传导延迟和皮层表面分辨率的全脑网络，并将其拟合到个体患者数据，所施加的计算需求在临床相关时间尺度上是现有框架无法满足的。在此，我们介绍BraiNN，一个基于JAX的Python框架，用于大规模神经质量建模，通过利用GPU/TPU加速的XLA编译数组计算，比现有工具实现了高达两到三个数量级的加速。BraiNN将区域级Jansen-Rit网络与受试者特定的耦合神经质量模型皮层表面网格以及基于互易性导联场的生物物理EEG正向建模相结合。其完全可微的计算图实现了混合个性化流程，将贝叶斯优化用于全局参数探索与基于梯度的精化相结合，在单个消费级GPU上约2-3小时内完成八维参数空间的EEG驱动谱拟合——而使用传统神经质量建模软件则需要数天。与既定基准的数值验证确认，BraiNN忠实地再现了Jansen-Rit网络的典型同步与分岔动力学。

通过将高细节全脑表面模型的个性化时间从数天减少到在消费级硬件上的几小时，BraiNN使个性化脑网络建模更接近临床实际应用。我们预期BraiNN将为患者特异性数字孪生和EEG引导的神经调控规划奠定基础。

## Abstract
Personalized whole-brain modeling aims to transform treatment planning for neurological disorders by enabling patient-specific simulations of brain network dynamics. Neural mass models (NMMs) offer a tractable compromise between biophysical detail and computational cost and can be directly linked to macroscopic observables such as EEG. However, scaling NMMs to whole-brain networks with realistic connectivity, conduction delays, and cortical surface resolution--and fitting them to individual patient data--imposes computational demands that existing frameworks cannot meet at clinically relevant timescales. Here we introduce BraiNN, a JAX-based Python framework for large-scale neural mass modeling that achieves speedups of up to two to three orders of magnitude over existing tools by leveraging GPU/TPU-accelerated, XLA-compiled array computation. BraiNN combines a region-level Jansen-Rit network with a subject-specific cortical surface mesh of coupled neural mass models and biophysically grounded EEG forward modeling via reciprocity-based lead fields. Its fully differentiable computational graph enables a hybrid personalization pipeline that pairs Bayesian optimization for global parameter exploration with gradient-based refinement, completing EEG-driven spectral fitting of an eight-dimensional parameter space in approximately 2-3 hours on a single consumer GPU--compared to multiple days with conventional neural mass modeling software. Numerical verification against established benchmarks confirms that BraiNN faithfully reproduces canonical synchronization and bifurcation dynamics of Jansen-Rit networks.

By reducing the time requirements for personalizing a high-detail whole-brain surface model from days to a few hours on consumer-grade hardware, BraiNN brings personalized brain network modeling closer to practical use in clinical contexts. We anticipate that BraiNN will serve as a foundation for patient-specific digital twins and EEG-guided neuromodulation planning.