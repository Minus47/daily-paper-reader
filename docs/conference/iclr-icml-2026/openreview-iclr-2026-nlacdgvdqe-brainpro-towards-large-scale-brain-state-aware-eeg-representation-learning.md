---
title: "BrainPro: Towards Large-scale Brain State-aware EEG Representation Learning"
title_zh: BrainPro：面向大规模脑状态感知的EEG表征学习
authors: "Yi Ding, Muyun Jiang, Weibang Jiang, Shuailei Zhang, Xinliang Zhou, Chenyu Liu, Shanglin Li, Yong Li, Cuntai Guan"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=nlaCdgvDQE"
tags: ["query:eeg-align"]
score: 9.0
evidence: 用脑状态感知来学习空间结构化的EEG表征，使不同通道设置下表征一致并提升下游任务
tldr: EEG的脑活动具有脑区空间结构，现有基于自注意力的基础模型难以保持通道位置信息，也无法在不同导联设置间对齐。BrainPro提出脑状态感知的大规模EEG表征学习，既建模多个脑区共有的状态，也保留状态特定的区域活动，并兼容不同通道配置。该方法获得神经生理上合理且状态可感知的表征，能更好地支持下游脑电解码与跨数据集任务。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自注意力EEG基础模型缺乏位置感知且难以跨通道对齐，脑状态包含共有与特定区域活动，需要专门表征。
method: 提出脑状态感知的大规模EEG表征学习，同时学习共享与状态特异脑区活动，统一不同通道配置。
result: 该方法可在大规模数据上学习状态感知且跨通道可泛化的EEG表征，改善下游解码。
conclusion: 纳入脑状态先验与空间结构是提升EEG基础模型表征质量的关键方向。
---

## Abstract
Electroencephalography (EEG) reflects underlying brain states, whose activities are distributed across brain regions and manifest as spatial patterns on the scalp. Learning these spatially structured, state-related patterns requires consistent spatial representations across datasets. However, existing EEG foundation models are typically based on self-attention, which does not preserve location-specific information and struggles to align signals recorded with different channel configurations. Moreover, brain states contain both shared and state-specific regional activity, suggesting that learning neurophysiologically plausible, state-aware representations can complement the shared representations targeted by current models and improve downstream decoding.
To address these limitations, we propose BrainPro, a large EEG model that combines a retrieval-based spatial learning mechanism for cross-layout spatial alignment with a brain state-decoupling module that learns both shared and state-specific representations through parallel encoders and region-aware reconstruction. Pre-trained on a large EEG corpus, BrainPro achieves state-of-the-art performance across nine public BCI datasets spanning emotion, motor, speech, stress, mental disease, and attention tasks. Analyses of spatial filters, channel-drop robustness, and encoder contributions further validate the effectiveness of its spatial alignment and state-aware pathways. These results show that BrainPro achieves improved interpretability of learned spatial patterns and produces representations that benefit diverse EEG decoding tasks.

---

## 论文详细总结（自动生成）

# 论文总结：BrainPro：面向大规模脑状态感知的 EEG 表征学习

> 说明：当前可获得的信息仅包括论文标题、作者、摘要及部分元数据（来源为 ICLR-2026-Rejected-Public），未包含完整正文、方法与实验细节。以下总结基于摘要和元数据进行客观归纳，对未明确信息会加以标注。

## 1. 核心问题与整体含义

- **核心问题**：现有 EEG 基础模型大多基于自注意力（self-attention）机制，缺乏对“通道位置/空间位置”的感知，难以保持不同脑区分工的空间结构，也无法在不同导联配置（channel configuration）之间对齐信号。
- **脑状态特性**：EEG 反映的脑状态活动分布于不同脑区，并以头皮空间模式体现；同时，脑状态既包含跨状态共享的共有活动，也包含状态特异的区域活动。因此，单纯学习共享表征并不能充分刻画脑状态的差异。
- **研究动机**：需要一个能够同时建模“共有脑状态”和“状态特有脑区活动”、并支持跨数据集/跨通道设置一致性的 EEG 表征学习方法。
- **整体含义**：论文提出 BrainPro，通过结合脑状态先验与空间结构，让 EEG 基础模型学习“神经生理上合理且状态可感知”的表征，从而更好地支持下游多种脑电解码任务。

## 2. 方法论

- **整体架构**：BrainPro 是一个大规模 EEG 模型，包含两个核心模块：
  - **基于检索的空间学习机制（retrieval-based spatial learning mechanism）**：用于跨布局空间对齐，使得不同通道配置下记录的 EEG 信号能够映射到统一、一致的空间表征。
  - **脑状态解耦模块（brain state-decoupling module）**：通过并行编码器（parallel encoders）和区域感知重建（region-aware reconstruction）任务，分别学习“共享表征”与“状态特定表征”。
- **预训练方式**：在大规模 EEG 语料上进行预训练，使模型习得可泛化的空间对齐与状态感知表征。
- **公式与算法流程**：摘要中未提供具体数学公式或训练伪代码；核心思想可概括为“将脑状态先验注入自监督预训练，通过解耦和区域重建让表征既保持共性又保留特异性，并利用检索实现通道布局不变性”。

## 3. 实验设计

- **数据集与场景**：使用了 **9 个公开 BCI 数据集**，涵盖：
  - 情绪识别
  - 运动想象
  - 言语相关任务
  - 压力检测
  - 精神疾病识别
  - 注意力任务
- **Benchmark**：在上述多任务、跨数据集的条件下进行下游解码评估，目标是验证“预训练表征”的通用性。
- **对比方法**：摘要中未明确列出具体基线名称，仅表明 BrainPro 在 9 个数据集上达到“state-of-the-art”性能。
- **辅助分析**：
  - 空间滤波器分析（spatial filters）
  - 通道丢弃鲁棒性（channel-drop robustness）
  - 编码器贡献分析（encoder contributions）
  - 这些分析旨在验证空间对齐和状态感知模块的具体作用。

## 4. 资源与算力

- **未明确说明**：摘要和元数据中均未提及训练所用的 GPU 型号、数量、训练时长、数据规模等具体算力信息。
- 若需要精确评估训练成本与可复现性，需查阅论文完整正文或补充材料。

## 5. 实验数量与充分性

- **实验覆盖面较广**：在 9 个公开数据集、6 类任务上验证，说明方法具有跨任务、跨数据集的泛化潜力。
- **分析性实验较丰富**：除主结果外，还进行了空间滤波器、通道缺失鲁棒性、编码器贡献等消融/分析实验，有助于归因每个模块的贡献。
- **充分性评价存在局限**：
  - 摘要未披露具体指标、数值差异和统计检验，无法判断性能提升是否显著；
  - 缺少对比方法细节、实现细节和数据划分方式；
  - 由于是“ICLR-2026-Rejected-Public”版本（公开为被拒稿版本），完整审稿意见未提供，难以从评审角度进一步判断实验漏洞。
- **总体判断**：从摘要所展示的实验设计来看，结构较为系统和多维度；但当前信息不足，不能完全判定其公平性和客观性。

## 6. 主要结论与发现

- BrainPro 在 9 个 BCI 数据集上达到了当前最优性能，证明其预训练表征能够服务于多种下游 EEG 解码任务。
- 空间对齐机制有效应对了不同通道配置带来的跨数据集不一致问题。
- 状态感知与状态解耦路径能够补充仅学习共享表征的方式，提升解码效果。
- 学习到的空间模式具有更好的可解释性，符合脑区空间结构。

## 7. 优点

- **问题切入有实际价值**：跨导联设置对齐是 EEG 现实中无法回避的问题，自注意力模型往往忽视位置信息，BrainPro 明确针对这一点设计机制。
- **神经生理合理性**：将“共享状态”和“状态特异区域活动”进行解耦，符合脑状态常识，比纯粹数据驱动的表征更易解释。
- **模块设计清晰**：结合检索机制与并行编码器、区域感知重建，形成完整预训练框架。
- **评估广泛**：覆盖 9 个数据集和 6 类任务，并辅以多种分析实验，体现较强的实证规模。

## 8. 不足与局限

- **文本信息不完整**：只有摘要，无法评估具体实现细节、损失函数、模型规模等。
- **计算资源未报告**：对于大规模预训练模型，缺少算力与训练成本说明。
- **对比方法与数值不足**：没有列出基线模型的具体性能，也没有报告结果方差、显著性检验等，难以验证“SOTA”的稳健性。
- **潜在偏差风险**：9 个数据集可能存在参与者差异、采集设备差异、任务范式差异等混淆因素，摘要未说明如何控制；
- **应用边界不明确**：主要面向 BCI 解码，对于临床 EEG 分析、睡眠分期、癫痫等更多实际场景的推广性尚未讨论。
- **该论文来源标注为被拒稿版本（ICLR-2026-Rejected-Public）**，虽不能代表方法一定不可靠，但说明在评审中存在可能未能解决的内在问题或实验不足，需谨慎看待其结论。

（完）
