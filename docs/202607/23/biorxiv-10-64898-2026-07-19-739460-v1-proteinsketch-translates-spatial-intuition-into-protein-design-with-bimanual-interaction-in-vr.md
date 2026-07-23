---
title: ProteinSketch translates spatial intuition into protein design with bimanual interaction in VR
title_zh: ProteinSketch：通过VR中的双手交互将空间直觉转化为蛋白质设计
authors: "Ma, D., Lee, J., Lee, H., Lee, S.-H., Oh, Y. G., Kim, Y.-e., Jin, T., Sutthiwanna, S., Lee, S.-J., Jeon, W., Ahn, J., Cho, R., Park, H., Jeong, S., Bae, S.-H., Kim, H. M."
date: 2026-07-20
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.19.739460v1.full.pdf"
tags: ["query:pers-gen"]
score: 6.0
evidence: 通过VR手绘进行用户交互式蛋白质生成
tldr: 蛋白质设计面临人类对三维结构控制有限的问题。ProteinSketch通过双人VR平台让用户直接绘制骨架草图，转化为扩散模型约束，并利用RFdiffusion实时优化。该方法实现了对各向异性几何的高保真控制，经冷冻电镜验证。该框架结合人类空间推理与生成式AI，实现了空间导向的形状可控蛋白质设计。
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739460-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1799, \"height\": 1840, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739460-v1/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1820, \"height\": 1741, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739460-v1/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1803, \"height\": 2267, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739460-v1/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1789, \"height\": 1732, \"label\": \"Figure\"}]"
motivation: 现有生成式AI蛋白质设计缺少对三维架构的直接控制，人类设计师需要更直观的交互方式。
method: 开发双人VR平台ProteinSketch，用户通过骨架草图与体积包络直接创建约束，结合RFdiffusion实时优化和体积条件控制。
result: 实现了对各向异性几何的高保真控制，冷冻电镜确认结构，并成功设计功能结合体及扩展minibinders到难接近表面。
conclusion: 建立了人机协作框架，将空间推理与生成式AI融合，实现空间定向、形状可控的蛋白质结构与功能设计。
---

## 摘要
基于生成式AI的蛋白质设计能够为所需功能创造多样化的结构，但人类设计师对三维架构的控制有限。我们开发了ProteinSketch，这是一个双手操作的虚拟现实平台，在其中可以直接创建沉浸式骨架草图和体积包络，并将其转化为基于扩散的蛋白质生成的约束条件。基于RFdiffusion的实时细化实现了用户指定蛋白质拓扑的交互式探索与构建。草图包络的体积条件作用实现了对各向异性几何形状的高保真控制，并通过冷冻电镜得到验证。这些用户定义的体积约束使得功能结合剂的设计以及将现有微结合剂扩展至表面成为可能，而这些表面在复杂分子环境中使用传统设计方法难以触及。这种人类-AI协作框架将人类空间推理与生成式AI相结合，用于蛋白质结构和功能的空间导向、形状控制设计。

## Abstract
Generative AI-based protein design can create diverse structures for desired functions but offers human designers limited control over three-dimensional architectures. We developed ProteinSketch, a bimanual virtual reality platform in which immersive backbone sketches and volumetric envelopes are directly created and translated into constraints for diffusion-based protein generation. RFdiffusion-based real-time refinement enabled interactive exploration and construction of user-specified protein topologies. Volumetric conditioning of sketched envelopes enabled high-fidelity control of anisometric geometries, confirmed by cryo-electron microscopy. These user-defined volumetric constraints enabled functional-binder design and extension of pre-existing minibinders to surfaces otherwise difficult to access within complex molecular environments using conventional design approaches. This collaborative human-AI framework integrates human spatial reasoning with generative AI for spatially directed, shape-controlled design of protein structure and function.