---
title: "Neuro-KE: Scaling EEG Foundation Models via Label-Free Knowledge Integration"
title_zh: Neuro-KE：通过无标签知识集成扩展EEG基础模型
authors: "Hanrui Chen, Haotian Deng, Xiang Chen, Kexin Lou, Shinan Wang, Chen Wei, Quanying Liu"
date: 2026-01-24
pdf: "https://openreview.net/pdf/77274615f21aaae77f95a71dbcedaa417155232c.pdf"
tags: ["query:eeg-align"]
score: 9.0
evidence: 将时域/频域等信号处理先验知识融入EEG基础模型预训练来学习通用神经表征
tldr: 现有EEG基础模型多依赖原始信号重建或稀缺监督标签，忽视了信号处理领域的先验知识。Neuro-KE提出无标签即插即用的知识引擎，将时域、频功率、频结构与频比值四类知识蒸馏进EEG预训练。该方法可在无人工标注情况下融入历史专家经验，有望提升EEG基础模型在下游任务上的表征质量与泛化能力。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG基础模型常只做原始信号重建或使用稀缺监督，忽略了可用的信号处理领域先验知识，限制表征泛化。
method: Neuro-KE作为即插即用无标签框架，把时域、频功率、频结构和频比值知识蒸馏到EEG预训练中。
result: 该框架可在无标签条件下利用信号处理专长提升EEG基础模型的表现。
conclusion: 说明以信号特征作为先验知识可以改善EEG基础模型的预训练与下游能力。
---

## Abstract
Foundation Models for Electroencephalography (EEG) have shown promise in learning generalized representations from large-scale datasets.
However, current approaches primarily rely on raw signal reconstruction or scarce supervised labels, often neglecting the rich, domain-specific prior knowledge encapsulated in decades of signal processing research.
In this work, we introduce Neuro-KE (Neuro-Knowledge Engine), a plug-and-play, label-free framework designed to seamlessly integrate comprehensive signal characteristics into the pre-training of EEG foundation models.
Neuro-KE aggregates a 4-domain knowledge including Time Domain, Frequency Power, Frequency Structure, and Frequency Ratios features, distilling historical expertise into a unified knowledge base.
We demonstrate the versatility and effectiveness of Neuro-KE across three mainstream technical paradigms: Masked Modeling, Contrastive Learning, and EEG--Large Language Models.
Extensive experiments show that Neuro-KE significantly enhances model generalization and robustness, particularly in label-scarce downstream tasks, offering a rigorous pathway to embed domain-invariant signal dynamics into modern deep learning architectures.

---

## 论文详细总结（自动生成）

根据提供的论文元数据与摘要，该论文尚未给出完整正文，本次总结将严格基于现有内容进行信息提取，并对缺失信息做明确标注。

# Neuro-KE 论文中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：脑电图（EEG）基础模型在从大规模数据集中学习通用表征方面展现出前景，是脑科学领域的重要发展方向。
- **核心问题**：当前 EEG 基础模型的预训练方式主要依赖两种路径——原始信号的直接重建，或者依赖稀缺的人工监督标签——这两种方式都忽视了 EEG 信号处理领域数十年来积累的、丰富且高度专业化的先验知识。
- **研究意义**：如果能够系统性地将这些信号处理领域的先验知识嵌入 EEG 基础模型，将有助于模型学到更高质量、更稳健且更具泛化能力的神经表征。

## 2. 方法论：核心思想与技术细节

- **方法名称**：Neuro-KE（Neuro-Knowledge Engine，神经知识引擎）。
- **总体框架**：一个即插即用、无标签的辅助框架，在不改变原有预训练范式的前提下，将领域知识注入 EEG 基础模型的预训练过程。
- **核心思想**：将长期积累的“历史专家经验”转化为统一的知识库，再通过蒸馏的方式集成到 EEG 基础模型的训练中，从而摆脱对人工标注的依赖。
- **知识体系构造**：Neuro-KE 将经典信号处理先验知识归纳为四个领域的特征：
  - **时域特征**：如波形形态相关的时序统计量；
  - **频功率特征**：各频带（如 delta、theta、alpha、beta、gamma）的功率信息；
  - **频结构特征**：功率谱中的结构形态描述；
  - **频比值特征**：不同频段之间的功率比值（常用于刻画脑状态，如 alpha/beta 比值）。
- **集成方式**：将上述四类特征汇总整合为统一的知识底座，再通过知识蒸馏方式对齐/约束 EEG 基础模型的表征学习。
- **算法流程**（文字说明，论文未提供公式）：首先从训练 EEG 数据中离线提取四域信号特征，构建“专家知识库”；随后在 EEG 基础模型的预训练过程中，让模型的中间表征或输出与知识库中的知识信号对齐；整个过程无需任何人工标注，属于自监督/无监督层面的知识注入。
- **适用范围**：该框架与具体模型结构解耦，可作为通用模块接入三类主流 EEG 基础模型范式中：
  - 掩码建模（Masked Modeling）；
  - 对比学习（Contrastive Learning）；
  - EEG-大语言模型（EEG–LLM）架构。

## 3. 实验设计

- **验证场景**：论文声称在三种主流技术范式（掩码建模、对比学习、EEG-大语言模型）下验证了方法的通用性和有效性。
- **任务侧重**：着重考察下游任务在标签稀缺（label-scarce）条件下的表现，即验证知识注入是否能够弥补标注不足带来的性能下降。
- **数据集与 Benchmark**：由于只提供摘要，该部分无法确认以下关键信息：
  - 论文中具体使用了哪些公开数据集；
  - Benchmark 的对比基线是哪些代表性 EEG 基础模型；
  - 具体报告的指标与设置窗口。
- **对比方法**：论文没有明确列出对比方法清单，仅从表述推断其对比基线应是未加入 Neuro-KE 的原始预训练模型（即消融式对比）。

## 4. 资源与算力

- **未说明**：摘要与元数据中**均未披露**所使用的 GPU 型号、GPU 数量、训练时长、参数量级以及能耗等计算资源信息。
- 这一点只能在论文全文的“实验设置”部分进行补充确认。

## 5. 实验数量与充分性

- **可见实验信息有限**：由于仅能获取摘要，目前只可确认论文至少在三个技术范式（掩码建模、对比学习、EEG-LLM）上做了验证，可推测为至少包含每组范式主实验及各模型的接入前后对比。
- **充分的方面**：覆盖三种技术范式中的主流类型，若确在三组独立架构上均报告了令人信服的一致的性能提升，那么在表示不同架构背景下方法有效性的维度上是可观的。
- **不充分/待确认的方面**：
  - 数据集数量未知；
  - 未见多数据集交叉验证的具体表述；
  - 有无显式的消融实验、知识域组合分析无法从摘要确认；
  - 无法评估其在更现实的噪声数据、个体差异大、跨域迁移等场景的表现；
  - 公平性需要看其相对基线的具体数据以及统计显著性的报告。
- **总体评估**：方案维度（多范式框架）具备较好的系统性论证基础，但仅凭当前信息无法完全确认实验覆盖的完备性与基准选择的公平性。

## 6. 主要结论与发现

- **核心结论**：Neuro-KE 能够显著增强 EEG 基础模型的**泛化能力**和**稳健性**，尤其是在下游标注稀缺时收益尤为明显。
- **机理解释方向**：将“领域不变的信号动态”（domain-invariant signal dynamics）嵌入现代深度学习架构是一条可行且有效的路径。
- **定位**：该工作提供了一种严谨、无标签且即插即用的方法来弥合传统信号处理知识与深度学习模型之间的鸿沟。

## 7. 优点与亮点

- **极低标注依赖**：不依赖人工标签，在大规模无标注EEG数据上可以直接使用。
- **即插即用**：方法作为外部知识引擎，不强制改变现有基础模型的架构，能够较容易地迁移到不同主流架构（尤其是还覆盖了较新的 EEG-LLM）。
- **知识来源有据**：利用的是神经科学/信号处理几十年的成熟经验（时域、频域），而非从零学到的隐含特征，具有较好的可解释性和科学依据。
- **多域知识覆盖**：不是单一特征，而是同时纳入时域与多视角频域特征（能量、结构、比值），知识维度较丰富。
- **问题切入点好**：目前 EEG 基础模型多关注数据规模，对“如何注入专家知识”关注度不足，本研究填补了这一缺口。

## 8. 不足与局限

- **核心细节不可验证**：由于摘要不含数据集说明、对比基准与性能数值，无法判断其报告收益的大小与显著性。
- **频域先验可能涉及设备与个体迁移风险**：不同采集设备、不同参考电极配置下的频谱功率与频比值等特征存在明显分布差异，该方法对这类领域偏移的鲁棒性未知。
- **蒸馏机制可能带来信息瓶颈**：若知识蒸馏方式设计不当，容易将传统信号处理中的固有噪声或有限假设（如线性平稳假设）引入原本可学习的深度表征中，反而可能限制上限。摘要中对此没有分析。
- **信号特征时间尺度受限**：时域/频域知识多为短时段特征，对大规模基础模型建模长程依赖或跨时间段演变的作用路径需要在原文中交代，但目前没有体现。
- **算力开销未见披露**：提取多域特征与额外对齐损失会增加训练成本，论文未说明额外负载是否可接受。
- **局限性交代不明**：未阅读正文前，无法评估作者是否报告了方法失败的模式（例如：对哪些下游任务无增益，以及对多少数据规模下收益消退等）。
- **缺少医学/临床场景交代**：EEG 下游任务差异大（睡眠分期、癫痫检测、情绪识别等），不同任务是否都能从频域知识中等价获益？摘要中仅泛泛以“下游任务”概括，并未分类讨论。

> **注**：上述第 3、4、5、8 部分中的多处“未知项”是因为输入材料只包含论文摘要与元数据而非全文，并非刻意省略；要得到完整回答，需要补充论文正文进行核对。

（完）
