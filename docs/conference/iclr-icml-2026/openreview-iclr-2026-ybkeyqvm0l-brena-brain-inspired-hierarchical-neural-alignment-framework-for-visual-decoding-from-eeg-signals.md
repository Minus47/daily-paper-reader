---
title: "BRENA: Brain-inspired Hierarchical Neural Alignment Framework for Visual Decoding from EEG Signals"
title_zh: BRENA：用于EEG视觉解码的类脑分层神经对齐框架
authors: "Yanan Zhu, Ziwei Xiang, Jiamin Wu, Jinyang Guo, Hongyuan Zhang, Chunfeng Song, Hongjian Fang, Qihao Zheng, Yufei Guo, Xuelong Li, Xianglong Liu"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=ybKEYQvM0L"
tags: ["query:eeg-align"]
score: 9.0
evidence: 通过脑启发的层级神经对齐框架，将区域级与全局EEG表征同时与视觉嵌入对齐
tldr: 面向EEG的视觉解码通常采用全局对齐，忽略视觉皮层不同区域对不同视觉信息的选择性。本文提出BRENA，同时将区域级和全局脑表征与视觉嵌入进行层级对齐，使解码更贴合大脑皮层结构。结果显示其在视觉解码任务上更加鲁棒和准确。该框架为EEG与图像等外部表征的结构化对齐提供了重要参考。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有视觉解码方法仅做全局脑-视觉对齐，未能刻画视觉皮层区域特异性选择。
method: 构建类脑分层对齐框架，同时优化区域级与全局EEG表征和视觉嵌入之间的匹配。
result: 相比全局对齐范式，脑启发层级对齐显著提升视觉解码的鲁棒性和准确性。
conclusion: 表明结合脑区特异性的层级对齐能更好地支持EEG信号与视觉语义的匹配。
---

## Abstract
Decoding human visual experiences from neural signals is crucial for understanding the relationship between brain activity and perceptual representations, driving the advancement of brain-computer interface (BCI) applications. Existing visual decoding methods typically adopt a global alignment paradigm for brain-visual alignment, which may not account for the human visual cortex’s region-specific selectivity where distinct cortical areas are selectively sensitive to different visual information. In this work, we propose BRENA, a BRain-inspired hiErarchical Neural Alignment framework by simultaneously aligning both region-level and global brain representations with visual embeddings for robust and accurate brain decoding.  Unlike prior approaches that rely purely on global pooled representations, BRENA proposes an adaptive local neural alignment module to explore fine-grained correspondence between brain channel features and visual semantic units, allowing for better exploitation of brain signals by modeling region-specific feature selectivity. Additionally, a set of perceptual weights are adaptively generated to guide more target-aware alignment. We further integrate a global neural alignment module, rendering hierarchical brain-visual alignment with complementary region-level and global neural patterns captured. Experiments demonstrate that BRENA not only outperforms existing methods across subjects and settings but also reveals region-level brain selectivity for visual stimuli through meaningful local mappings between brain channels and diverse visual patterns.

---

## 论文详细总结（自动生成）

由于提供的论文内容仅包含题目、作者、元数据和摘要，以下总结主要依据摘要信息展开，部分实验与算力细节在原文中未明确给出，将如实标注。

# 中文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：从 EEG 神经信号中解码人类视觉体验，是理解大脑活动与知觉表征关系的重要途径，也是推进脑机接口（BCI）应用的关键技术。
- **已有范式的问题**：现有视觉解码方法普遍采用“全局对齐”范式，即将 EEG 整体表征与视觉嵌入进行对齐，忽略了视觉皮层不同区域对不同视觉特征具有选择性（region-specific selectivity）。
- **核心问题**：如何让 EEG 与视觉嵌入的对齐过程更好地贴合大脑视觉皮层的结构化分区特性，从而实现更鲁棒、更准确的视觉解码。
- **整体意义**：提出了一个“类脑”且“分层”的对齐思路，为 EEG 信号与其他外部表征（如图像）间的结构对齐提供了重要参考。

## 2. 论文提出的方法论

- **框架名称**：BRENA（BRain-inspired hiErarchical Neural Alignment Framework），即类脑分层神经对齐框架。
- **核心思想**：同时将“区域级”脑表征与“全局”脑表征分别对齐到视觉嵌入中，形成层级式的对齐机制，以捕捉大脑皮层中不同尺度、不同功能性的神经响应模式。
- **关键模块**：
  - **自适应局部神经对齐模块**：不依赖全局池化表征，而是在“EEG 脑通道特征”与“视觉语义单元”之间寻找细粒度的对应关系，从而建模区域特异性特征选择性。
  - **感知权重机制**：自适应地生成一组感知权重，引导更“目标感知”（target-aware）的对齐。
  - **全局神经对齐模块**：用于捕捉全局神经模式，与局部对齐形成互补，最终实现区域级与全局模式的层级融合。
- **公式/技术细节**：摘录中未给出明确的数学公式、损失函数定义或完整算法流程，因此无法从当前文本中提取更具体的数值化实现细节。

## 3. 实验设计

- **数据集 / 场景**：摘要中仅提到方法在“不同受试者（across subjects）”和“多种设置（settings）”上进行了验证，但未具体说明使用了哪些 EEG 数据集、视觉刺激类型（如自然图像、类别图片等）。
- **Benchmark**：未明确列出基准数据集、对比基线或评价指标的细节。
- **对比方法**：仅笼统说明“优于现有方法（outperforms existing methods）”，未给出具体方法名称。
- **评估重点**：包括解码准确性、跨受试者/设置的稳定性，以及可视化脑通道与视觉模式之间的局部映射，用于验证区域级选择性。

## 4. 资源与算力

- **未明确说明**：原文摘要和元数据中均未提及使用的 GPU 型号、GPU 数量、训练时间、显存消耗等算力相关信息。
- 需要查看论文全文或附录才能了解具体训练资源。

## 5. 实验数量与充分性

- **已知实验类型**：从摘要推断，至少包含跨受试者实验、不同设置下的对比实验，可能还有对局部映射机制的可视化/定性分析。
- **缺失信息**：未报告每个实验组的数量、消融实验设计、统计显著性检验、误差棒或重复随机种子等。
- **充分性评判**：由于本总结仅依据摘要，无法全面评估实验覆盖面和公平性。摘要声称效果更优，但缺乏公开的基线细节和消融验证。实验量化方面暂时不足；如正文中提供更多内容，需要进一步判断其充分性。

## 6. 论文的主要结论与发现

- BRENA 同时利用区域级与全局神经表征进行分层对齐，相比传统纯全局对齐范式，在视觉解码任务上取得了更优的性能。
- 通过局部脑通道特征与不同视觉语义单元的映射，BRENA 能揭示大脑对视觉刺激的区域级选择性，使模型具有潜在的可解释价值。
- 实验结果支持了“脑启发层级对齐比单一全局对齐更适合 EEG 视觉解码”这一核心假设。

## 7. 优点

- **问题切入点有说服力**：针对现有“全局对齐”的盲区，引入神经科学中的皮层区域性选择概念，增加了模型设计的生理合理性。
- **层级对齐设计清晰**：局部分支与全局分支相互补充，既保留精细的通道–语义对应关系，又保留整体语义信息。
- **动态感知权重**：注意力式感知权重有助于引导模型关注与任务目标更相关的区域，增强对齐的目标针对性。
- **具有可解释性**：学习得到的局部映射能对应到脑区与视觉模式，有助于理解 EEG 信号背后的视觉加工机制。

## 8. 不足与局限

- **文本信息不完整**：连基本信息（具体公式、损失函数、模型架构图）都无法从摘要中提取，限制了复现与深入评估。
- **实验细节缺失**：数据集、基线方法、评测指标、跨受试者设置方式均未给出，难以判断公平性与应用范围。
- **未报告算力需求**：缺少模型复杂度、训练开销与推理效率的信息，无法判断其实际可落地程度。
- **潜在偏差风险**：如果仅展示最先进结果而无充分消融，无法确认每个模块（局部模块、全局模块、感知权重）的独立贡献以及可能存在的过拟合风险。
- **EEG 信号自身特性限制**：EEG 时空分辨率低、个体差异大，摘要未讨论如何通过预处理或正则化等策略缓解这些问题，实际场景下的泛化性仍需更充分验证。

（完）
