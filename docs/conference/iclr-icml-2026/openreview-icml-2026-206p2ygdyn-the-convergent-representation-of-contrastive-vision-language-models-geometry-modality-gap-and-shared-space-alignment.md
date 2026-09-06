---
title: "The Convergent Representation of Contrastive Vision-Language Models: Geometry, Modality Gap and Shared Space Alignment"
title_zh: 对比视觉语言模型的收敛表征：几何、模态间隙与共享空间对齐
authors: "Lingjie Yi, Raphael Douady, Chao Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1b1a84ee51bd83f710bafd9da992e8b854065837.pdf"
tags: ["query:eeg-align"]
score: 4.0
evidence: 理论分析对比多模态模型共享空间中的模态差距与对齐几何，可为EEG-行为模态对齐方法设计提供参考
tldr: 多模态对比学习试图把图像和文本嵌入同一空间，但实际中模态间常出现明显间隙且其对下游性能的影响未被厘清。作者为优化训练下的收敛最优表征建立了首个几何框架，证明模态间隙在何种条件下涌现，并刻画影响下游性能的关键几何因素。该理论不仅解释图像-文本共享空间的行为，也为EEG等信号与外部行为模态构建公共空间提供了可借鉴的建模原则。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多模态对比学习的理论缺失，图像-文本表征存在模态间隙且影响下游性能的机制不清楚，难以指导其他跨模态共享空间的设计。
method: 建立对比多模态学习收敛最优表征的几何分析框架，从优化与几何角度推导模态间隙成因及影响共享空间对齐性能的因素。
result: 严格证明了模态间隙在训练最优时出现，并给出决定下游性能的几何条件，统一解释了此前不一致的经验现象。
conclusion: 为理解多模态共享空间几何提供基础理论，可为EEG与语义或行为模态的对齐方法提供普适启发。
---

## Abstract
Multimodal contrastive learning (MCL) aims to embed data from two modalities in a shared embedding space. However, in practice, image and text representations occupy completely separated regions of the embedding space, a phenomenon called the modality gap. Meanwhile, empirical findings on how the modality gap affects downstream performance remain inconsistent. These observations motivate two key questions: (1) What causes the modality gap? (2) What determines downstream performance? To address these questions, we develop the first theoretical framework for analyzing the geometry of convergent optimal representations (COR) of MCL when training is optimized. We prove that the modality gap emerges when image and text representations collapse into different subspaces, a phenomenon called \emph{dimension collapse}. Our theory further reveals that although the modality gap prevents direct alignment between image and text representations, their projections onto the shared subspace can align. Moreover, we show that shared space alignment is a dominant factor in downstream performance, while the effect of the modality gap is limited. Inspired by these findings, we propose Shared Space Alignment (SSA) to improve MCL pretraining by enhancing alignment in the shared space without optimizing for modality gap reduction. Extensive empirical results validate our theoretical analysis and the proposed method.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：多模态对比学习（MCL）的目标是将两种模态（如图像与文本）嵌入到同一个共享向量空间中，以实现跨模态语义对齐。
- **核心问题**：
  1. 实践中图像与文本嵌入往往在空间中占据完全分离的区域，即“模态间隙”（modality gap），这一现象的原因尚不明确。
  2. 关于模态间隙如何影响下游任务性能，已有经验结果相互矛盾，缺乏统一理论解释。
- **研究意义**：论文首次尝试从理论角度回答“模态间隙为什么出现”以及“什么决定了多模态共享空间的下游性能”，为理解对比多模态模型的表示几何提供了基础框架，并可为其他跨模态对齐场景（如 EEG 与语义/行为模态）提供设计启发。

## 2. 论文提出的方法论

- **核心思想**：在训练优化的前提下，分析 MCL 的“收敛最优表示”（Convergent Optimal Representations, COR）的几何结构，将模态间隙与下游性能统一归因于共享空间中的子空间结构。
- **关键理论发现**：
  - 模态间隙的出现条件：当图像与文本表示分别塌缩到不同的子空间（即发生“维度塌缩” dimension collapse）时，模态间隙就会产生。
  - 模态间隙与可对齐性的关系：尽管模态间隙阻止了图像与文本表示的“直接”对齐，但二者在共享子空间上的投影仍然可以形成对齐。
  - 下游性能的决定因素：共享空间内的对齐程度（shared space alignment）是影响下游任务表现的主导因素，而模态间隙本身的影响有限。
- **提出的方法**：基于上述分析，作者提出一种改进 MCL 预训练的方法 **Shared Space Alignment (SSA)**，核心策略是增强共享子空间内的表示对齐，而**不直接优化/减小模态间隙**。
- **公式与算法流程**：摘要中未给出具体损失函数或算法伪代码，仅描述为一种训练策略；从摘要可推测其内部约束包含对共享子空间投影进行对齐的正则项或对比目标。

## 3. 实验设计

- **数据集与场景**：摘要仅说明进行了“大量的经验验证”（Extensive empirical results），未列出具体数据集名称（如 MSCOCO、Flickr30K、CLIP 基准等），也没有说明下游任务类型（如图像-文本检索、零样本分类等）。
- **Benchmark 与对比方法**：未提及基准数据集和对比方法（如 CLIP、ALIGN 及其他多模态对比学习变体）。
- **评估协议**：无法从中得知训练设置、微调方式或评测指标。

## 4. 资源与算力

- 论文摘要和给定元数据中**未包含任何算力信息**（例如 GPU 型号、数量、训练时长、显存消耗等）。
- 因此，无法对实验的资源开销进行总结或评估。

## 5. 实验数量与充分性

- 摘要仅用“大量”来描述实验，**没有给出具体实验组数**（例如数据集数量、消融实验次数、对比方法数量）。
- 由于缺少实验细节，**无法从当前信息独立评估实验的充分性、客观性与公平性**。需要查看完整论文中的实验设置和统计检验才能判断。

## 6. 论文的主要结论与发现

- **模态间隙的成因**：在训练最优时，模态间隙是由不同模态表示分别发生维度塌缩导致的。该结论以理论证明的方式给出。
- **模态间隙不是下游性能的直接决定因素**：模态间隙只表明不同模态位于不同子空间；真正影响性能的是它们在共享子空间中的投影是否对齐。
- **共享空间对齐（SSA）的可操作性**：通过增强共享子空间对齐可以提升 MCL 预训练质量，而无需强行消除模态间隙。
- **统一解释经验矛盾**：该理论能够同时解释先前关于模态间隙对下游性能影响不一致的观察，将分歧归因于是否测量了共享空间对齐。
- **潜在推广**：理论对一般跨模态共享空间具有指导意义，可启发如何为 EEG 等信号设计与行为/语义模态对齐的公共表示。

## 7. 优点

- **填补理论空白**：是首个针对 MCL 优化收敛后的表示几何进行严格分析的框架，将现象描述（模态间隙）上升到理论层面。
- **概念创新**：明确指出“维度塌缩”是模态间隙的几何根源，将“模态间隙”与“共享空间对齐”分离，提供了更细致的分析维度。
- **方法设计有依据**：SSA 方法直接来源于理论结论，属于“理论驱动”的模型改进，而非纯经验调参。
- **解释性强**：理论和经验结合，解释了已有文献中的矛盾结果，增加结果的可信度。
- **跨模态迁移价值**：结论为图像-文本之外的其他跨模态对齐（如 EEG-行为）提供了设计原则，具有前瞻性。

## 8. 不足与局限

- **实验信息严重缺失**：当前提供的文本中没有数据集、对比方法、评估细节和算力信息，无法验证结果的可复现性。
- **理论假设范围有限**：所有结论基于“训练优化的”收敛最优表示，未讨论实际训练中未完全收敛、次优解或动态优化路径对表征几何的影响。
- **模态覆盖面较窄**：主要面向图像-文本对比学习；能否直接推广到 EEG、行为等连续、高噪声、低信噪比的模态组合，仍需额外验证。
- **方法有效性证据不足**：摘要中声称 SSA 有效，但缺乏具体的性能数字或与基线模型的显著性比较。
- **应用落地的挑战**：未讨论实际中如何构建“共享子空间”并计算其投影，涉及维度选择、计算代价和优化稳定性等问题。

---

（完）
