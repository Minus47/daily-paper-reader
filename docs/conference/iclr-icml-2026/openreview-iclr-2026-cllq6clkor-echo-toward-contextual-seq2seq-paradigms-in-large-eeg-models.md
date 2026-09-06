---
title: "ECHO: Toward Contextual Seq2Seq Paradigms in Large EEG Models"
title_zh: ECHO：大型脑电模型中的上下文序列到序列范式
authors: "Chenyu Liu, Yuqiu Deng, Tianyu Liu, Jinan Zhou, Xinliang Zhou, Ziyu Jia, Yi Ding"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ClLQ6cLkoR"
tags: ["query:eeg-align"]
score: 9.0
evidence: 通过无标注EEG预训练大模型提取通用神经表征，提出解码器中心的序列到序列范式，学习更具表达力的脑电特征。
tldr: 大型脑电模型（LEMs）通常以编码器为中心在大规模无标注EEG上预训练以获得通用表征，但解码器能力不足限制了特征在下游任务中的利用。ECHO提出解码器中心的序列到序列范式，在序列空间中显式建模信号、标签与任务的层级关系，并引入离散支持样本构建上下文线索。该设计有望提升EEG特征的利用率，增强模型在跨任务数据集上的泛化能力，为脑电通用表征学习提供了新路径。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有EEG大模型以编码器为中心预训练，缺少与之匹配的解码器，难以充分利用学到的通用特征。
method: 提出ECHO，将EEG建模转为上下文序列到序列学习，对信号、标签与任务进行层级建模，并用离散支持样本构造上下文线索。
result: 从方法设计上弥补了解码器能力不足，通过统一序列预测提高EEG特征的可用性和跨任务适应能力（摘要未见具体数值）。
conclusion: ECHO为EEG大模型提供了解码器中心的新预训练思路，可促进通用脑电表征在下游任务上的高效转移。
---

## Abstract
Electroencephalography (EEG), with its broad range of applications, necessitates models that can generalize effectively across various tasks and datasets. Large EEG Models (LEMs) address this by pretraining encoder-centric architectures on large-scale unlabeled data to extract universal representations. While effective, these models lack decoders of comparable capacity, limiting the full utilization of the learned features.
To address this issue, we introduce ECHO, a novel decoder-centric LEM paradigm that reformulates EEG modeling as sequence-to-sequence learning. ECHO captures layered relationships among signals, labels, and tasks within sequence space, while incorporating discrete support samples to construct contextual cues. This design equips ECHO with in-context learning, enabling dynamic adaptation to heterogeneous tasks without parameter updates.
Extensive experiments across multiple datasets demonstrate that, even with basic model components, ECHO consistently outperforms state-of-the-art single-task LEMs in multi-task settings, showing superior generalization and adaptability.

---

## 论文详细总结（自动生成）

## ECHO：大型脑电模型中的上下文序列到序列范式 —— 论文总结

### 1. 核心问题与研究动机

- **研究背景**：脑电图（EEG）应用场景广泛，但跨任务、跨数据集的泛化一直是核心挑战。近年来，大型脑电模型（Large EEG Models, LEMs）试图通过在大规模**无标注**EEG数据上预训练**以编码器为中心（encoder-centric）** 的架构，学习通用的神经表征（universal representations），以支撑多种下游任务。
- **核心问题**：现有 LEMs 虽然拥有强大的编码器，却**缺乏容量匹配的解码器（decoders of comparable capacity）**，导致学到的通用特征不能在下游任务中被充分利用，即「表征学得好、用得不充分」的结构性失衡问题。
- **整体含义**：论文认为，单纯的编码器预训练只是解决了特征提取的一半问题，须将范式重心转向解码器一方，才能在异构任务上实现真正高效的通用表征迁移。作者由此提出 ECHO，一种以**解码器为中心（decoder-centric）** 的大型 EEG 模型新范式。

### 2. 方法论：核心思想与关键技术

- **核心思想**：将 EEG 建模从传统「编码器预训练 + 下游微调」重构为**序列到序列（sequence-to-sequence）学习**，以统一的序列预测目标连接预训练与下游任务。
- **技术要点（按摘要可提取的层面）**：
  1. **层级关系建模**：在序列空间中显式建模**信号（signal）、标签（label）、任务（task）**三者之间的层次化关系。这一设计使模型不只学习信号到标签的浅层映射，而是建立包含任务语义在内的更丰富上下文表示。
  2. **离散支持样本构建上下文线索**：引入**离散支持样本（discrete support samples）**，作为上下文提示（contextual cues）注入模型输入序列，为每个样本提供与任务相关的参照信息。
  3. **上下文学习（in-context learning）能力**：借助支持样本与序列化建模，ECHO 能在**不更新参数**的情况下动态适应异质任务。这意味着模型具备类似大语言模型的上下文学习机制：给定少量示例即可切换行为模式以适配新任务。
  4. **算法流程（文字描述）**：输入 EEG 信号 → 与离散化的支持样本及任务描述共同组织为输入序列 → 经解码器中心的 seq2seq 架构 → 以自回归/序列生成方式输出标签或任务相关的目标序列。整个流程以统一的序列预测目标贯穿预训练与下游适配。
- **设计哲学**：作者强调即使使用**基础模型组件（basic model components）**，只要范式得当也能取得优秀效果——凸显架构选择（paradigm）比单纯堆叠模型容量更为重要。

### 3. 实验设计

- **场景**：**多任务（multi-task）设置**下的 EEG 分类/分析任务，涵盖跨多个数据集的泛化与适配场景。
- **数据集与 benchmark**：摘要**未明确列出具体数据集名称**（如 BCI Competition 系列、TUAB、Sleep-EDF 等均未提），仅笼统说明「multiple datasets」。 benchmark 为跨任务/跨数据集的多任务评估协议，对比对象为**当前最优的单任务 LEMs（state-of-the-art single-task LEMs）**。
- **对比方法**：主要是 SOTA 的 encoder-centric 单任务大型脑电模型；此外以「使用基础组件」为条件进行对照，排除模型容量带来的混淆。
- **评估指标**：未在摘要中明确给出（可能为分类准确率或 F1 等），需阅读正文确认。

### 4. 资源与算力

- 摘要及元数据中**未提供任何算力相关信息**，包括 GPU 型号、数量、训练时长、参数量等均未披露。
- 这一点本身也构成可复现性层面的信息缺口。

### 5. 实验数量与充分性评估

- **从摘要可见的实验量**：仅报告了「在多个数据集上的广泛实验」以及在多任务设置下与单任务 SOTA 的整体对比。
- **可判断的充分性**：
  - 摘要层面**未报告具体的实验组数**、消融实验、模型规模对照、跨数据集迁移的逐项数值；
  - 未提及针对各组件（层级建模、离散支持样本、上下文学习机制）的消融验证；
  - 未报告统计显著性检验或多重重复实验设置。
- **总体评价**：实验方向的选取较为合理（多任务 vs 单任务大模型的对比是公平且必要的），但由于本研究为 ICLR 2026 录用论文，摘要篇幅有限，**实验的完整性与客观性需以正文补充的实验表格与细节为准**；仅凭摘要无法充分判断其统计效力与消融覆盖的完备程度。

### 6. 主要结论与发现

- 即使在配备**基础模型组件**的情况下，ECHO 在多任务设置中**持续超越（consistently outperforms）** 当前最优的单任务 LEMs；
- 解码器中心的 seq2seq 范式配合离散支持样本构建的上下文线索，能赋予 LEMs 上下文学习能力，从而实现**无需参数更新的动态任务适应（dynamic adaptation）**；
- 验证了「范式创新可以弥补解码器容量不足」的核心假设，为脑电通用表征的高效下游利用提供了可行新路径。

### 7. 主要优点与亮点

- **问题定位敏锐**：精准指出 encoder-centric 预训练 LEMs 在解码端的结构性短板，属于真实且关键的研究空白；
- **范式转向有洞见**：将 LLM 社区验证成功的 seq2seq 与 in-context learning 机制引入 EEG 领域，跨领域借鉴自然且论据合理；
- **层级建模设计**：对信号、标签、任务的三层关系在序列空间内做显式统一建模，从数据组织层面而非仅网络结构层面解决问题，角度新颖；
- **「基础组件也能超过 SOTA」的论证策略**具有说服力：有效隔离了模型容量的干扰变量，凸显范式本身的价值；
- **免参数更新适应**（in-context adaptation）在脑电领域具有重要的实际意义——脑电数据异质性高、标注稀缺，免微调适应能显著降低落地成本。

### 8. 不足与局限

- **实验信息不足（摘要层面）**：具体数据集名称、样本量、任务类型均未给出，难以判断评测广度是否覆盖睡眠分期、运动想象、情绪识别、癫痫检测等 EEG 主要基准；
- **消融验证缺失**：未说明「层级建模」「离散支持样本」「上下文线索注入」各自的单独贡献度，无法确认哪些组件是性能提升的主因；
- **baseline 范围局限**：仅对比「单任务 LEMs」，未见与其他多任务/通用 EEG 模型或基于 prompt/adaptor 范式的对照，公平性有待在正文中检验；
- **无算力与效率报告**：训练成本、推理开销（尤其 seq2seq 生成式解码可能带来延迟）未披露，影响实用性评估；
- **潜在的应用限制**：seq2seq 生成式解码器在脑电时序建模中可能产生误差累积或推理不稳定，离散化支持样本的构造方式（如何选取、数量、顺序敏感性）也需进一步讨论；
- **适用范围边界**：是否适用于低信噪比、跨个体差异极大的原始 EEG 场景仍需验证；摘要未报告具体性能数值，难以估计实际增益的幅度。

---

（完）
