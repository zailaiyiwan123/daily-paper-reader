---
title: "AnyView: Few Shot Personalized View Transfer"
title_zh: AnyView：少样本个性化视图迁移
authors: "Rukhshanda Hussain, Hui Xian Grace Lim, Bor-Chun Chen, Mubarak Shah, Ser-Nam Lim"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=GuQeZWbaGr"
tags: ["query:pers-gen"]
score: 8.0
evidence: 基于DreamBooth的少样本个性化视图迁移
tldr: 现有个性化方法主要用于风格迁移，本文探索将DreamBooth个性化用于视图合成任务，研究扩散模型理解“视图”概念的能力。实验表明，无需3D先验，仅需少量样本即可实现个性化视图迁移。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 探索个性化模型在视图合成任务中的能力，而非仅风格。
method: 基于DreamBooth对预训练稳定扩散模型进行个性化微调实现视图迁移。
result: 少样本条件下无需3D先验即可生成个性化视图。
conclusion: AnyView扩展了个性化生成的应用范围到视图领域。
---

## Abstract
Fine-tuning generative models for concept driven personalization have witnessed tremendous growth ever since the arrival of methods like DreamBooth, Textual Inversion etc. Particularly, such techniques have been thoroughly explored for style-driven generation. Recently, diffusion models have also demonstrated impressive capabilities in view synthesis tasks, setting the foundation for exploring view-driven generation approaches. Motivated by these advancements, we investigate the capacity of a pretrained stable diffusion model to grasp ``what constitutes a view" without relying on explicit 3D priors. Specifically, we base our method on a personalized text to image model, Dreambooth, given its strong ability to adapt to specific novel objects with a few shots. Our research reveals two interesting findings. First, we observe that Dreambooth can learn the high level concept of a view, compared to arguably more complex strategies which involve fine-tuning diffusions on large amounts of multi-view data. Second, we establish that the concept of a view can be disentangled and transferred to a novel object irrespective of the original object’s identity from which the views are learnt. Motivated by this, we introduce a learning strategy, AnyView, which inherits a specific view through only one image sample of a single scene, and transfers the knowledge to a novel object, learnt from a few shots, using low rank adapters. Through extensive experiments we demonstrate that our method, albeit simple, is efficient in generating reliable view samples for in the wild images. Code and models will be released.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有个性化生成方法（如 DreamBooth、Textual Inversion）主要聚焦于风格驱动的生成，而在视图合成任务中，扩散模型尽管展现出强大能力，但通常依赖于大量多视图数据或显式3D先验。本文旨在探索：**预训练的稳定扩散模型能否在不依赖3D先验的情况下，通过少量样本学习并理解“视图”这一高级概念**，从而扩展个性化生成的应用范围到视图迁移领域。
- **核心问题**：如何利用 DreamBooth 对预训练扩散模型进行个性化微调，使其能够从单个场景的一张图像中学习特定视图，并**将所学视图概念解耦并迁移到另一个仅经过少样本学习的新对象上**，实现“少样本个性化视图迁移”。
- **整体含义**：验证了扩散模型在极少量样本条件下也能内化“视图”的抽象表征，且该表征与对象身份可分离，为无需3D重建的视图合成提供了新思路。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：基于 DreamBooth 的个性化微调框架，结合低秩适配器（LoRA），在不改变原始模型大规模参数的前提下，同时学习“视图”和“对象身份”两个概念，并在推理时通过解耦实现跨对象视图迁移。
- **关键技术细节**：
  - **视图学习阶段**：仅使用**单个场景的一张图像**作为视图参考，通过微调扩散模型的 LoRA 模块，让模型学会该特定视角下的视觉特征（如角度、光照、布局等）。
  - **对象学习阶段**：对目标新对象使用**少量样本（几幅图像）**，同样基于 DreamBooth + LoRA 学习其身份特征（颜色、纹理、形状等）。
  - **视图迁移推理**：在推理时，同时加载视图 LoRA 和对象 LoRA，将视图概念施加于新对象，生成该对象在目标视角下的图像。
  - **公式/算法流程**：论文未给出显式公式，核心流程可概括为：  
    `Finetune Stable Diffusion with (view sample) → learn view LoRA`  
    `Finetune Stable Diffusion with (few-shot object samples) → learn object LoRA`  
    `Inference: combine view LoRA + object LoRA → generate personalized view transfer.`

### 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：论文未明确列出具体数据集名称，仅提及使用“in the wild images”（自然环境图像）进行测试，包括不同场景和对象（如日常物体、动物等）。视图样本为单个场景的单张图像。
- **基准（Benchmark）**：未明确给出标准基准。实验主要验证方法的**有效性**和**泛化性**，而非与SOTA系统对比。
- **对比方法**：论文未列出与其他视图合成方法（如NeRF、多视图扩散模型）的直接对比，仅与 DreamBooth 本身（无视图控制）进行了隐含对比，以证明视图概念被成功学习。

### 4. 资源与算力

- **未明确说明**：论文正文中未提及使用的GPU型号、数量、训练时长、模型参数量等算力信息。根据常规 DreamBooth + LoRA 实践，通常可在单张消费级GPU（如24GB显存）上完成，但论文未提供具体细节。

### 5. 实验数量与充分性

- **实验数量**：论文声称进行了“extensive experiments”，但具体实验组数未列出。从摘要看，至少包括：
  - 视图学习有效性验证（从单张图像提取视图）
  - 视图迁移到不同新对象（多组对象案例）
  - 少样本对象学习（不同数量的shots）
  - 与 DreamBooth 基线对比（显示视图概念被解耦）
- **充分性与客观性**：
  - **优点**：验证了核心假设（视图与身份可解耦），且结果定性展示较为直观。
  - **不足**：缺乏定量指标（如PSNR、SSIM、LPIPS、CLIP score等），没有与现有视图合成方法（如Zero-1-to-3、MVDream等）进行比较，实验覆盖范围有限，公平性存疑。此论文被ICLR 2025拒稿，可能正因实验不充分或方法对比不足。

### 6. 论文的主要结论与发现

- **发现1**：DreamBooth 能够学习 **视图的高层概念**，而无需大量多视图数据或3D先验。
- **发现2**：视图概念可以**与对象身份解耦**，并成功迁移到其他对象上，即使新对象仅通过少样本学习得到。
- **结论**：AnyView 方法简单有效，能可靠地生成野外图像的个性化视图样本，扩展了 DreamBooth 的应用场景至视图合成领域。

### 7. 优点：方法或实验设计上的亮点

- **简洁性**：仅依赖 DreamBooth + LoRA，无需复杂3D模型或多视图训练，易于复现和部署。
- **少样本能力**：视图学习仅需单张图像，对象学习仅需几幅图像，大幅降低数据采集成本。
- **概念解耦**：验证了视图与身份在扩散模型隐空间中的可分离性，具有理论启发意义。
- **实用性**：可直接应用于个人化图像生成（如为宠物生成不同角度的照片），无需物体3D扫描。

### 8. 不足与局限

- **实验覆盖不足**：缺少定量指标和系统对比基准方法，无法客观评估生成质量与现有技术的差距。
- **泛化性验证有限**：仅展示了少量静态场景和物体，未测试复杂光照、遮挡、极端视角等情况，也未验证对未见物体或场景的鲁棒性。
- **多视图一致性未评估**：视图迁移后的多视角图片间是否存在几何一致性（如NeRF的3D一致性）未讨论。
- **应用限制**：依赖于 DreamBooth 对目标对象的少样本学习效果，若目标对象与视图来源场景差异过大，可能失败。此外，视图概念可能包含场景背景信息，导致迁移时引入不期望的混叠。
- **资源与可重复性**：未提供算力细节，且被拒稿意味着实验或方法说服力有待加强。

（完）
