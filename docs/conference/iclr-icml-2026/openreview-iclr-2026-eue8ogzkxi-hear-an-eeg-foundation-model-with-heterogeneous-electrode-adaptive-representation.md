---
title: "HEAR: An EEG Foundation Model with Heterogeneous Electrode Adaptive Representation"
title_zh: HEAR：具备异质电极自适应表示的EEG基础模型
authors: "Zhige Chen, Chengxuan Qin, Wenlong You, RUI LIU, Congying Chu, Rui Yang, KC Tan, Jibin Wu"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=eue8OGzkxI"
tags: ["query:eeg-align"]
score: 9.0
evidence: 面向异质电极布局的EEG基础模型，利用坐标空间嵌入学习通用神经表征
tldr: EEG设备导联数与布局各异，增加了基础模型推广难度。HEAR首次显式支持异质电极设备，用可学习的坐标空间嵌入将不同导联布局映射到统一表征空间，并配合大规模预训练获得跨任务、跨受试者的通用EEG表征。该设计为后续多模态脑信号对齐提供了稳定的异质EEG编码底座。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG设备电极布局与数量异构，阻碍了统一EEG基础模型的规模化与推广。
method: HEAR引入基于坐标的可学习空间嵌入，将任意导联布局的EEG映射到统一共享表征空间。
result: 在多种异质设备与下游任务上展现出稳健的通用EEG表征与泛化性能。
conclusion: 显式建模电极几何布局可实现设备无关的EEG基础模型，支撑大规模应用。
---

## Abstract
Electroencephalography (EEG) is an essential technique for neuroscience research and brain-computer interface (BCI) applications. Recently, large-scale EEG foundation models have been developed, exhibiting robust generalization capabilities across diverse tasks and subjects. However, the heterogeneity of EEG devices not only hinders the widespread adoption of these models but also poses significant challenges to their further scaling and development. In this paper, we introduce HEAR, the first EEG foundation model explicitly designed to support heterogeneous EEG devices, accommodating varying electrode layouts and electrode counts. HEAR employs a learnable, coordinate-based spatial embedding to map electrodes with diverse layouts and varying counts into a unified representational space. This unified spatial representation is then processed by a novel spatially-guided transformer, which effectively captures spatiotemporal dependencies across electrodes. To support the development of HEAR, we construct a large-scale EEG dataset comprising 8,782 hours of data collected from over 150 distinct electrode layouts with up to 1,132 electrodes. Experimental results demonstrate that HEAR substantially outperforms existing EEG foundation models in supporting heterogeneous EEG devices and generalizing across diverse cognitive tasks and subjects.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- 脑电图（EEG）是神经科学研究和脑机接口应用中的关键无创脑信号采集技术。
- 近年来，大规模 EEG 基础模型已在跨任务、跨受试者泛化方面展现出较强能力。
- 然而，不同 EEG 设备之间**电极布局和电极数量差异巨大**（异质性），这使得同一个基础模型难以直接适配不同设备，严重阻碍了模型的规模化训练、部署和跨场景推广。
- 本文提出 **HEAR**（Heterogeneous Electrode Adaptive Representation），宣称是**首个显式支持异质电极设备的 EEG 基础模型**，旨在统一建模任意电极布局和电极数目的 EEG 数据，为跨任务、跨受试者、跨设备的通用神经表征奠定基础。

---

## 2. 方法论

- **核心思想**：将不同设备的电极几何布局统一映射到一个共享表征空间中，使模型能够摆脱对特定导联配置的依赖。
- **关键技术与流程**：
  1. 输入为任意 EEG 设备记录的原始多导联信号。
  2. 采用**可学习的、基于坐标的空间嵌入**：针对每个电极的三维空间位置（或二维坐标）学习一个嵌入向量，从而将不同布局、不同数量的电极映射到统一的空间表征。
  3. 该统一空间表征被送入一个**空间引导的 Transformer（spatially-guided transformer）**，用于建模电极之间的时空依赖关系。
  4. 预训练阶段在大规模异质 EEG 数据上进行，学习通用的表征；下游任务可在此基础上进行微调或线性探测。
- 摘要中未给出具体公式、损失函数或模型架构的超参数细节，因此算法流程仅能作如上文字性说明。

---

## 3. 实验设计

- **数据集构建**：作者构造了一个大规模 EEG 预训练数据集，包含：
  - 共 **8,782 小时**的 EEG 数据；
  - 来自 **超过 150 种不同电极布局**；
  - 单次记录最多包含 **1,132 个电极**。
- **评估场景**：摘要提及在“多种异质设备”和“多样认知任务与受试者”上评估泛化性能，但未明确列出具体任务名称（如情绪识别、运动想象、睡眠分期等）和基准数据集。
- **对比方法**：摘要仅笼统提到“现有 EEG 基础模型”，未具体给出比较的模型名称、是否同参数量级、训练协议是否一致等关键细节。
- **评测指标**：未在摘要中报告具体准确率或提升幅度。

---

## 4. 资源与算力

- 论文摘要和元数据中**均未提及**所使用 GPU 型号、数量、训练时长、显存消耗或预训练成本等任何算力相关信息。
- 因此无法对方法的计算效率、训练可复现性进行量化评估。

---

## 5. 实验数量与充分性

- 由于可获得内容仅为摘要，可见的实验描述非常有限：
  - 仅说明了构造了大规模数据及一个总体的性能优势结论；
  - **未报告任何消融实验**（如空间嵌入的影响、Transformer 设计的替代选择）；
  - **未提供跨设备的迁移实验细节**；
  - **未展示与基线方法的统计显著性检验**或误差条。
- 从摘要本身判断，实验设计较难被充分评估。其宣称的大规模数据和优越性能需要阅读原文图表、实验设置后，方可判断客观性与公平性。

---

## 6. 主要结论与发现

- HEAR 能够有效支持异构 EEG 设备，适应不同的电极布局和电极数量。
- 基于坐标的可学习空间嵌入 + 空间引导 Transformer 可以获得统一的时空表征，进而在多个认知任务和受试者上表现出优于现有 EEG 基础模型的泛化能力。
- 作者认为显式建模电极几何布局是实现“设备无关”EEG 基础模型的关键路径，可为后续大规模脑机接口和多模态脑信号对齐研究提供稳定的异质 EEG 编码底座。

---

## 7. 优点

- **问题切入点新颖**：现有 EEG 基础模型通常假设固定导联配置，HEAR 首次从模型架构层面显式解决了电极异质性这一真实世界中的核心痛点。
- **方法设计直觉性强**：利用电极的三维/二维坐标进行可学习空间嵌入，将几何结构注入模型，物理可解释性较好。
- **模型架构具有实用性**：能够处理“任意”布局和“可变”电极数量，显著提高了模型的泛化能力和应用便利性。
- **数据工程贡献显著**：构建了超 8,000 小时、150+ 种电极布局的大规模预训练数据集，本身具有较大的科研与工程价值。
- **潜在延展性好**：这类统一空间表征有望作为多模态脑信号统一模型的通用前端。

---

## 8. 不足与局限

- **技术细节缺失**：摘要未给出空间嵌入的具体编码方式（如坐标归一化、插值策略）、Transformer 如何整合不规则电极位置、以及针对可变电极数量是否采用 mask 或 padding 等关键实现。
- **实验证据不足**：缺少具体任务清单、基线模型配置、详细的性能对比结果，无法验证其声称的“大幅超越”是否在不同设置下均成立。
- **缺乏消融与鲁棒性分析**：未说明坐标噪声、电极缺失或布局稀疏时模型的退化表现，而这些在真实 EEG 设备更换、接触不良场景中很重要。
- **数据偏见风险**：数据虽包含 150+ 种布局，但可能仍以某种标准布局为主，模型对极少数特殊布局的泛化能力有待检验。
- **应用范围受限**：目前只针对 EEG 模态，未讨论对 MEG、fNIRS 等其他神经信号的兼容性；作为基础模型，也未见多任务联合训练或零样本评估证据。
- **评审状态提示**：该论文标注为 ICLR 2026 Rejected，摘要中的优势声明可能被审稿人质疑，但由于公开信息有限，此处不展开推测具体审稿意见。

---

**总结而言**：HEAR 提出的异质电极自适应思路具有很强的现实价值，但当前可见摘要信息不足以全面评估其方法的有效性、实验严谨性与实际贡献，需要结合全文和复现实验进一步综合判断。

（完）
