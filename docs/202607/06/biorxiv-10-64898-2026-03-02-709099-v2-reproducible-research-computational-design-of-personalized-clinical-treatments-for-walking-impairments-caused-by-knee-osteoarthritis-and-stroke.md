---
title: "Reproducible Research: Computational Design of Personalized Clinical Treatments for Walking Impairments Caused by Knee Osteoarthritis and Stroke"
title_zh: 可重复研究：针对膝骨关节炎和中风所致步行障碍的个性化临床治疗的计算设计
authors: "Salati, R. M., Li, G., Williams, S. T., Fregly, B. J."
date: 2026-06-30
pdf: "https://www.biorxiv.org/content/10.64898/2026.03.02.709099v2.full.pdf"
tags: ["query:pers-gen"]
score: 6.0
evidence: 用于临床治疗适应的个性化计算模型
tldr: 个性化计算模型可优化运动障碍治疗，但现有工具需要编程经验且缺乏完整培训材料。本文通过两个真实临床案例（膝骨关节炎和中风）的教程，展示使用NMSM Pipeline进行模型个性化和治疗优化的完整流程。教程成功生成了个性化模型并设计了达到目标（如降低膝内收力矩）的治疗方案。这是首个引导用户完成全过程的教学材料，可用于课程培训。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有工具难以复现研究且缺少完整培训，阻碍初学者掌握个性化模型和治疗优化的方法。
method: 采用NMSM Pipeline，针对膝骨关节炎和中风两个案例，实施模型个性化和治疗优化教程作为课程项目。
result: 成功生成个性化模型和跟踪优化，精确复现实验数据，并设计出达到目标力矩或冲量水平的治疗方案。
conclusion: 该教程首次提供从模型个性化到治疗设计的完整指导，降低学习曲线，可用于课程教学。
---

## 摘要
背景：个性化计算神经肌肉骨骼模型在优化运动障碍的临床治疗方案方面具有巨大潜力。尽管许多软件工具处理模型个性化和治疗优化过程中的特定部分，但它们通常需要丰富的编程经验才能使用，并且不能涵盖这两个过程的全部范围。此外，已发表的神经肌肉骨骼建模研究通常没有提供他人复制工作所需的所有方法细节。因此，由于缺乏使用真实受试者运动数据展示这两个过程以解决实际临床问题的详细培训材料，寻求在模型个性化和治疗优化过程中发展技能的研究人员面临着陡峭的学习曲线。

方法：本文通过两个实际临床问题和神经肌肉骨骼建模（NMSM）流程，提供了模型个性化和治疗优化过程的详细培训教程。第一个临床问题涉及为双侧内侧膝骨关节炎患者设计个性化步态修改和高位胫骨截骨手术，目标是降低双膝的峰值内收力矩至特定目标水平。第二个临床问题涉及为中风后步行功能受损患者设计基于协同的功能性电刺激处方，目标是使双腿之间的推进冲量和制动冲量相等。这两个教程均作为课程项目，提供给机械工程本科/研究生联合课程的新手用户。

结果：两个教程均生成了个性化的神经肌肉骨骼模型及相关的动态一致性跟踪优化，这些优化密切再现了受试者特定实验测量的步行过程中的关节角度、关节力矩、地面反作用力及力矩，以及（如适用）肌肉激活。随后的设计优化预测了个性化治疗方案，达到了峰值膝内收力矩或推进冲量和制动冲量的目标值。

结论：本文附带的详细分步教程首次引导用户完成创建个性化神经肌肉骨骼模型，并随后利用这些模型为两名患有不同临床问题的受试者设计个性化治疗方案的整个过程。这些教程可用于向新用户介绍NMSM流程，并作为神经肌肉骨骼建模课程的项目。

## Abstract
BackgroundPersonalized computational neuromusculoskeletal models have great potential for optimizing the design of clinical treatments for movement impairments. While many software tools address specific parts of the model personalization and treatment optimization processes, they typically require significant programming experience to use and do not cover the full breadth of these two processes. Furthermore, published neuromusculoskeletal modeling studies typically do not provide all the methodological details needed for others to reproduce the work. Consequently, researchers seeking to develop skills in the model personalization and treatment optimization processes face a steep learning curve due to the lack of detailed training materials that demonstrate both processes for real-life clinical problems using real-life subject movement data.

MethodsThis article presents detailed training tutorials for the model personalization and treatment optimization processes using two real-life clinical problems and the Neuromusculoskeletal Modeling (NMSM) Pipeline. The first clinical problem involves the design of personalized gait modifications and high tibial osteotomy surgery for an individual with bilateral medial knee osteoarthritis, where the goal is to reduce the peak adduction moment in both knees to a specified target level. The second clinical problem involves the design of a synergy-based functional electrical stimulation prescription for an individual post-stroke with impaired walking function, where the goal is to equalize the propulsive and braking impulses between the two legs. Both tutorials were implemented as course projects given to novice users in a combined undergraduate/graduate mechanical engineering course.

ResultsBoth tutorials produced personalized neuromusculoskeletal models and associated dynamically consistent tracking optimizations that closely reproduced subject-specific experimental joint angles, joint moments, ground reaction forces and moments, and (if applicable) muscle activations measured during walking. Subsequent design optimizations predicted personalized treatments that achieved target values of peak knee adduction moments or propulsive and braking impulses.

ConclusionsThe detailed step-by-step tutorials presented with this article are the first to walk users through the entire process of creating personalized neuromusculoskeletal models and then using them to design personalized treatments for two subjects with two different clinical problems. These tutorials can be used to introduce new users to the NMSM Pipeline and as projects in neuromusculoskeletal modeling courses.