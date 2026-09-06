---
title: "NERVE: Noise-Variability-Robust EEG Foundation Model with Electrode-Brain Interactions"
title_zh: NERVE：具有电极-大脑交互的噪声与变异性鲁棒EEG基础模型
authors: "Hyunwoo Seo, Jiwon Kim, Byeongyeon So, MINSEONG KIM, Youngjun Song, Seoyoung Jin, Woon-Hong Yeo, Chiehyeon Lim"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=03jzVlLxEe"
tags: ["query:eeg-align"]
score: 9.0
evidence: 面向噪声与变异性鲁棒性的EEG基础模型，建立电极脑相互作用来学习稳定表征
tldr: 大多数EEG基础模型只处理格式差异，忽略记录中的低信噪比、样本高变异与电极脑空间依赖。NERVE把这些获取特性显式纳入模型设计，通过电极相互作用机制建模空间依赖并对抗噪声与变异，从而在多个下游脑机接口和医疗EEG任务上学得稳定、可迁移的表征。它填补了EEG基础模型对采集物理过程建模的不足。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG获取过程中信噪比低、样本变异性高且电极空间依赖强，已有基础模型未显式建模这些特性。
method: NERVE将低信噪比、样本变异与电极脑空间交互纳入基础模型架构，学习稳定通用EEG表征。
result: 在脑机接口与医疗典型下游任务中显示更强的跨数据集稳定性和表征迁移能力。
conclusion: 建模EEG获取物理特性可显著增强基础模型对真实噪声场景的鲁棒性。
---

## Abstract
Electroencephalography (EEG) is an indispensable modality for measuring and recording brain electrical activity, with broad applications in brain–computer interfaces (BCI) and healthcare. While early EEG models predominantly adopted supervised learning methods due to the scarcity of large-scale datasets and the heterogeneity across tasks and datasets, the recent success of large foundation models has driven increasing efforts to build EEG foundation models. However, most existing studies focus on handling signals with varying formats while overlooking inherent characteristics of EEG signals during acquisition, including low signal-to-noise ratios (SNR), high variability across samples, and spatial dependencies arising from electrode placement within the acquisition system. To address these challenges, we propose NERVE, a novel noise-variability-robust EEG foundation model with electrode-brain interactions. Specifically, pre-training of NERVE begins with learning a noise-robust neural tokenizer that encodes EEG patches into discrete neural tokens. The tokenizer is trained through denoising temporal–spectral prediction to reconstruct temporal and frequency information of the original signal from noise-augmented inputs. NERVE is further pretrained to predict the neural codes of masked EEG patches, integrated with a variability-robust objective that promotes uniform EEG representations. To incorporate spatial structure in EEG, we propose an electrode-position-aware transformer as the backbone for both the tokenizer and the foundation model. It enables the model to capture spatial dependencies among electrodes and brain regions via attention mechanisms. NERVE demonstrates competitive performance across diverse BCI tasks and improved robustness to noise and variability compared to existing EEG foundation models.

---

## 论文详细总结（自动生成）

# NERVE：具有电极-大脑交互的噪声与变异性鲁棒EEG基础模型 —— 论文总结

> 说明：本次提供的材料主要为论文的元数据与摘要。以下总结基于可获取的内容展开，凡涉及正文细节（如数据集名称、算力配置、消融数量等）且未在材料中提及之处，均会明确标注“本文材料未提及”，以避免过度推断。

## 1. 论文的核心问题与整体含义

- **研究背景**：脑电图（EEG）是记录大脑电活动的重要工具，广泛应用于脑机接口（BCI）与医疗健康场景。EEG 数据标注成本高、任务与数据集之间差异大，早期研究多采用监督学习；而近年来大模型在多种模态上的成功，推动了 EEG 基础模型的研究浪潮。
- **被忽视的核心问题**：作者指出，现有 EEG 基础模型大多只处理数据“格式不一致”的表层问题（如通道数、采样率、任务范式差异），却忽略了 EEG 信号在**采集过程**中固有的三个物理特性：
  1. **低信噪比（low SNR）**：EEG 信号微弱，极易被眼电、肌电、工频干扰等噪声污染；
  2. **高样本变异性（high variability across samples）**：同一被试在不同时刻、不同被试之间的 EEG 信号波动极大；
  3. **电极-大脑空间依赖（spatial dependencies of electrode placement）**：电极在头皮上的物理位置对应不同脑区，构成有意义的空间拓扑结构，而许多模型将电极当作无序通道处理。
- **研究意义**：该论文主张——EEG 基础模型应当**显式建模采集过程的物理特性**，而非仅仅追求“格式无关”的表征对齐。这是对现有 EEG 基础模型设计范式的一个重要补缺。

## 2. 论文提出的方法论

论文提出 **NERVE**（Noise-Variability-Robust EEG Foundation Model with Electrode-Brain Interactions），其方法体系包含以下几大核心组件：

- **核心思想**：将低信噪比、样本高变异性和电极-脑空间交互这三类“采集固有属性”纳入基础模型的预训练目标与网络架构中，从而学到**对噪声与变异均鲁棒**的通用 EEG 表征。
- **噪声鲁棒神经分词器（Noise-Robust Neural Tokenizer）**：
  - 将 EEG 信号切分为 patch，并编码为**离散神经 token（neural tokens）**；
  - 采用**去噪时-谱预测（denoising temporal-spectral prediction）** 作为训练目标：对输入施加噪声增强后，要求 tokenizer 仍能重构原始信号的时间域信息和频率域信息；
  - 该设计使 tokenizer 在源头就具备抗噪能力，避免噪声信息被带入后续表征。
- **掩码神经代码预测（Masked Neural Code Prediction）预训练**：
  - 在 tokenizer 之后，NERVE 以类似 masked autoencoding 的方式，预测被随机掩码 patch 的神经代码（neural codes），以学习 EEG 序列的上下文语义。
- **变异性鲁棒目标（Variability-Robust Objective）**：
  - 在预训练中额外加入一项促进 EEG 表征**均匀化（uniformity）**的目标函数，用于压缩跨样本、跨个体的无关变异，使表征更稳定、更可迁移。
- **电极位置感知 Transformer（Electrode-Position-Aware Transformer）**：
  - 同时作为 tokenizer 和主干网络的骨干架构；
  - 通过**注意力机制显式建模电极之间的空间依赖以及电极与脑区之间的交互关系**（即 electrode-brain interactions），而非把多通道 EEG 当作平面序列处理；
  - 该机制的引入使得模型能利用电极的空间拓扑结构，增强表征的物理可解释性。

> 由于材料中仅有摘要，论文未提供完整公式与原理解释，上述内容为基于摘要文字的忠实转述，不含编造的公式推导。

## 3. 实验设计

- **任务场景**：论文声称其评估覆盖了**多种 BCI 任务**（如运动想象等典型脑机接口范式）以及**医疗 EEG 下游任务**。
- **评估维度**：除了常规任务性能外，重点评估了模型对**噪声**与**样本变异性**的鲁棒性。
- **对比方法**：与**现有 EEG 基础模型（existing EEG foundation models）** 进行比较，结果显示 NERVE 在多种任务中具有竞争力，且在噪声与变异性鲁棒性上表现更优。
- **具体数据集与基准（benchmark）细节**：本材料未提及具体的数据集名称、样本量、任务类型数量与评价指标数值。若要判断实验的覆盖面与公平性，需查看论文正文的实验章节。

## 4. 资源与算力

- 本材料**未提及**任何算力信息，包括 GPU 型号、数量、训练时长、参数量等。
- 也**未提供**模型规模（如参数量级、token 数量）或训练数据规模。
- 若需要评估方法的资源代价与可复现性，需参考论文正文或附录。

## 5. 实验数量与充分性

- 从摘要判断，实验涵盖了两大方向（BCI 与医疗），并做了与现有 EE G 基础模型的对比实验，显示 NERVE 在“性能竞争力”与“鲁棒性”两个维度上均有结果支撑。
- 然而，在本文提供的材料范围内**看不到具体的实验数量、消融实验设置、统计显著性检验、跨数据集迁移的具体结果**。
- 因此，对实验充分性的完整判断受限。仅凭摘要，只能说实验设计思路覆盖了核心声称（性能与鲁棒性），但无法确认其是否做了充分的消融（如去掉电极位置感知模块、去掉变异性鲁棒目标等）以及各组件贡献的量化分析。

## 6. 论文的主要结论与发现

- NERVE 在多个 BCI 下游任务上取得了**具有竞争力的表现**。
- 相比现有 EEG 基础模型，NERVE 表现出**更强的抗噪能力**（对低信噪比输入的鲁棒性）与**更强的抗变异性能力**（对跨样本/跨个体波动的稳定性）。
- 作者由此论证：**将 EEG 采集过程中的物理特性（噪声、变异、电极-脑空间结构）显式纳入模型设计，能显著提升 EEG 基础模型在真实场景下的泛化能力**。这构成了论文的核心贡献主张。

## 7. 优点

- **问题切入精准**：指出了现有 EEG 基础模型“重格式对齐、轻采集物理过程”的重要盲区，研究动机清晰且具有实际临床与工程价值。
- **方法论完整且自洽**：从 tokenizer 的噪声鲁棒训练（去噪时-谱预测）到主干网络的变异性鲁棒目标（均匀化约束），再到电极位置感知 Transformer，三个设计分别对应论文提出的三个采集痛点，映射关系明确、逻辑紧凑。
- **引入空间结构建模**：将电极位置与脑区关系通过注意力机制引入 Transformer，是对 EEG 基础模型架构的一种有价值探索，突破了将电极视为无关通道的常规做法。
- **输出模态设计合理**：离散神经 token + 掩码预测的框架与当前多模态大模型主流技术路线接轨，具备良好的可扩展性。
- **评价指标聚焦鲁棒性**：不仅看任务精度，还专门检验噪声与变异性下的稳定性，评估方式与论文主张高度一致。

## 8. 不足与局限

- **材料信息量有限**：本总结仅基于摘要与元数据，无法获取数据集的详细构成、实现细节与消融实验等关键信息。
- **实验结果细节缺失风险**：摘要中未报告定量数值（如分类准确率、AUROC、对比提升幅度），因此实际效果的幅度难以独立判断。
- **“变异性鲁棒目标”的具体机制未展开**：如何定义均匀性目标、如何与其他损失函数加权、是否会在某些任务上牺牲类别判别性，仍有待正文说明。
- **“电极-大脑交互”的可解释性与验证不足**：仅凭摘要无法判断注意力机制是否真正学到了符合神经科学的电极-脑区对应关系，抑或只是隐式学到了位置相关的统计规律。
- **评估范围限制**：若正文化验只集中在有限的公开数据集，则跨真实临床环境的噪声多样性（如运动伪迹、电极脱落）未必能充分体现。
- **值得注意的审稿状态**：该论文标注为 ICLR-2026-Rejected-Public，虽本材料给出的评分较高（9.0），但既然被拒，说明评审者可能在方法新颖性、实验完整性或写作组织等方面存在实质性质疑；具体拒稿原因需阅读审稿意见与正文判断。

（完）
