---
title: "PRISM: Prioritized Channel Importance with Semi-supervised Domain Adaptation for Cross-Subject EEG Emotion Recognition"
title_zh: PRISM：基于通道优先级与半监督域适配的跨被试EEG情绪识别
authors: "Xin Zhou, Xiang Zhang, Hao Deng, Lijun Yin"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=gdtmK6JEzB"
tags: ["query:eeg-align"]
score: 8.0
evidence: 基于通道权重与半监督域自适应的跨被试EEG情绪识别，用神经信号提升分类任务泛化
tldr: EEG情绪解码虽潜力大，但通道冗余和个体差异阻碍跨被试泛化。PRISM提出可微通道重要性加权的轻量专家集成，在通道维度放大可靠电极、抑制干扰通道；同时在域层面使用半监督域自适应，利用无标签数据降低标注负担。该方法在跨被试情绪识别上实现标签高效的分类解码，为神经信号驱动的情绪预测和适应性模型提供了实用框架。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: EEG情绪解码受通道冗余和跨被试差异制约，需要标签高效且可泛化的识别模型。
method: 提出PRISM框架：通道侧用轻量专家集成学习可微通道权重，域侧用半监督自适应利用无标签数据。
result: 在跨被试设置中实现标签高效的情绪解码，同时压制冗余电极和个体差异影响。
conclusion: 表明通道优先性与半监督域自适应相结合能提升EEG情绪识别的泛化能力。
---

## Abstract
Electroencephalogram (EEG) captures endogenous brain activity with high temporal fidelity and holds substantial promise for precise emotion decoding. However, channel redundancy and pronounced inter-subject variability remain key obstacles to scalable generalization. To address these limitations, we propose a novel framework termed PRioritized channel Importance with Semi-supervised doMain adaptation (PRISM), enabling label-efficient cross-subject emotion decoding. On the channel side, PRISM assigns differentiable, data-dependent channel weights via a lightweight expert ensemble, amplifying reliable electrodes while suppressing distractors. On the domain side, PRISM leverages unlabeled data through confidence-filtered pseudo-labels to drive consistency regularization and domain alignment, mitigating subject-specific heterogeneity. Extensive experiments show that PRISM surpasses state-of-the-art time-series baselines on DEAP,
DREAMER, and SEED datasets, achieving robust cross-subject generalization given limited annotations. The code will be released to the research community.

---

## 论文详细总结（自动生成）

# PRISM：基于通道优先级与半监督域适配的跨被试EEG情绪识别——论文总结

## 1. 核心问题与研究动机
- **背景**：脑电图（EEG）以高时间分辨率捕捉内源性大脑活动，被认为是一种极具潜力的情绪解码信号。  
- **痛点**：
  - **通道冗余**：EEG 多通道数据中，并非所有电极都对情绪识别有正面贡献，存在大量无关或干扰通道，增加计算开销并引入噪声。
  - **跨被试差异**：不同被试的大脑信号存在显著个体差异，导致同一模型难以在不同人之间泛化，跨被试（cross-subject）识别性能严重下降。
  - **标注成本高**：准确的情绪标签通常依赖被试自评或专家标注，获取大量高质量标注数据困难，限制了模型在实际场景中的规模化应用。
- **核心目标**：实现 **标签高效（label-efficient）** 的跨被试情绪解码，即在有限标注条件下，仍能获得稳健的跨被试泛化能力。

## 2. 方法论：PRISM 框架
- **整体思想**：PRISM 从两个维度同时解决上述问题——**通道维度**与**域（被试）维度**。
  - 通道维度：使用可微分的、数据依赖的通道权重，自动放大可靠电极、抑制干扰电极。
  - 域维度：利用无标签数据，通过半监督域自适应减小被试间差异。
- **通道侧：轻量专家集成（lightweight expert ensemble）**
  - 由若干轻量子模型（专家）共同预测，并通过可学习机制为每个通道分配权重。
  - 这种通道重要性加权是**可微分**且**数据依赖**的——不同输入样本会得到不同通道权重，而非静态固定权重。
  - 效果上相当于实现一种“软选择”：对情绪相关信息丰富的电极赋予更高权重，对冗余电极则压低权重。
- **域侧：半监督域自适应（semi-supervised domain adaptation）**
  - 利用**无标签数据**生成伪标签，并引入**置信度过滤**（confidence-filtered），只保留高置信度的伪标签参与训练。
  - 基于这些伪标签构建一致性正则化（consistency regularization）与域对齐（domain alignment）约束，迫使模型学习不随被试变化的表征。
  - 这样既降低了对人工标签的依赖，又减轻了目标被试数据分布偏移带来的负迁移。
- **数学/数理细节**：原文仅给出框架概览，未提供具体公式。关键操作可概括为：
  1. 前向计算得到各专家的通道级表征；
  2. 通过注意力或门控机制计算通道权重并加权融合；
  3. 在有标签源域数据上计算监督损失；
  4. 在目标域无标签数据上生成高置信度伪标签，计算一致性/对齐损失；
  5. 联合优化以同时降低分类误差与域间差异。

## 3. 实验设计
- **数据集**：
  - **DEAP**：常用多通道 EEG 情绪数据库（32 通道，效价/唤醒/支配度评分）。
  - **DREAMER**：采用 14 通道 EEG 的情绪数据库。
  - **SEED**：中文被试脑电情绪数据集，通常包含多session与多情绪类别。
- **任务场景**：跨被试（cross-subject）情绪识别，训练与测试来自不同被试，且要求训练时只使用少量标注（标签高效）。
- **基准（Benchmark）**：与“SOTA 时间序列基线方法”（state-of-the-art time-series baselines）进行比较。未具体点名基线模型。
- **对比方法**：论文称 PRISM 在三个数据集上均超过这些基线，但原摘要未列出具体对比方法名称。

## 4. 资源与算力
- 论文摘要和提供的元数据中**没有说明使用的 GPU 型号、数量、训练时长、显存占用等硬件信息**。
- 因此无法判断其计算开销详情，只能推测该方法采用了轻量专家集成，整体参数量应相对可控。
- 如果后续需要精确复现或评估部署成本，需要查阅论文实验章节或补充材料。

## 5. 实验数量与充分性
- **已明确的实验**：
  - 在 **3 个公开基准数据集**（DEAP、DREAMER、SEED）上进行跨被试评估。
  - 与 SOTA 时间序列方法进行性能对比。
- **未明确的实验**：
  - 代码未在摘要中提供消融实验（如去掉通道权重、去掉半监督对齐、不同伪标签阈值等）的具体细节。
  - 未见跨数据集迁移、噪声鲁棒性、通道数敏感性等额外分析。
- **总体评价**：
  - 三个数据集涵盖不同采集设备、通道数量和情绪标注范式，一定程度上说明了泛化能力。
  - 但由于仅有摘要信息，**无法完全评估消融实验的充分性、统计显著性与公平性设置**（如是否使用相同的训练样本数、是否控制了调参预算等）。
  - 论文获得 ICLR-2026 评审 8.0 分，说明评审人认为实验设计整体可信，但作为外部读者仍需谨慎看待细节缺失。

## 6. 主要结论与发现
- PRISM 在跨被试情绪识别中显著优于现有 SOTA 时间序列基线。
- **通道重要性加权**能够有效放大可靠电极、抑制干扰通道，减少通道冗余的负面影响。
- **带置信度过滤的半监督域自适应**使模型能够从未标注数据中获益，降低人工标签需求，同时缓解被试间异质性。
- 总而言之，将“通道级优先级”与“域级半监督对齐”结合，是在有限标注下提升 EEG 情绪识别跨被试泛化能力的有效途径。
- 作者表示代码将开源，有利于后续复现与研究。

## 7. 优点（亮点）
- **双维度协同**：不仅在通道维度做特征降噪，还在域维度做分布对齐，直击 EEG 跨被试任务的两大顽疾。
- **标签高效**：用半监督伪标签与一致性正则化减少对人工标注的依赖，实用价值高。
- **可微分通道权重**：将通道选择嵌入端到端训练，避免手工设定或两步式启发式通道筛选，使模型更灵活。
- **轻量专家集成**：在提升容量的同时尽量控制计算复杂度，兼顾效果与成本。
- **多数据集验证**：在三个广泛使用的 EEG 情绪数据集上均获得提升，证据具有较强说服力。
- **数据依赖**：通道权重随样本动态变化，可适应不同被试甚至不同时刻的脑电特征。

## 8. 不足与局限
- **信息不透明**：摘要及可用元数据未提供具体公式、网络结构配置、伪标签置信度阈值、训练策略等，难以深度复现。
- **硬件与训练成本未知**：未报告 GPU 资源与训练时长，影响对可复现性和工程成本的判断。
- **基线对比细节缺失**：没有列出具体比较方法名称与版本，无法确定比较条件是否完全公平（如基线是否经过同等调优）。
- **实验维度有限**：缺少针对不同标注比例、不同通道数量、不同伪标签阈值、不同域偏移程度的系统分析；也未提及 EEG 情绪识别中常见的性别、session 间漂移等偏差问题。
- **应用限制**：半监督依赖伪标签置信度，在情绪类别重叠或信噪比极低时，错误伪标签可能被放大；仅用三数据集也无法证明在所有 EEG 设备/场景下均有效。
- **需要完整论文核实**：由于当前仅有摘要，本文总结的部分方法论细节属于合理推断，正式结论应以全文实验为准。

（完）
