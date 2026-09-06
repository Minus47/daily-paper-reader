---
title: "MTSSRL-MD: Multi-Task Self-Supervised Representation Learning for EEG Signals across Multiple Datasets"
title_zh: MTSSRL-MD：跨多个EEG数据集的面向EEG的多任务自监督表征学习
authors: "I-Hui Li, Oscar Tai-Yuan Chen, Vincent S. Tseng"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=qD4N15aL2W"
tags: ["query:eeg-align"]
score: 9.0
evidence: 跨多数据集进行EEG多任务自监督表征学习，学习更具信息性和泛化性的EEG特征
tldr: EEG表征学习受标签稀缺和异构采集配置影响，单一小规模数据集泛化能力不足。该文提出MTSSRL-MD，在多个数据集上联合进行多任务自监督学习，缓解了标注少和电极布局不一致的问题。在睡眠分期任务中，模型对少数类别和转迁期样本的分类得到改善。这为跨数据集的EEG预训练复用提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG有标注数据少、采集布局异构，单数据集训练易导致少数类泛化差。
method: 在多个EEG数据集上设计多任务自监督学习目标，学习通用表征后再用于下游分类任务。
result: 在睡眠分期等任务上显著提升少数类别与过渡阶段的分类性能，超越单数据集模型。
conclusion: 验证了跨数据多任务自监督能有效提升EEG表征的通用性及下游分类效果。
---

## Abstract
Electroencephalography (EEG) supports diverse clinical applications. However, effective EEG representation learning remains difficult because scarce label annotations and heterogeneous EEG montages limit the scale of available datasets. In practice, single and small-scale datasets often result in models with poor generalization, particularly for underrepresented classes with limited samples, which are harder to learn reliably. These challenges become even more critical in the EEG-based sleep stage classification task, especially for minority stages that are not only scarce but also transitional with overlapping characteristics, which makes them prone to misclassification. In this work, we propose MTSSRL-MD (Multi-Task Self-Supervised Representation Learning for EEG Signals across Multiple Datasets), a unified framework that combines multi-dataset and multi-task self-supervised pretraining with a channel alignment module to alleviate the impact of scarce labels, heterogeneous EEG montages, and small-scale datasets that often cause poor generalization. This design enables the learning of EEG representations that are generalizable. Multi-dataset learning provides broader feature diversity that facilitates more robust cross-dataset generalization. A spatial-attention Channel Alignment Module (CAM) projects heterogeneous EEG montages into a shared channel space and provides spatial weights that highlight regions aligned with standard EEG montages, offering interpretability. Complementary self-supervised tasks—augmentation contrastive, temporal shuffling discrimination, and frequency band masking—provide temporal and spectral information that improve robustness on these underrepresented classes. Experiments on three heterogeneous EEG sleep datasets show that MTSSRL-MD consistently outperforms single-dataset SSRL baselines and even surpasses SeqCLR, a representative multi-dataset single-task SSRL method, particularly under low-label conditions, demonstrating the effectiveness of integrating multi-dataset and multi-task learning for EEG-based sleep stage classification. Besides classification performance, MTSSRL-MD achieves more efficient inference than single- and multi-dataset SSRL baselines. Moreover, the unified design of our proposed method allows the use of a single pretrained encoder to be fine-tuned across diverse datasets, highlighting efficiency and practical value for clinical research, suggesting strong potential for deployment in real-world settings.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **研究背景**：EEG 在临床诊断中应用广泛，但 EEG 表征学习面临两个突出障碍：**标注稀缺**（睡眠分期等任务需专家逐段标注）与**采集配置异构**（不同中心采用不同电极布局/montage、不同采样率等）。这些因素使大规模数据集难以构建。
- **核心痛点**：现有大多数方法依赖**单一小规模数据集**训练，模型泛化能力较差，尤其对样本量少、具有过渡性和重叠特征的少数类别（如睡眠分期中的 N1 期与 REM 期）极易误分类。
- **关键问题**：本文要解决的核心问题是——**如何在标签稀缺、电极布局异构、数据规模有限的条件下，学习跨数据集通用、且对少数类稳定的 EEG 表征**。
- **整体含义**：该工作将自监督预训练从单数据集扩展至多数据集、从单任务扩展至多任务，并引入通道对齐，从而为跨数据集的 EEG 预训练模型复用提供了新范式。

---

## 2. 方法论

### 2.1 核心思想
提出了 **MTSSRL-MD（Multi-Task Self-Supervised Representation Learning for EEG Signals across Multiple Datasets）**，一个将**多数据集联合预训练、多任务自监督目标、通道对齐**三者融合的统一框架。

### 2.2 关键技术细节

- **通道对齐模块（Channel Alignment Module, CAM）**：
  - 采用**空间注意力机制**，将异构 EEG 电极布局投影到一个**共享通道空间**；
  - 同时输出空间权重，突出与标准电极布局对应的脑区，提供一定**可解释性**。
- **多任务自监督学习目标（互补的三个任务）**：
  1. **增强对比学习**：学习对噪声与个体差异鲁棒的表征；
  2. **时间乱序判别**：判断时间片段顺序是否被打乱，捕捉时间动态信息；
  3. **频带掩码预测/重建**：掩盖特定频带信息，迫使模型学习频谱结构。
- 以上三种任务分别补充了**时间信息与频谱信息**，共同强化模型对少数类的判别能力。
- **训练流程（文字描述）**：
  1. 将多个数据集原始 EEG 信号输入模型；
  2. 对异质蒙太奇先经过 CAM 做通道对齐，映射到共享通道空间；
  3. 在同一共享空间下联合优化上述多个自监督目标，得到统一的预训练编码器；
  4. 下游任务中，用**同一个预训练编码器**针对不同数据集进行微调，做睡眠分期分类等任务。

---

## 3. 实验设计

- **下游场景**：EEG 睡眠阶段分类（sleep stage classification）。
- **数据集**：使用了 **3 个异构的 EEG 睡眠数据集**（具体数据集名称在摘要中未列出），存在不同通道布局、设备与规模差异。
- **Benchmark / 对比方法**：
  - **单数据集自监督学习基线（single-dataset SSRL baselines）**：即在单个数据集上预训练的同类自监督方法；
  - **SeqCLR**：作为**多数据集单任务自监督学习**的代表方法（在 EEG 领域有代表性的对比学习框架）。
- **实验条件**：特别考察了 **低标签率（low-label conditions）** 下的性能表现，即下游微调标签量很少的更严苛情况。
- **评估维度**：分类准确率、对少数类与过渡期阶段的分类改善情况、跨数据集泛化能力，以及**推理效率**。

---

## 4. 资源与算力

- 论文摘要在当前提供的内容中**未明确提及**所使用的 GPU 型号与数量、训练时长、参数量等具体算力信息。
- 仅可推断：由于涉及多个数据集和多任务预训练，预训练阶段计算开销预计较高；但文中强调该方法在**下游微调的推理效率上优于单数据集与多数据集基线**——此处“效率”也可能指“统一编码器复用带来的总训练成本分摊”。
- 若需要完整算力细节，需查阅论文实验设置部分的完整版本。

---

## 5. 实验数量与充分性

- 根据摘要反映的实验内容，至少包含：
  - 3 个异构数据集上的跨数据集实验；
  - 与单数据集 SSRL 基线的对比；
  - 与多数据集单任务 SSRL 代表方法 SeqCLR 的对比；
  - 低标签率条件下的专项评测；
  - 对少数类和过渡期类别的分类效果分析；
  - 推理效率对比。
- **充分性与客观性评估**：
  - 实验设计覆盖了**方法核心主张**（多数据集 + 多任务优于单数据集 + 单任务），对比合理；
  - 但受限于摘要文本，当前没有足够信息确认是否做了完整的消融实验（例如删除 CAM 或移除某一自监督任务的组合消融）、统计显著性检验，或对各类别精细的混淆矩阵分析；
  - “效率更高”的描述仅凭摘要难以判断测量口径是否统一（如是否在相同硬件与批次条件下测量）。
- 总体来看，实验设计方向**较为充分且针对性强**，但详细证据仍需查看正文。

---

## 6. 主要结论与发现

1. MTSSRL-MD 在三个异构 EEG 睡眠数据集上**一致优于单数据集 SSRL 基线**，表现出更强的跨数据集泛化能力。
2. 在低标签率条件下，MTSSRL-MD 甚至**超越了多数据集单任务 SSRL 方法 SeqCLR**，说明多任务学习引入了互补的归纳偏置。
3. 对**少数类别（如过渡性睡眠阶段）** 的分类性能获得显著提升，恰是对传统方法最困难的部分。
4. 该方法的统一设计可使**一个预训练编码器适用并微调于多个不同数据集**，具有较高的实际部署价值与临床转化潜力。

---

## 7. 方法优点

- **多数据集联合训练**：突破了单数据集规模限制，学习到的特征多样性更丰富，泛化性更强。
- **多任务互补自监督**：对比学习 + 时间判别 + 频带掩码，同时捕获空间、时间与频谱三种维度的表征，显著改善信息不足的少数类判识。
- **通道对齐模块（CAM）创新性强**：
  - 结构上解决电极异质性问题，使多数据集联合训练成为可能；
  - 注意力权重具有物理含义（对应脑区），提供可解释性，优于隐式适配策略。
- **统一预训练编码器**：一个模型对应多个数据集，减少重复预训练成本，在临床与真实应用中的实用价值突出。
- **两阶段范式清晰**：预训练与微调解耦，适用于标签稀缺的医学信号场景。

---

## 8. 不足与局限

- **数据集范围有限**：仅用了 3 个睡眠数据集，且限于睡眠分期场景；对更广泛的 EEG 任务（如癫痫检测、运动想象）是否同样有效尚不清楚。
- **数据集信息缺失**：摘要未给出各数据集的具体规模、通道数目与采样方式，无法判断异构性的跨越幅度有多高。
- **算力信息不明**：未报告 GPU 类型、训练耗时与参数量，预训练阶段的成本在经济性上无法评估。
- **少数类的定义差异**：不同睡眠分期体系对过渡期类别的标注口径不一，模型性能提升是否依赖特定标注方案需要验证。
- **无消融实验的信息披露**：难以判断 CAM 与三个自监督任务各自的贡献占比，多任务之间是否存在冗余或冲突也需要文本进一步支持。
- **真实临床验证缺失**：摘要未提及由临床专家评估或真实临床数据集上的外部验证，生态效度有待加强。
- **潜在评估偏差风险**：在对比 SeqCLR 时，是否完全控制了相同的预训练 epoch、超参数与下游微调预算，仅在摘要层面无法核实。

---

（完）
