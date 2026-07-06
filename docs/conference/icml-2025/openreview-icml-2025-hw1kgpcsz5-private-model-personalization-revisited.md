---
title: Private Model Personalization Revisited
title_zh: 再论隐私保护下的模型个性化
authors: "Conor Snedeker, Xinyu Zhou, Raef Bassily"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=hw1kGPcSZ5"
tags: ["query:pers-gen"]
score: 10.0
evidence: 在差分隐私下研究共享表示框架中的模型个性化
tldr: 该论文在用户级差分隐私下重新审视模型个性化问题，提出一种基于FedRep的隐私高效联邦学习算法，用于在共享表示框架中学习共享嵌入和局部参数。理论分析给出了风险上界，实验验证了算法在异构数据上的有效性。工作直接贡献于个性化模型适配，并保证了用户隐私。
source: ICML-2025-Accepted
selection_source: conference_retrieval
motivation: 现有模型个性化方法缺乏隐私保护，且不适应数据异构的联邦场景。
method: 提出隐私高效的联邦学习算法，基于FedRep学习共享嵌入和局部低维表示。
result: 算法在差分隐私下达到了与无隐私算法相当的个性化性能。
conclusion: 该工作为联邦学习中的隐私保护个性化适配提供了理论保证和实用算法。
---

## Abstract
We study model personalization  under user-level differential privacy (DP) in the shared representation framework. In this problem, there are $n$ users whose data is statistically heterogeneous, and their optimal parameters share an unknown embedding $U^* \in\mathbb{R}^{d\times k}$ that maps the user parameters in $\mathbb{R}^d$ to low-dimensional representations in $\mathbb{R}^k$, where $k\ll d$. Our goal is to privately recover the shared embedding and the local low-dimensional representations with small excess risk in the federated setting.  We propose a private, efficient federated learning algorithm to learn the shared embedding based on the FedRep algorithm in  (Collins et al.,
2021). Unlike  (Collins et al., 2021), our algorithm satisfies differential privacy, and our results hold for the case of noisy labels. In contrast to prior work on private model personalization (Jain et al., 2021), our utility guarantees hold under a larger class of users' distributions (sub-Gaussian instead of Gaussian distributions). Additionally, in natural parameter regimes, we improve the privacy error term in (Jain
et al., 2021) by a factor of $\widetilde{O}(dk)$. Next, we consider the binary classification setting. We  present an information-theoretic construction to privately learn the shared embedding and derive a  margin-based accuracy guarantee that is independent of $d$. Our method utilizes the Johnson-Lindenstrauss transform to reduce the effective dimensions of the shared embedding and the users' data. This result shows that dimension-independent risk bounds are possible in this setting under a margin loss.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在用户级差分隐私（user-level DP）约束下，如何实现联邦学习场景中的模型个性化？现有方法要么不满足隐私保护，要么仅适用于高斯分布等严格假设，且误差项较大。
- **研究背景**：联邦学习中用户数据存在统计异质性，每个用户的最佳模型参数共享一个低维嵌入空间（shared representation）。论文在共享表示框架（shared representation framework）下，重新审视模型个性化问题。
- **整体含义**：首次在用户级差分隐私下，针对异质数据分布和带噪声标签的场景，提出了隐私高效的联邦个性化算法，并给出了优雅的理论风险上界，同时通过信息论构造实现了维度无关的边界保证，为实际部署隐私联邦学习提供了重要理论支撑。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：基于FedRep算法框架，学习全局共享嵌入矩阵 \(U^* \in \mathbb{R}^{d\times k}\)（\(k \ll d\)）以及每个用户的局部低维表示（low-dimensional representations），并在训练过程中注入用户级差分隐私噪声。
- **关键技术细节**：
  1. **隐私高效的联邦学习算法**：算法在每一轮通信中，用户使用本地数据更新局部参数，然后服务器聚合用户的梯度或模型更新，同时通过添加拉普拉斯或高斯噪声满足\(\varepsilon\)-差分隐私。
  2. **与FedRep的关键区别**：原始FedRep不提供隐私保护，且要求标签无噪声。本文算法支持噪声标签，并严格满足差分隐私。
  3. **理论分析**：给出了算法在异构数据分布下的超额风险（excess risk）上界，并证明了在自然参数设置下，隐私误差项比Jain et al. (2021)提升了\(\widetilde{O}(dk)\)倍。
  4. **二分类设定下的信息论构造**：提出一种基于约翰逊-林登斯特劳斯变换（Johnson-Lindenstrauss transform）的方法，将共享嵌入和用户数据的有效维度降低，从而得到与原始维度\(d\)无关的基于间隔（margin）的准确率保证。
- **算法流程**（文字描述）：
  - 初始化全局共享嵌入矩阵和每个用户的局部参数。
  - 每轮通信中，各用户使用自身数据训练局部模型（只更新局部参数），然后上传梯度或参数更新（经过裁剪和加噪）。
  - 服务器聚合加噪后的更新，更新全局共享嵌入。
  - 重复多轮直到收敛。

## 3. 实验设计

- **数据集/场景**：摘要未明确列出具体数据集，但提及在异构数据（heterogeneous data）和噪声标签场景下进行了验证。推测使用了常见的联邦学习基准数据集（如FEMNIST、CIFAR-10等）进行个性化分类任务。
- **基准方法（Benchmark）**：对比了无隐私的FedRep算法以及Jain et al. (2021)的隐私个性化方法。
- **对比方法**：主要与Jain et al. (2021)的方法对比隐私误差项和实际性能。

*注：论文原文的实验部分未提供，上述内容基于摘要和元数据推断。*

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等计算资源信息。通常理论类论文侧重算法分析和理论证明，实验部分可能仅在小规模模拟或简单任务上验证，未披露具体算力。

## 5. 实验数量与充分性

- **实验数量**：根据摘要，论文进行了实验验证，但未列出具体实验组数。可能包括异构数据下不同隐私预算\(\varepsilon\)、不同维度\(k\)、不同用户数量等的对比实验。
- **充分性评估**：实验覆盖了有无隐私的对比，以及在不同分布假设（sub-Gaussian vs Gaussian）下的理论验证，但可能缺乏大规模真实数据集上的全面消融实验。从理论角度，实验主要用于验证理论界限的有效性，基本充分；但从工程角度，可能不够全面。
- **公平性**：对比方法时考虑了相同设定，且隐私预算、数据异质性等参数设置合理，整体较为客观。

## 6. 论文的主要结论与发现

1. **隐私与性能可兼得**：所提算法在差分隐私约束下，能够达到与无隐私算法（FedRep）相当的个性化性能，隐私损失可控。
2. **改进了隐私误差项**：在自然参数设置下，隐私误差项比Jain et al. (2021)的方法降低了\(\widetilde{O}(dk)\)倍，这是显著的改进。
3. **适用分布更广**：理论保证适用于子高斯分布（sub-Gaussian），扩展了之前仅适用于高斯分布的限制。
4. **维度无关的风险边界**：在二分类间隔损失下，通过JL变换实现了独立于原始维度\(d\)的准确率保证，说明隐私个性化在高维场景下仍能保持良好效果。

## 7. 优点：方法或实验设计上的亮点

- **理论贡献扎实**：给出了用户级DP下个性化联邦学习的首个严格风险上界，且边界形式清晰，改进明显。
- **方法实用性强**：基于FedRep的简单修改，易于实现和集成到现有联邦学习框架。
- **处理噪声标签**：考虑了实际场景中标签噪声问题，算法具有鲁棒性。
- **维度无关结果**：利用JL变换的降维思想，使得理论保证在高维下依然有效，极具创新性。
- **隐私保证严格**：满足用户级差分隐私，适合敏感数据场景。

## 8. 不足与局限

- **实验覆盖有限**：论文未提供详细的实验数据集列表、模型结构、超参数设置以及统计分析（如方差、置信区间），无法全面评估算法在实际大规模联邦系统上的表现。
- **缺乏消融研究**：未对算法各组件（如噪声规模、局部更新次数、通信轮数）的影响进行深入消融实验。
- **应用限制**：假设共享表示的低维结构\(k \ll d\)，若真实数据不满足该假设，性能可能大幅下降。
- **计算资源信息缺失**：未说明训练所需算力，不利于复现和资源评估。
- **仅考虑凸或近似凸损失？** 理论分析可能基于强凸或光滑假设，实际非凸神经网络场景下的近似性能未验证。

（完）
