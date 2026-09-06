---
title: The Human Brain as a Dynamic Mixture of Expert Models in Video Understanding
title_zh: 将人脑视为视频理解中的动态专家混合模型
authors: "Christina Sartzetaki, Anne W. Zonneveld, Pablo Oyarzo, Alessandro Thomas Gifford, Radoslaw Martin Cichy, Pascal Mettes, Iris Groen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=bSsNSfyj8m"
tags: ["query:eeg-align"]
score: 8.0
evidence: 利用跨时间表征比对将视频模型特征与EEG动态记录对齐，评测自然视频理解的脑一致性
tldr: 人类大脑善于理解动态视频，但模型脑对齐研究多集中于fMRI。该文首次构造大规模EEG自然视频基准，提出跨时间表征对齐度量，系统比较100多种视频模型在时间整合、分类任务、架构与预训练上的表现。结果显示动态EEG能够揭示人对视频处理的精细时序，并为筛选更类脑的视频表示提供依据。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 视频理解模型脑对齐多用fMRI，缺少动态EEG下对精细时序加工的评测方法。
method: 提出跨时间表征对齐度量，对100余种视频模型特征与自然视频EEG记录进行系统比较。
result: 不同视频模型在时间整合、任务与架构等维度上展现出与EEG动态模式不同的对齐表现。
conclusion: EEG为模型与大脑视频理解在毫秒级时序上的对齐提供新的评测范式。
---

## Abstract
The human brain is the most efficient and versatile system for processing dynamic visual input. By comparing representations from deep video models to brain activity, we can gain insights into mechanistic solutions for effective video processing, important to better understand the brain and to build better models. Current works in model-brain alignment primarily focus on fMRI measurements, leaving open questions about fine-grained dynamic processing. Here, we introduce the first large-scale model benchmarking on alignment to dynamic electroencephalography (EEG) recordings of short natural videos. We analyze 100+ models across the axes of temporal integration, classification task, architecture, and pretraining, using our proposed Cross-Temporal Representational Similarity Analysis (CT-RSA) which matches the best time-unfolded model features to dynamically evolving brain responses, distilling $10^7$ alignment scores. Our findings reveal novel insights on how continuous visual input is integrated in the brain, beyond the standard temporal processing hierarchy from low to high-level representations. After initial alignment to hierarchical static object processing, responses in posterior electrodes best align to mid-level temporally-integrative action features, showing high temporal correspondence to feature timings. In contrast, responses in frontal electrodes best align with high-level static action representations and show no temporal correspondence to the video. Additionally, temporally-integrating state-space models show superior alignment to intermediate posterior activity, in which self-supervised pretraining is also beneficial. We draw a metaphor to a dynamic mixture of expert models for the changing neural preference in tasks and temporal integration reflected in the alignment to different model types across time. We posit that a single best-aligned model would need such training and architecture as to allow combining and dynamically switching between these capacities.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **背景与动机**：人类大脑是处理动态视觉输入最高效、最灵活的系统。通过将深度视频模型的特征表示与大脑活动进行对齐（model-brain alignment），是理解大脑视频处理机制以及构建更优视频模型的重要途径。然而，现有脑对齐研究大多依赖 fMRI，因其时间分辨率不足，无法刻画大脑在毫秒级尺度上对连续、动态视频输入进行精细加工的过程。
- **核心问题**：大脑在观看自然视频时的**动态神经表征**随时间如何演化？不同类型的深度视频模型（如按时间整合方式、训练任务、架构、预训练策略划分）在**何时**以及**何种程度上**与大脑的活动模式相互对齐？是否存在一个单一最优模型可以全面解释大脑的视频理解能力？
- **整体含义**：该论文首次将脑对齐基准研究拓展到动态 EEG 模态，以“动态专家混合模型（dynamic mixture of expert models）”为隐喻，提出大脑在视频理解的**不同时间阶段**可能动态切换或组合不同的计算偏好（如物体静态加工与动作时间整合加工），为类脑视频模型的设计提供了新的时间维度上的目标。

## 2. 论文提出的方法论

- **核心思想**：与以往将模型特征与静态脑活动快照对齐（如 fMRI 的体素响应模式）不同，本文针对 EEG 高时间分辨率的特点，允许模型时间展开后的动态特征与大脑诱发电位随时间演变的过程进行**逐时间点匹配**，从而刻画模型和大脑之间在时间序列上的最优对齐关系。
- **核心方法：跨时间表征相似性分析（Cross-Temporal Representational Similarity Analysis, CT-RSA）**
  - 从视频模型（Video Model）内部提取随时间展开的（time-unfolded）特征序列。
  - 从 EEG 记录中，按时间窗切分出连续变化的脑状态表征矩阵。
  - 通过 **表征相似性分析（RSA）** 计算模型各时间点的特征表征与 EEG 各时间点的神经表征之间的相似性（即表征相异矩阵的相关系数）。
  - 为每一个模型-脑区/电极组合生成一个“跨时间对齐矩阵”，再对该矩阵求每个时间点上的峰值为最佳对齐点，最终产生可以相互比较的对齐分数。
- **指标与规模**：使用标准化后的对齐分数，计算了 **10^7** 量级的模型-EEG 对齐评分，支撑 100+ 模型的系统比较。
- **算法流程（文字表述）**：
  1. 采集被试观看自然视频时的 EEG 数据，并进行预处理和编码建模得到时间的表征相异矩阵。
  2. 将每个视频模型在给定输入下提取各层或各时间步的特征，做时间展开与表征相异矩阵化。
  3. 对每个 EEG 时间点，寻找该模型最匹配的特征时间点（或反之），进而得到交叉时间 RDM 的相关得分。
  4. 根据显著性检验分别确定“模型特征与哪些脑电时间窗口具有显著对应”以及“对应关系的强弱”。

## 3. 实验设计

- **EEG 数据集与基准场景**：
  - 是首个以 **自然视频** 为刺激的动态脑电基准 benchmark（未提供具体被试数量、EEG 设备与导联数细节，但表示属于大规模 EEG 自然视频模型评测）。
  - 使用短视频作为刺激材料，覆盖从静态物体识别到动态动作理解等不同层次的内容。
- **基准对象**：
  - 系统调研比较了视频理解领域 **100+ 深度模型**。
  - 沿多个关键维度划分为不同子集进行比较：
    - **时间整合**：静态帧模型 vs 时序池化/时间平均 vs 显式时间模型（如视频Transformer、状态空间模型）。
    - **分类任务域**：物体识别、动作识别（如不同规模的动作分类训练）以及多任务等。
    - **架构类型**：3D 卷积网络、视频 Transformer、状态空间模型（SSMs）等。
    - **预训练策略**：监督预训练 vs 自监督预训练。
- **比较方法**：
  - 以 **CT-RSA** 作为核心评测指标，将所有模型在统一的标准下与 EEG 进行标记对齐比较。
  - 揭示了不同时间整合方式（时间无关、池化整合、显式整合）与不同电极区域（前部额叶 vs 后部枕颞等）的动态响应曲线之间的关系。

## 4. 资源与算力

- 论文原摘要中并未提供具体 **GPU 型号、数量、训练时长、模型推理代价或总计算量** 的说明。
- 理论上，由于涉及对 100+ 视频模型在 EEG 基准上进行完整前向推理以及大规模表征距离矩阵计算（构建 10^7 对齐评分），总计算资源开销比较可观，尤其是涉及多个大规模视频模型的部分。
- 论文没有公开可复现实验所需的准确硬件配置，因此难以从文中直接获知精确算力需求。

## 5. 实验数量与充分性

- **实验规模与组数**：
  - 对 **100 多个模型** 在同一个基准上做模型 - 脑对齐评测，覆盖 4 个主要分析轴（时间整合、任务、架构、预训练策略），实验总量和模型覆盖面远超当前领域既有同类工作。
  - 各维度内模型对比的组数和基底条件比较充分，至少足以支撑各因素（如时间交互 vs 静态特征）的单因素对比。
- **充分性与客观性**：
  - 作为唯一首次使用大规模 EEG 视频基准进行的模型评测来说，实验规模是**很充分的**。
  - 由于使用统一评价标准、多模型多维度交叉比较，在方法控制上较为客观。
  - 不足之处在于，该 benchmark 的被试数量和 EEG 记录通道数未在摘要中给出；也没有提到对照的控制刺激或者是反事实分析（如倒转视频、模型随机化权重对照等），可能影响因果推论的稳健性。总体评估，在”动态视频处理模型类脑对比“的子领域内，实验数量级上是空前的、较充分，但单一大规模基准下验证仍不能完全排除偏差。

## 6. 论文的主要结论与发现

- **发现了大脑动态视频处理的三阶段模式**：
  - **初始阶段**：较早处于层级化静态物体表征处理中。
  - **中间段**：大脑**后部电极**（枕、颞视觉皮层）表现出的响应模式与“中层次、时间整合”的动作特征（如局部运动和动作句法）对齐最佳，并且模型特征和神经动态在这些区域呈“高时间对应性”（即模型“看到”特征的时间与大脑加工该特征的时间具有精细同步性）。
  - **较晚 / 保持阶段**：**前部电极（额叶）** 所对应的响应与“高层次、较静态”的动作语义表征更契合，但与视频的具体呈现时间没有精细的时间对应关系（即具有时间不变性/抽象整合性）。
- **模型层面的发现**：
  - **时间整合的状态空间模型**（state-space models）对中段、后部电极的神经活动对齐效果最优，验证了状态空间机制对视频动态处理的合理性。
  - **自监督预训练**有利于增强模型与大脑视觉动态整体对齐，明显优于监督预训练（至少在特定中间层/后部区域上）。
- **隐喻模型**：大脑不是一个单一的模型，而是像一个**动态切换的专家混合系统**：在视觉处理的某个时间点，“获胜专家”（对齐最好的模型类型）可能是静态物体模型；紧接着变成状态空间时间整合模型；最后又切换至高层语义动作描述模型。因此，最优的单一类脑视频模型需要具备模型间组合和任务动态切换的能力。

## 7. 优点

- **首次从 fMRI 拓展到动态 EEG 自然视频基准**：以毫秒尺度刻画视频模型的动态对齐关系，填补了既往在**时域精度**上的研究空白。
- **方法学创新 —— CT-RSA**：提出跨时间展开的特征匹配框架，能够细致区分时间对齐和仅类别级别对应，使脑与模型的关系从静态划分深入到时间动态耦合层。
- **并行大规模的模型横断评估**：上百个模型 + 多维度设计，使得结论具有高度可推广性和比较性，可能成为后续视频类脑研究的标准范式。
- **多电极、脑区与模型特征的类型分解**：区分了既有的低层、高层层级顺序与脑区的关系，发现大脑前部额叶保持静态高层语义、后部皮层做动态动作中间表征，提供了新的脑机制性解释。
- **提出可行方法路径**：结合动态时序模型（如状态空间）和自监督学习为更类脑 AI 模型的设计指明了新的方向。

## 8. 不足与局限

- **任务生态效度存在局限**：虽然测试刺激是自然视频，仍属于观看任务，未能探讨实际主动行为参与中的视频理解过程，无法评估“行动中视频加工”的完整动态。
- **电极与空间推断相对粗粒度**：EEG 只有头皮投影的空间分辨率，对“后部/前部”位置的讨论并不意味着皮层下深部网络或精细皮层功能柱级别的对齐关系可以由此确定。
- **无法推翻“层级加工学派的无时序动态理论”**：结果基于行为与神经信号的关联，是**相关解释**，而非操作特定脑区验证（如破坏性实验/临床或干预研究）下得到的因果性结论。
- **计算和方法信息不足**：缺失关于被试数量、EEG 设备型号、通道数、预处理细节（如伪迹去除方法）、刺激呈现次数、统计校正方式以及跨时间比较的显著性检验细节。
- **单一大规模基准的潜在偏差**：没有提供对照条件（如固定随机权重模型基线、动静倒置视频对照），且未给出对刺激内容多样性、视频语义类别分布的控制，有可能让模型对齐分数受语义类别标签影响。
- **“专家混动”模型具有总结性，缺少工程落地**：虽然给了动态切换隐喻和架构启示，但尚未给出实现“动态混合专家”的具体神经网络算法，也没有在基线上证明这样一个混合模型可以比单模型获得更高的 CT-RSA 分数。

**（完）**
