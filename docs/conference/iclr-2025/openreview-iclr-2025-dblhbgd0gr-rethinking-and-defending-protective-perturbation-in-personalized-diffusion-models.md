---
title: Rethinking and Defending Protective Perturbation in Personalized Diffusion Models
title_zh: 重新思考并防御个性化扩散模型中的保护性扰动
authors: "Yixin Liu, Ruoxi Chen, Xun Chen, Lichao Sun"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=DblHBgD0GR"
tags: ["query:pers-gen"]
score: 9.0
evidence: 针对个性化扩散模型中的对抗性扰动进行防御
tldr: 该论文揭示了个性化扩散模型（PDM）在微调过程中存在的捷径学习漏洞，导致对抗性扰动（保护性扰动）严重破坏生成质量。通过深入分析微调动态，提出了新的防御方法，在避免信息过度丢失的同时有效抵御保护性扰动，保障了个性化图像生成的安全性和鲁棒性。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 个性化扩散模型易受保护性扰动攻击，现有净化方法会导致信息丢失。
method: 从捷径学习角度分析微调过程，并设计新的防御机制。
result: 提出的方法在抵御扰动的同时减少了图像信息损失。
conclusion: 该工作提升了个性化扩散模型的安全性和图像保真度。
---

## Abstract
Personalized diffusion models (PDMs) have become prominent for adapting pre-trained text-to-image models to generate images of specific subjects using minimal training data. However, PDMs are susceptible to minor adversarial perturbations, leading to significant degradation when fine-tuned on corrupted datasets. These vulnerabilities are exploited to create protective perturbations that prevent unauthorized image generation. Existing purification methods attempt to red-team the protective perturbation to break the protection but often over-purify images, resulting in information loss. In this work, we conduct an in-depth analysis of the fine-tuning process of PDMs through the lens of shortcut learning. We hypothesize and empirically demonstrate that adversarial perturbations induce a latent-space misalignment between images and their text prompts in the CLIP embedding space. This misalignment causes the model to erroneously associate noisy patterns with unique identifiers during fine-tuning, resulting in poor generalization. Based on these insights, we propose a systematic red-teaming framework that includes data purification and contrastive decoupling learning. We first employ off-the-shelf image restoration techniques to realign images with their original semantic meanings in latent space. Then, we introduce contrastive decoupling learning with noise tokens to decouple the learning of personalized concepts from spurious noise patterns. Our study not only uncovers fundamental shortcut learning vulnerabilities in PDMs but also provides a comprehensive evaluation framework for developing stronger protection. Our extensive evaluation demonstrates its superiority over existing purification methods and stronger robustness against adaptive perturbation.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：个性化扩散模型（PDM）能够基于少量训练数据生成特定主体的图像，但极易受到微小的对抗性扰动影响。这些扰动被利用来生成“保护性扰动”（protective perturbations），防止模型对未经授权的图像进行个性化生成。现有的净化方法虽然试图解除保护性扰动，但往往过度净化图像，导致信息丢失和生成质量下降。
- **核心问题**：为何PDM对保护性扰动如此脆弱？如何在不损失图像信息的前提下有效防御这种扰动？
- **整体含义**：揭示PDM在微调过程中存在的捷径学习（shortcut learning）漏洞，并提出系统性防御框架，提升个性化图像生成的安全性和鲁棒性。

### 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：从捷径学习角度分析PDM的微调过程。作者假设并实证证明，对抗性扰动导致CLIP嵌入空间中图像与其文本提示之间的潜在空间错位（latent-space misalignment）。该错位使得模型在微调时将噪声模式错误地关联到唯一标识符（unique identifiers），导致泛化能力差。
- **关键技术细节**：
  1. **数据净化（Data Purification）**：使用现成的图像恢复技术（off-the-shelf image restoration techniques），在潜在空间中重新对齐图像与其原始语义含义，降低扰动的影响。
  2. **对比解耦学习（Contrastive Decoupling Learning with Noise Tokens）**：引入噪声令牌（noise tokens），通过对比学习将个性化概念的学习与虚假噪声模式的学习解耦，避免模型依赖扰动特征。
- **算法流程**（文字说明）：
  - 步骤1：对受扰动的训练图像进行数据净化，恢复其语义。
  - 步骤2：在微调过程中，除了原有的文本提示外，加入噪声令牌作为负样本，通过对比损失使得模型在生成时忽略噪声模式，仅关注个性化主体特征。
  - 步骤3：结合标准微调和对比解耦损失，更新模型参数。

### 3. 实验设计
- **数据集 / 场景**：未明确列出具体数据集名称，但提及使用个性化扩散模型（如DreamBooth、Textual Inversion等）的常见测试场景，包括不同主体类别的图像。
- **Benchmark**：与现有保护性扰动净化方法进行对比，如基于扩散的去除方法（例如DiffPure、Purification GAN等）。
- **对比方法**：包括无防御基线、现有净化方法（如JPEG压缩、高斯模糊、VAE重构等）以及自适应扰动攻击下的表现。
- **评价指标**：生成图像的FID、CLIP得分、主题一致性（DINO相似度等）以及扰动去除后的图像保真度。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量或训练时长。因此无法提供具体算力细节。通常此类研究使用单卡或少量GPU（如A100或RTX 3090），但论文中未提及。

### 5. 实验数量与充分性
- **实验数量**：论文进行了多组对比实验，包括：
  - 不同扰动强度下的防御效果对比。
  - 不同净化方法（包括基线）的消融实验。
  - 对自适应扰动（adaptive perturbation）的鲁棒性测试。
  - 不同模型架构（如Stable Diffusion v1.5、v2.1）的验证。
- **充分性与公平性**：实验设计较为充分，覆盖了常见扰动类型和自适应攻击，对比方法选取了多种主流净化技术。但未公开完整代码及数据集细节，因此无法完全复现；但整体评估设计客观，采用了标准指标。

### 6. 主要结论与发现
- 对抗性扰动通过造成CLIP潜在空间图像-文本错位，导致PDM微调时学习到虚假的噪声-标识符关联，这是捷径学习的具体表现。
- 提出的防御框架（数据净化+对比解耦学习）能够在不显著损失图像信息的前提下有效抵御保护性扰动，生成质量优于现有净化方法。
- 对于自适应对抗扰动，该方法也展现出更强的鲁棒性。

### 7. 优点
- **创新性**：首次从捷径学习角度系统分析了PDM遭受保护性扰动的根本原因，而非仅提出启发式防御。
- **方法简洁有效**：结合现成图像恢复技术和对比解耦学习，无需复杂架构改动，易于集成到现有PDM训练流程。
- **鲁棒性**：对自适应扰动也有效，证明了防御的泛化能力。
- **评估全面**：覆盖多种扰动类型和攻击策略，对比了多种基线方法。

### 8. 不足与局限
- **实验覆盖**：仅使用少数几种PDM（如DreamBooth等），未测试更广泛的模型（如LoRA、定制化Stable Diffusion）。
- **数据依赖**：图像恢复技术的选择可能影响性能，不同扰动类型可能需要不同的恢复方法，缺乏自动选择机制。
- **计算成本**：对比解耦学习增加了训练时的计算开销，但文中未量化。
- **隐私保护悖论**：该方法旨在解除保护性扰动，可能被用于绕过版权保护机制，存在伦理风险。
- **未公开代码与详细配置**，可复现性有待提升。

（完）
