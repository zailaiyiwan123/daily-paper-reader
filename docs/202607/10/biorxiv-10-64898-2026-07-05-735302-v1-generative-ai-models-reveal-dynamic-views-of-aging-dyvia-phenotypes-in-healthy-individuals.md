---
title: Generative AI Models Reveal Dynamic Views of Aging (DyViA) Phenotypes in Healthy Individuals
title_zh: 生成式人工智能模型揭示健康个体衰老的动态视图（DyViA）表型
authors: "Ray, D., Ray, M., Pyne, S."
date: 2026-07-09
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.05.735302v1.full.pdf"
tags: ["query:pers-gen"]
score: 9.0
evidence: 利用扩散模型和GAN构建个性化衰老轨迹的生成平台
tldr: 鉴于骨质疏松在老年人群中的普遍性，迫切需要动态分析骨密度等衰老表型。本文提出DyViA平台，包含新型扩散模型DyViA-Diff和改进式生成对抗网络DyViA-mGAN，基于美国65岁以上女性队列的股骨颈骨密度(BMD)数据，利用极少量初始测量即可生成从66岁至89岁的个体化连续轨迹，并给出可接受预测区间。经过严格质量控制和多种方法比较，DyViA-Diff预测更连贯准确，DyViA-mGAN能更好编码群体与个体效应。DyViA的贡献在于将静态推断系统性地转变为动态连续模型，赋予个体在衰老过程中的战略准备能力。
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-05-735302-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1491, \"height\": 664, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-05-735302-v1/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1664, \"height\": 921, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-05-735302-v1/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1704, \"height\": 953, \"label\": \"Figure\"}]"
motivation: 现有衰老模型多为静态推断，难以捕获个体随时间动态变化，需开发生成式AI实现个性化轨迹预测。
method: 提出DyViA平台，包含扩散模型DyViA-Diff和改进GAN模型DyViA-mGAN，利用少量纵向BMD测量生成连续轨迹。
result: 在65岁以上女性队列中，DyViA从66-89岁生成个体化BMD连续预测，DyViA-Diff更准确，DyViA-mGAN更好控制效应。
conclusion: DyViA将静态推断转变为动态连续模型，为个体提供老龄化战略预见，助力健康老龄化。
---

## 摘要
背景与目标：近年来，开发健康老龄化分析策略的需求日益重要。本研究介绍了DyViA——一个生成式人工智能（genAI）平台，能够构建个性化轨迹，预测所选表型随年龄增长的合理进展。研究设计与方法：DyViA提供了一套涵盖两种主要GenAI方法的深度学习模型：DyViA-Diff，一种新的扩散模型；以及DyViA-mGAN，一种近期生成对抗网络模型的改进版本。利用美国65岁及以上女性纵向队列研究的数据，展示了股骨颈骨密度（BMD）的动态变化。结果：使用极少的初始测量数据，DyViA生成了个体特异性的BMD连续轨迹，并对应66至89岁的可接受预测区域。结果经过严格的质量控制，并在多种方法间进行了比较分析。DyViA-Diff作为更优模型，预测更连贯、更准确；而DyViA-mGAN则能够更好地编码群体与个体层面的效应。讨论与意义：鉴于老龄化人群中骨质疏松症的普遍性，DyViA以个性化、合理的BMD随年龄增长模型形式提供的genAI驱动贡献，其核心影响在于将原本关于这一明显动态现象的静态推断模型系统而严谨地转变为连续模型。DyViA输出提供的远见使个体在老龄化过程中具备一定程度的战略准备。

## Abstract
Background and objectives: In recent years, the need to develop analytical strategies for healthy aging has assumed great importance. In this study, we introduce DyViA, a generative artificial intelligence (genAI) platform that can construct personalized trajectories capable of predicting the plausible progression of selected phenotypes with advancing age. Research design and methods: DyViA presents a suite of deep learning models covering two major GenAI approaches: DyViA-Diff, a new diffusion model; and DyViA-mGAN, an improved version of a recent Generative Adversarial Network model. It demonstrated the dynamic progression of femoral neck bone mineral density (BMD) using data from a longitudinal cohort study of women in the U.S. of age 65 years or above. Results: Using very few initial measurements, DyViA generated individual-specific continuous trajectories of BMD, with a corresponding region of acceptable predictions, from 66 to 89 years. The results were subjected to rigorous quality-control and comparative analysis across multiple methods. While DyViA-Diff is the superior model with more coherent and accurate predictions, DyViA-mGAN allows for encoding population- and individual-level effects with a better control. Discussion and implications: Given the prevalence of osteoporosis in the aging population, the main impact of DyViAs genAI-driven contribution in the form of personalized, plausible models of BMD progression with age lies in the systematic yet rigorous transition from otherwise static models of inference about a clearly dynamic phenomenon to a continuous one. The foresight offered by DyViAs outputs empowers an individual by conferring a certain degree of strategic preparedness in the course of aging.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 核心问题：现有衰老研究多依赖静态推断（如骨折风险评分）或群体层面轨迹，缺乏对个体表型（如骨密度）随年龄连续、动态变化的个性化预测模型。
- 背景：骨质疏松在老年人群中高发（约43.4%低骨量、10.2%骨质疏松），女性绝经后骨密度年降幅达1-3%。传统模型如FRAX仅给出终点概率，而线性混合模型等虽能生成轨迹，但难以个性化且局限于群体均值。
- 整体含义：DyViA（Dynamic Views of Aging）是一个生成式AI平台，通过从极少量初始测量（2个时间点）和协变量出发，生成个体从66岁至89岁的连续BMD轨迹，实现从“静态推断”到“动态连续预测”的范式转变，为个体提供衰老过程中的战略准备（如生活方式调整、保险规划）。

## 2. 论文提出的方法论

- 核心思想：将轨迹预测建模为学习条件概率分布 \(p_{X|Y}(x|y)\)，其中 \(X\) 是未来时间的表型值，\(Y\) 由历史测量 \(\rho_{\text{hist}}\)、协变量 \(Q\) 和查询时间 \(t\) 组成。训练后，通过采样噪声并随时间变化生成多条候选轨迹。
- 关键技术细节：
  - **DyViA-mGAN**（改进的条件生成对抗网络）：
    - 生成器 \(G_\theta\) 包含：群体水平子网络 \(\mu_{\theta_1}(t)\)、受历史数据约束的修正项 \(A(t,\rho_{\text{hist}})\)、个体特异因子 \(E_{\theta_2}(z,\rho_{\text{hist}},Q,t)\) 及权重函数 \(w(t)\)（在历史时间点处为零）。该结构显式编码混合效应。
    - 训练采用最小二乘对抗损失（LSGAN）。
    - 预测时：固定噪声 \(z\)，遍历时间 \(t\) 得一条轨迹；重采样 \(z\) 得多条轨迹。
  - **DyViA-Diff**（条件得分扩散模型）：
    - 通过前向扩散将目标分布转化为高斯分布，反向过程由Itô随机微分方程驱动，需学习得分函数 \(s_\theta(x,\tau,y)=\nabla_x \log p_\tau(x|y)\)。
    - 训练目标为得分匹配损失，通过去噪得分匹配技巧实现。
    - 预测时：从高斯噪声 \(x_T\) 开始，使用训练好的得分网络逐步去噪，并结合条件 \(y\) 得到时间点 \(t\) 的预测值。
- 公式与算法流程（文字说明）：
  - 训练阶段：使用带标签数据集 \(\mathbb{D}=\{(x^{(i)},y^{(i)})\}\)，分别优化GAN的生成器和判别器或Diff的得分网络。
  - 测试阶段：对新个体，输入历史测量和协变量，多次采样噪声（mGAN的 \(z\) 或Diff的 \(x_T\)），生成1000条轨迹，取均值作为预测，并计算5%-95%分位作为90%可信区间。

## 3. 实验设计

- 数据集：使用美国**SOF（Study of Osteoporotic Fracture）** 公开纵向研究，纳入9704名主要为白人的65岁以上女性。选取**股骨颈骨密度（FND）的T分数**作为目标表型。
- 实验设置：
  - 每个个体有4次临床访问年龄（\(t_1\sim t_4\)），前两次（\(K=2\)）作为历史输入（即\(\rho_{\text{hist}}\)），后两次用于评估。
  - 生成\(L=1000\)条轨迹，取均值+90%可信区间。
- 对比方法：
  - **主要对比**：DyViA-mGAN vs DyViA-Diff。
  - **补充对比**（见补充材料）：原始DyViA-GAN、标准线性混合模型（LMM），以及过滤异常轨迹策略。
- 评估指标：**RMSE**（基于后两个时间点的预测均值与真实值的平方误差）、可视化轨迹和置信区间。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- 仅描述使用了深度学习模型（神经网络）进行训练，但没有提供具体训练环境和计算资源细节。

## 5. 实验数量与充分性

- **实验数量**：核心实验仅在SOF这一个数据集上比较了两个模型（mGAN和Diff），并在补充材料中增加了与LMM和原始GAN的对比以及过滤策略分析。总体实验组数较少。
- **充分性与公平性**：
  - 优点：使用标准RMSE和可视化方法，对比方法包括传统统计模型和先前GAN变体，相对公平。
  - 不足：缺少跨数据集验证、不同人群（性别、种族）的验证、不同表型的验证、以及交叉验证或重抽样评估。实验覆盖范围有限，主要作为概念验证。
  - 客观性：指标和可视化都较为客观，但未报告统计显著性检验（如p值或置信区间对比）。

## 6. 论文的主要结论与发现

- **DyViA-Diff优于DyViA-mGAN**：测试RMSE分别为0.4364和0.4411，Diff更准；且Diff生成的轨迹更连贯、可信区间更紧凑（靠近历史时间点）且能更好包含真实值。
- **DyViA-mGAN的优势**：显式建模混合效应，可分离群体和个体影响，且通过修正项保证轨迹在历史时间点完全匹配。
- **总体贡献**：验证了生成式AI能够从极少量数据生成合理、个性化的衰老表型连续轨迹，实现了从静态推断到动态预测的转换。

## 7. 优点

- **方法创新**：首次将扩散模型应用于个性化衰老轨迹生成，并改进GAN加入混合效应结构，增强可解释性。
- **输入要求极低**：仅需2个时间点的表型测量和协变量，即可预测长达20多年的连续变化。
- **输出实用性**：提供轨迹均值及置信区间，可直接辅助个体决策（如生活方式调整、保险规划）。
- **模型灵活性**：平台包含多种生成模型，可供用户根据对群体/个体效应的控制需求选择。

## 8. 不足与局限

- **人群局限性**：仅使用SOF数据，全部为65岁以上白人女性，结论推广至男性、其他种族或年轻人群需要验证。
- **实验覆盖不足**：仅在一个数据集上测试，未做外部验证；未考虑骨密度相关的动态事件（骨折、用药、生活方式改变）对轨迹的修正。
- **可用性限制**：算力资源未报告，模型复杂度及训练可重复性存疑；当前为预印本，未经同行评审。
- **应用风险**：模型预测仅作为“战略准备”参考，未达到临床决策级别；缺乏实际临床验证和前瞻性测试。
- **未比较更广泛基准**：如时间序列预测模型（LSTM、Transformer）或统计生长曲线模型，无法全面评估生成式AI的相对优势。

（完）
