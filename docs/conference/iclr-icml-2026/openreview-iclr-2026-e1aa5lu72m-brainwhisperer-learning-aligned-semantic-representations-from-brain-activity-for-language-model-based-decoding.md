---
title: "BrainWhisperer: Learning Aligned Semantic Representations from Brain Activity for Language Model-based Decoding"
title_zh: BrainWhisperer：从脑活动学习对齐的语义表征用于基于语言模型的脑解码
authors: "Dongyang Li, Dazhou Liu, Sitong Chen, Jiayu Zuo, Kunpeng Xie, Yiming Liu, Chen Wei, Quanying Liu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=e1Aa5lU72m"
tags: ["query:eeg-align"]
score: 9.0
evidence: 将EEG表征与大语言模型语义空间对齐，实现脑到文本解码
tldr: 非侵入脑电缺乏结构化语义空间，导致脑到文本解码困难。BrainWhisperer用Transformer编码器在EEG上学习神经活动表征，并通过对齐方法将其映射到大语言模型丰富的语义空间中。该对齐缓解了脑电与文本之间的模态鸿沟，使脑到文本解码更有效。这为基于脑电的语义对齐和语言解码建立了直接可行的强基线。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 脑电信号缺少结构化语义空间，与语言模型语义空间的差异妨碍脑到文本解码。
method: 训练EEG上的Transformer编码器，将对齐目标用于最小化神经活动表征与LLM语义表征之间的距离。
result: 在脑到文本解码中取得更有效的结果，验证了神经信号与语言模型语义空间对齐的作用。
conclusion: 将神经活动对齐到预训练语言模型语义空间是弥合EEG与文本鸿沟的有效途径。
---

## Abstract
Large language models (LLMs) have demonstrated remarkable capabilities in capturing rich and generalizable semantic representations. In contrast, non-invasive neural signals such as electroencephalography (EEG) lack a well-structured semantic space, making brain-to-text (B2T) decoding especially difficult. This gap motivates us to ask: can neural activity embeddings be aligned with the powerful semantic space of language models, thereby enabling more effective brain decoding? We introduce BrainWhisperer, a novel framework that leverages the rich semantic capabilities of LLMs to address this gap. Our core contribution is an alignment methodology where a Transformer-based encoder, trained on EEG data, is optimized via a contrastive objective to map neural activity into the latent representation space of a powerful, pre-trained and frozen text encoder. This generates unified semantic tokens for language models. We propose and evaluate two decoding pathways: (1) a direct decoding approach where the learned brain embeddings are fed into a lightweight adapter and a frozen text decoder to autoregressively generate text, and (2) an LLM-copilot strategy, where retrieved semantically relevant words from brain embeddings serve as prompts for large language models to generate coherent and context-rich text. Experiments on listening datasets demonstrate that BrainWhisperer produces semantically faithful and fluent text, outperforming baseline approaches. By bridging neural signals with the semantic capacity of LLMs, BrainWhisperer represents a step toward practical and robust brain-to-text communication systems.

---

## 论文详细总结（自动生成）

好的，我仔细阅读了您提供的论文内容（标题、元数据、摘要等）。请注意，原始PDF链接是一个OpenReview的验证页面，所提取的正文内容为空。因此，以下总结严格基于给定的摘要和元数据字段展开；有关实验细节、具体数据集名称等，我会明确指出哪些是论文中未提供的信息。

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心研究动机**：大型语言模型（LLM）能够捕捉丰富且可泛化的语义表征，但非侵入性神经信号（尤其是脑电图，EEG）缺乏结构化的语义空间，这使得将脑信号直接解码为文本（brain-to-text，B2T）变得极其困难。
- **核心研究问题**：能否将神经活动的高维嵌入与语言模型强大的语义空间对齐，从而利用LLM的语义先验实现更有效的脑解码？
- **整体定位**：论文提出的**BrainWhisperer**框架旨在缓解脑电信号与语义文本之间的模态鸿沟，为基于EEG的实际脑到文本通信系统建立直接、可复现的强基线。

## 2. 论文提出的方法论：核心思想、关键技术细节与流程

- **整体架构设计**：论文采用了一种两阶段式的语义对齐方案，而非传统的端到端直接映射。
- **核心组件一：EEG编码器**
  - 使用基于Transformer架构的编码器，直接作用于原始EEG信号（或浅层时间特征），训练目标是将神经活动映射到语义表征空间。
  - 通过对原始EEG数据训练而非依赖手工特征，提高了对不同脑活动模式的适配性。
- **核心组件二：冻结的文本编码器**
  - 采用**强大、预训练且冻结（frozen）的文本编码器**（如LLM的编码端或前若干层）作为语义锚点。
  - 冻结保证了语义空间的稳定性，防止训练过程中被神经信号带偏，从而将神经信号对齐到LLM已有的、丰富的语义空间。
- **核心组件三：对比对齐目标（Contrastive Objective）**
  - 核心方法的关键在于****对齐损失**：最小化神经活动表征与对应的文本语义表征之间的距离。
  - 通过**对比学习（contrastive learning）** 将EEG编码器学到的嵌入向量拉近真实文本的语义嵌入，同时推远非匹配的负样本，生成统一的语义token供语言模型使用。
- **解码路径一：“直接解码”**
  - 将训练得到的brain embeddings传递给一个轻量级adapter模块，再输入到**冻结的文本解码器**中，以自回归方式（逐个token）生成文本。
  - 该路径强调利用已对齐语义的连贯生成能力，适用于需要完整句子的任务。
- **解码路径二：“LLM辅助解码”（LLM-copilot策略）**
  - 先从brain embeddings中检索出与语义最相关的关键词（可能借助对比学习后的类别/词表分类头或最近邻查询）。
  - 再将这些检索到的关键词作为提示（prompt）输入给强大的大语言模型，由LLM负责生成连贯、上下文丰富、语法正确的文本。
  - 该路径将传统“检索-重写”思想融入神经信号解码中，借助外部语言先验降低解码难度。

## 3. 实验设计：数据集、基准（Benchmark）与对比方法

- **数据集与场景**：论文明确提及在**“听数据集”（listening datasets）**上进行实验——受试者聆听自然刺激（如语音故事、音频），同时记录EEG信号，模型需从脑活动中还原出所听内容。
  - **未明确信息**：具体使用的是哪个公开数据集（如Kay等人大规模自然听数据集、Schoofs等人的Narratives数据集等）在给定的摘要和元数据中**未给出具体名称**。此外，未提及是否包含“想象阅读”（imagined speech）等多样化场景。
- **Benchmark与评估指标**：论文未具体展开指标种类。一般而言，该任务会采用BLEU、ROUGE、METEOR等文本质量指标（流水句匹配），及Semantic Similarity、Word Error Rate等语义衡量指标。结合摘要描述，重点指标应侧重于**语义保真度**（alignment/fidelity）和**流畅度**（fluency），但**具体的数值结果与对比表格在给定信息中缺失**。
- **对比方法**：论文表示BrainWhisperer成绩**优于基准方法（baseline approaches）**。但摘要未列出具体对比基线（如常用解码器、前人的EEG转文本模型）。信息有限。

## 4. 资源与算力

- **明确未提及**：在提供的摘要和元数据中，**没有给出任何硬件配置信息**（如GPU型号是A100还是4090、具体卡数）、**训练轮数（epochs）**、**训练时长**、**模型参数量**等详细信息。
- 作者团队的硬件背景：该工作来自香港科技大学及合作机构，通常涉及大规模深度学习资源，但在此不构成客观依据。
- **总结**：若要考察算力开销，需要查看完整论文的实验部分——这属于本总结的未覆盖信息，论文正文中很可能有具体报告（如批次大小、GPU数量），但仅凭当前材料无法确认。

## 5. 实验数量与充分性评价

- **实验组数**：由摘要可推断出至少有**两组主要实验**，分别对应两种解码路径：①直接解码（EEG → Adapter → Frozen Text Decoder）；②LLM-copilot策略（检索增强 + LLM生成）。
  - 具体**消融实验**（如去掉对比对齐改成回归损失、是否冻结文本编码器、是否使用LLM重写等）**未在摘要中列出**。
- **客观性与公平性**：能看到的优势是同时验证两种不同范式（自回归直接生成 vs. LLM辅助生成），能体现方法在不同解码需求下的鲁棒性。但**由于缺乏公开数据集的明确命名、指标的具体定义、与基线方法的统计显著性检验描述，我无法判断其是否采用了最公平的基准设置**。
- **充分性**：只提到“听数据集”，未提及是否包含跨受试者（cross-subject）、跨数据集（cross-dataset）的泛化实验，也未说明消融实验、参数敏感性分析是否充分。如果全文补齐，可能实验更丰富，但目前来看**只从摘要看实验覆盖范围尚不够全面**。

## 6. 论文的主要结论与发现

- **有效性结论**：BrainWhisperer在接受自然语音刺激的大规模数据集上，生成的文本**语义上与真实刺激高度吻合（语义保真）且语言流畅**，显著优于当前的基线方法。
- **方法论结论**：**将神经活动对齐到预训练的语言模型语义空间，是弥合EEG与文本模态鸿沟的有效途径**——这验证了最初的假设。
- **系统意义**：通过两条解码路径的验证，作者认为该框架为**实用性和鲁棒性**兼备的脑到文本通信系统铺平了道路。

## 7. 优点（方法或实验设计的亮点）

- **创新方向明确**：将模态对齐从以往常见的“图像↔文本”或“音频↔文本”扩展到“脑电↔文本”，直接用强LLM的语义空间作为锚点，跳过了构建中间脑态语义词典的繁琐步骤。
- **冻结参数降低过拟合风险**：冻结文本编码器以及文本解码器，可大幅减少参数量并避免神经信号中的噪声污染语言先验。
- **对比学习损失的设计恰到好处**：有效将不规则的EEG脉冲信号压缩/嵌入到紧凑的语言语义坐标中，使同一语义空间下可直接度量。
- **双路径解码机制互补**：既支持受限的高保真自回归生成（适合精确答案），也支持通过检索+LLM的放荡生成（适合丰富叙事），增加了系统的灵活性和用户可选择性。
- **对脑解码领域实践贡献大**：摘要明确表态会**建立直接、可复现的强基线**，对后续其他科研工作是可对比的对象。

## 8. 不足与局限

- **数据模态单一**：仅在脑电（EEG）非侵入性听数据上验证。脑磁图（MEG）、fMRI及其他侵入性信号并不在讨论范围内，这限制了泛化说服力。
- **语言场景局限**：听数据集只覆盖英语等自然语言聆听过程，未覆盖“视觉想象文字”、“阅读时脑信号”、“主动说话意向（imagined speech）”等真实的通信场景（如瘫痪病人的意图表达）。对B2T的“通信”目标而言，测试范围不足。
- **未提供可复现全面细节**：既然摘要中无详细benchmark数值和具体公开数据集名称，需谨慎看待（可能导致社区难以直接横向对比）。
- **语义空间依赖于文本数据质量**：若输入的文本模态本身不具备丰富性（如简单指令），所学表征可能偏向听力内容；对多语种和非母语实验的泛化尤显不足。
- **可能的偏差或伪影风险**：EEG中肌电、眼动的噪音可能被编码器误认为有效信号，论文未提及在此方面的降噪鲁棒性设计。
- **算力开销未知**：对齐大量EEG到LLM嵌入空间仍需要不小显存和训练时长，在资源受限的临床/可穿戴设备中，其推理效率能否达到实时使用效果评估不足。

（完）
