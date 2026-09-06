---
title: "CodeBrain: Bridging Decoupled Tokenizer and Multi-Scale Architecture for EEG Foundation Model"
title_zh: CodeBrain：连接解耦分词器与多尺度架构的脑电基础模型
authors: "Jingying Ma, Feng Wu, Qika Lin, Yucheng Xing, Chenyu Liu, Ziyu Jia, Mengling Feng"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=msJgEkjwh5"
tags: ["query:eeg-align"]
score: 9.0
evidence: CodeBrain作为两阶段脑电基础模型，以时频解耦token和多尺度架构学习判别性强且可解释的脑活动表征
tldr: 脑电基础模型虽能应对任务特定模型扩展问题，但现有模型表示判别力弱、可解释性差，且难以兼顾全局依赖与局部神经事件。该文提出CodeBrain两阶段脑电基础模型，第一阶段用TFDual-Tokenizer将时频异构脑电信号解耦为离散token以扩大表征空间，第二阶段用多尺度架构建模全局和局部脑动态。该方法旨在提升脑电表示的判别力与可解释性，为多种下游神经科学任务提供通用而稳健的特征基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有EEG基础模型表示判别性弱、不可解释，也难同时捕获全局依赖与局部神经事件。
method: 两阶段设计：先用TFDual-Tokenizer时频解耦为离散token，再用多尺度架构提取全局与局部特征。
result: 该表示学习方案可扩大EEG特征空间并提高下游解码任务的判别能力。
conclusion: 为脑电通用基础模型的表征学习与神经任务迁移提供了可行架构。
---

## Abstract
Electroencephalography (EEG) provides real-time insights into brain activity and supports diverse applications in neuroscience. While EEG foundation models (EFMs) have emerged to address the scalability issues of task-specific models, current approaches still yield clinically uninterpretable and weakly discriminative representations, inefficiently capturing global dependencies and neglecting important local neural events. We present CodeBrain, a two-stage EFM designed to fill this gap. In the first stage, we introduce the TFDual-Tokenizer, which decouples heterogeneous temporal and frequency EEG signals into discrete tokens, quadratically expanding the representation space to enhance discriminative power and offering domain-specific representation-level interpretability by suggesting potential links to neural events and spectral rhythms. In the second stage, we propose the multi-scale EEGSSM architecture, which combines structured global convolution with sliding window attention to efficiently capture both sparse long-range and local dependencies, reflecting the brain’s small-world topology. Pretrained on the largest public EEG corpus, CodeBrain achieves strong generalization across eight downstream tasks and ten datasets under distribution shifts, supported by comprehensive ablations, scaling-law analyzes, and interpretability evaluations. The code and the pretrained weights are available at https://github.com/jingyingma01/CodeBrain.

---

## 论文详细总结（自动生成）

# CodeBrain：脑电基础模型的解耦分词与多尺度架构

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：脑电图（EEG）能实时反映脑活动，支撑多种神经科学应用。为应对任务特定模型在扩展性上的瓶颈，脑电基础模型（EFM）已成为新兴研究方向。
- **核心问题**：现有EFM存在三重缺陷：
  - 学到的表征**判别力弱**，不足以为下游任务提供高区分度的特征；
  - 表征**临床不可解释**，难以映射到神经事件和频谱节律等生理学意义；
  - **建模效率不足**：无法在有效捕获全局依赖关系的同时，兼顾对重要局部神经事件的建模。
- **研究意义**：该论文旨在填补"判别力 + 可解释性 + 多尺度动态建模"三方面同时缺失的空白，为脑电通用基础模型提供一套可行的表征学习范式与可迁移架构。

## 2. 方法论：核心思想与技术细节

- **整体设计**：CodeBrain 是一个两阶段脑电基础模型：
  - **第一阶段 — TFDual-Tokenizer（时频双通道分词器）**：
    - 核心思想：EEG 信号在时域和频域上具有异质性（heterogeneous），需将二者**解耦处理**后再统一映射。
    - 具体做法：将时域与频域信息分别处理为离散 token，再合并形成统一的离散编码。
    - 关键收益一：由于时/频两个通道的 token 空间相互组合，表征空间获得**二次方级扩展**，从而显著增强特征的判别力。
    - 关键收益二：解耦后的 token 可分别与神经事件（时域）和频谱节律（频域）建议性地关联，从而带来**表征级别的领域可解释性**。
  - **第二阶段 — 多尺度 EEGSSM 架构**：
    - 核心思想：大脑网络具有**小世界拓扑**特性——既存在稀疏的长距离全局连接，也存在高度聚集的局部连接，模型架构应顺应这一特性。
    - 具体做法：将**结构化全局卷积**（建模稀疏长程依赖）与**滑动窗口注意力**（建模局部依赖）结合在一个统一架构中，从而高效捕获全局与局部双尺度脑动态。
- **预训练**：在目前最大的公共 EEG 语料库上进行预训练，使模型获得跨任务、跨数据集的通用脑电表征基础。

## 3. 实验设计

- **评测范围**：覆盖 **8 个下游任务 × 10 个数据集**，强调在**分布偏移（distribution shifts）**条件下的泛化能力验证。
- **对比基准**：与现有脑电基础模型及任务特定模型进行对比（具体对比方法名称未在摘要中逐一列出，但强调 CodeBrain 在跨任务与跨数据集上的强泛化性）。
- **补充实验**：
  - **消融研究**：验证 TFDual-Tokenizer 与多尺度 EEGSSM 各组件的贡献；
  - **缩放律分析**：检验模型规模/数据规模与性能的扩展关系；
  - **可解释性评估**：验证 token 解耦所赋予的神经事件与频谱节律层面的解释能力是否在实际任务中成立。

## 4. 资源与算力

- **注意**：在提供的论文摘要与元数据中，**未明确提及**预训练所用的 GPU 型号/数量、总训练时长及计算量（如 FLOPs）等信息。
- 论文中仅提到"预训练于最大的公共 EEG 语料库"，但未披露具体算力成本，若需获取此信息需查阅论文正文的实验设置部分。

## 5. 实验数量与充分性

- **实验体量**：属于较全面的实证研究——跨越 10 个数据集、8 类下游任务，并配套消融、缩放律和可解释性三类专项评估，实验数量在 EEG 基础模型研究领域属于**较充分**水平。
- **客观性评估**：强调分布偏移下的评测设计，有助于检验模型真实泛化能力而非仅在域内过拟合，这一点在公平性上具有明显优势。
- **潜在不足**：从摘要无法判断其对比方法是否涵盖了所有最新强基线（如其他 SOTA EFM），也未说明具体评测指标与统计显著性检验方式，这在一定程度上影响对实验"客观/公平"的最终判断。

## 6. 主要结论与发现

- 时/频解耦的离散 token 化方法能够**二次方级扩大 EEG 特征表征空间**，有效改善下游任务的判别能力。
- 多尺度架构（全局卷积 + 局部滑动窗口注意力）能够同时捕捉**稀疏长程依赖与局部神经事件**，契合大脑小世界拓扑特性，优于仅侧重单一尺度的范式。
- CodeBrain 在最大规模公共 EEG 语料上预训练后，在多种下游任务与开放数据集上显示出强泛化能力，**验证了规模化 EEG 预训练的可行性**。
- 总体结论：为脑电通用基础模型的表征学习（可解释、可判别）与神经任务迁移提供了**可行且有效的架构方案**。

## 7. 优点

- **问题定位精准**：直击现有 EFM 在可解释表征与多尺度动态建模上的真空白，而非简单堆叠更大模型。
- **方法论有生物先验支撑**：以小世界拓扑作为多尺度架构的设计依据，而非纯工程性的注意力堆叠，算法与神经科学之间建立了合理映射。
- **可解释性设计内生于架构**：时域/频域解耦分词天然建立 token 与神经事件/频谱节律之间的表征级对应，不是事后附加的解释工具，创新性强。
- **评测全面**：多任务 + 多数据集 + 分布偏移 + 消融 + 缩放律 + 可解释性评估，形成较为完整的证据链。
- **开放性**：代码与预训练权重已公开，可复现性强。

## 8. 不足与局限

- **算力信息缺失**：未提供 GPU 类型/数量、训练时长与能耗等硬件信息，外部研究者难以评估其复现成本。
- **对比方法覆盖不明**：需进一步确认是否与最新的主流脑电基础模型全部做过对比，以防引入选择偏差。
- **临床有效性验证有限**：虽然提出了"可解释性"，但从摘要无法确认是否获得了神经科学/临床专家的系统性验证——表征层面的解释性未必等于临床决策层面的高可信度。
- **任务与数据集的物种/人群覆盖**：10 个数据集的跨度是否覆盖了足够的年龄、疾病和采集设备条件，从摘要无从判别，可能存在应用层面的推广限制。
- **缩放律范围不清楚**：缩放律分析的具体参数范围未在摘要中披露，其对巨型模型的指导意义尚需验证。

（完）
