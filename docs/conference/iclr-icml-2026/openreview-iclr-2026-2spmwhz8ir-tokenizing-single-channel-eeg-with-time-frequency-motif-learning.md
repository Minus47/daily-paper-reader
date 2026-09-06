---
title: Tokenizing Single-Channel EEG with Time-Frequency Motif Learning
title_zh: 基于时频基元学习的单通道EEG分词化
authors: "Jathurshan Pradeepkumar, Xihao Piao, Zheng Chen, Jimeng Sun"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=2sPmWHZ8Ir"
tags: ["query:eeg-align"]
score: 8.0
evidence: 通过学习时频基元字典将单通道EEG转成离散token，获得富有信息且可迁移的神经表征
tldr: "EEG基础模型发展迅速，但如何把原始EEG转化为有意义的token仍是关键未解问题。作者提出TFM-Tokenizer，通过双路径时频掩码学习单通道EEG的时频基元字典，并用该词表编码离散token作为后续Transformer输入。该分词器与模型无关，可服务于轻量模型或现有EEG基础模型，在四个EEG基准上无论单数据集还是多数据集预训练都稳定提升下游任务性能，最高提升约11%，显示学到的EEG基元token表征具有较强的泛化能力。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: EEG基础模型依赖稳定的输入编码，但现有EEG分词化仍不成熟，难以从单通道原始信号学到通用且可迁移的表征。
method: 提出时频基元学习的分词框架TFM-Tokenizer，以双路径时频掩码提取单通道EEG的基元，学习离散词表并编码token供Transformer使用。
result: "在四个EEG基准上始终超过现有编码方式，最高相对提升11%，并兼容不同下游模型，跨数据集预训练也有增益。"
conclusion: 学习数据驱动的EEG时频基元词表，可增强EEG基础模型的下游复用能力，为脑电表征学习提供通用接口。
---

## Abstract
Foundation models are reshaping EEG analysis, yet an important problem of EEG tokenization remains a challenge. 
This paper presents TFM-Tokenizer, a novel tokenization framework that learns a vocabulary of time-frequency motifs from *single-channel* EEG signals and encodes them into discrete tokens. 
We propose a dual-path architecture with time–frequency masking to capture robust motif representations, and it is model-agnostic, supporting both lightweight transformers and existing foundation models for downstream tasks. 
Our study demonstrates three key benefits:
*Accuracy:* Experiments on four diverse EEG benchmarks demonstrate consistent performance gains across both single- and multi-dataset pretraining settings, achieving up to $11\%$ improvement in Cohen’s Kappa over strong baselines.
*Generalization:* Moreover, as a plug-and-play component, it consistently boosts the performance of diverse foundation models, including BIOT and LaBraM. 
*Scalability:* By operating at the single-channel level rather than relying on the strict 10–20 EEG system, our method has the potential to be device-agnostic.
Experiments on ear-EEG sleep staging, which differs from the pretraining data in signal format, channel configuration, recording device, and task, show that our tokenizer outperforms baselines by $14\%$.
A comprehensive token analysis reveals strong class-discriminative, frequency-aware, and consistent structure, enabling improved representation quality and interpretability.
Code is available at https://github.com/Jathurshan0330/TFM-Tokenizer.

---

## 论文详细总结（自动生成）

# 论文总结：基于时频基元学习的单通道EEG分词化（TFM-Tokenizer）

> 注意：本总结主要基于论文摘要与元数据生成，缺少部分细节时已明确说明。

## 1. 核心问题与研究动机

- **背景**：EEG 基础模型正在快速发展，但 EEG 信号如何被有效“分词化”（Tokenization）仍是关键瓶颈。  
- **核心问题**：直接从原始单通道 EEG 中学习通用、可迁移且信息丰富的离散表征，而不是依赖人工设计的特征或固定的电极通道布局。  
- **研究意义**：一个好的分词器应是“模型无关”的，能够作为即插即用组件服务于轻量 Transformer 和现有 EEG 基础模型，从而提升下游复用能力。  
- **论文主张**：通过在该问题上引入“时频基元”（time-frequency motifs）学习，能够让 EEG 编码更准确、更具泛化性，并摆脱严格 10–20 系统的设备限制。

## 2. 方法论：TFM-Tokenizer

- **总体思想**：提出 TFM-Tokenizer，从**单通道 EEG**中学习一个与时频模式相关的“基元词汇表”，再将原始信号编码为该词汇表上的离散 token，供下游模型使用。
- **关键步骤（根据摘要推断）**：
  1. 将单通道 EEG 进行时频变换/分解，获取时间和频率维度的联合信息；
  2. 利用**双路径（dual-path）架构**分别处理不同维度的表征；
  3. 引入**时频掩码（time-frequency masking）**进行自监督/对比式训练，迫使模型学到具有鲁棒性的时频基元；
  4. 学习得到离散词表后，把输入 EEG 切分并映射为 token 序列，用于下游 Transformer 或现有基础模型。
- **可用细节程度**：摘要中**没有提供具体网络层数、掩码策略、矢量量化方式或损失函数公式**，因此无法给出一对一的数学流程；但“时频基元学习 + 离散编码”是该分词器的核心主旨。

## 3. 实验设计

- **主实验数据集**：使用了**四个不同的 EEG 基准数据集**，但摘要中未列出具体数据集名称（如 Sleep-EDF、TUEG 等未说明）。
- **预训练设置**：同时报告了
  - 单数据集预训练；
  - 多数据集预训练；
  两种情况下的表现。
- **对比基线与下游模型**：
  - 与现有编码/分词方式形成的强基线比较；
  - 作为即插即用组件，接入多种下游基础模型，**包括 BIOT 和 LaBraM**；
  - 对比包括 Cohen’s Kappa 等指标。
- **跨域泛化场景**：
  - 使用**耳-EEG（ear-EEG）睡眠分期**作为外部迁移测试；
  - 该场景在信号格式、通道配置、记录设备和任务上都与预训练数据不同，用于检验 device-agnostic 能力。
- **词表分析**：
  - 对学到的 token/基元进行深入的**类别区分度、频率感知性、结构一致性**分析，以说明表征质量和可解释性。

## 4. 资源与算力

- 论文摘要和元数据中**没有说明**使用了多少 GPU 数量、型号、训练时长或总计算量。  
- 因此无法评估其训练成本；但从设计上看，单通道级 tokenizer 相对轻量，适合作为基础模型的输入端模块。

## 5. 实验数量与充分性

- **实验组数可能包括**：
  1. 四个 EEG 基准上的主实验；
  2. 同时覆盖单数据集与多数据集预训练条件；
  3. 接入不同下游基础模型（如 BIOT、LaBraM）的插件测试；
  4. 一个跨设备/跨领域的耳-EEG 迁移实验；
  5. Token/基元的可视化或统计分析。
- **充分性评价**：
  - 从“性能提升 + 泛化性 + 可解释性”三方面验证了 tokenizer 的有效性，覆盖面较广；
  - 但摘要未提供各实验的误差线、统计检验、消融细节（如双路径和时频掩码的贡献量）以及每个任务的数据规模，因此**严格意义上的公平性和统计显著性尚不完整**。

## 6. 主要结论与发现

- **准确性**：在四个 EEG 基准上稳定超过现有强基线，单/多数据集预训练均有效，最高取得 **Cohen’s Kappa 提升约 11%**。
- **泛化性**：作为即插即用模块，TFM-Tokenizer 能稳定提升 BIOT、LaBraM 等多种基础模型的性能。
- **可扩展性**：在单通道层面工作，不依赖严格 10–20 通道位置，因此可以扩展到非标准设备。
- **跨域迁移**：在耳-EEG 睡眠分期任务上比基线高出 **14%**，证明了不同采集格式、不同任务上的迁移能力。
- **可解释性**：token 分析显示学到的是具有类别区分性、频率感知和内在一致结构的表征，有助于更好的表示学习和模型解释。

## 7. 方法或实验设计的亮点

- **单通道级别建模**：打破了传统多通道 EEG 固定电极布局的限制，面向可穿戴和耳-EEG 等非标准场景，实用性强。
- **时频基元学习**：将离散 token 建立在时频结构上，相比直接切分时间窗口或简单量化，更能保留脑电的生理意义。
- **双路径 + 时频掩码**：在多维度上做数据增强/掩码重建，有利于提高表征鲁棒性。
- **模型无关性**：允许将 tokenizer 作为标准接口，与多种基础模型协同工作，符合 EEG foundation model 生态需求。
- **多数据集与跨域验证**：不只做同分布测试，还验证了分布外场景下的迁移能力，增强了结论的可信度。

## 8. 不足与局限

- **技术细节缺失**：摘要中未披露双路径结构、mask 策略、量化方法、预训练任务目标和超参数设置，无法复现或深入分析其设计原因。
- **实验信息不完备**：四个 EEG 基准数据集名称未列出，无法判断数据集规模、任务类型和领域多样性；对于 benchmark 的选取也可能缺乏足够的覆盖。
- **缺乏消融和统计检验**：没有明确给出消融实验、方差分析或故障案例，因此无法单独量化“时频掩码”或“双路径”的贡献，也无法排除调参带来的偏差。
- **算力报告缺失**：未给出训练计算成本，不利于后续研究进行公平对比。
- **潜在应用限制**：虽然单通道策略提升了设备兼容性，但没有说明如何扩展到需要空间/通道间关系的任务；对于不同采样率、伪迹极强或极短时长的 EEG，其鲁棒性尚未讨论。
- **与人类专家知识的关系**：时频基元是数据驱动学习得到的，能否在语义上与已知脑电节律（delta、theta、alpha 等）对应也仍需更多分析。

---

（完）
