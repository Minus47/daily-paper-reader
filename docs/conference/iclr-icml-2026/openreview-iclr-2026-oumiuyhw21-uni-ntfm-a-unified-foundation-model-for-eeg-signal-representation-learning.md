---
title: "Uni-NTFM: A Unified Foundation Model for EEG Signal Representation Learning"
title_zh: Uni-NTFM：面向EEG信号表示学习的统一基础模型
authors: "Zhisheng Chen, Yingwei Zhang, Qizhen Lan, Tianyu Liu, Huacan Wang, Yi Ding, Ziyu Jia, Ronghao Chen, Kun Wang, Xinliang Zhou"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=oUMiuYHW21"
tags: ["query:eeg-align"]
score: 9.0
evidence: 构建面向EEG信号表示学习的统一基础模型
tldr: 现有EEG基础模型多沿用视觉或语言模型架构，把神经信号当作像素网格或token序列，忽略了大脑皮层拓扑上的稀疏编码机制。为此作者提出统一神经拓扑基础模型Uni-NTFM，根据三个神经科学原则设计异构特征投影模块，同时编码时域非平稳瞬态与频域稳态节律。实验表明该模型能更有效地学习EEG通用表征。该工作为EEG表征学习和下游解码提供了大脑启发的基础模型。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 当前EEG基础模型将信号当作像素网格或token序列，未充分利用大脑皮层拓扑结构与稀疏编码机制。
method: 提出Uni-NTFM，以解耦编码原则设计异构特征投影，联合编码时域暂态与频域节律特征。
result: 在EEG表征学习中验证了拓扑神经机制启发的架构带来的表示质量提升。
conclusion: 面向脑拓扑结构建模可提高EEG统一基础模型的表征能力与下游适用性。
---

## Abstract
Current foundation models for electroencephalography (EEG) rely on architectures adapted from computer vision or natural language processing, typically treating neural signals as pixel grids or token sequences. This approach overlooks that the neural activity is activated by diverse sparse coding across a complex geometric topological cortex. Inspired by biological neural mechanisms, we propose the Unified Neural Topological Foundation Model (Uni-NTFM), an architecture rooted in three core neuroscience principles. In detail, to align with the brain's decoupled coding mechanism, we design the Heterogeneous Feature Projection Module. This module simultaneously encodes both time-domain non-stationary transients and frequency-domain steady-state rhythms, ensuring high quality in both waveform morphology and spectral rhythms. Moreover, we introduce a Topological Embedding mechanism to inject structured spatial priors and align different sensor configurations onto a unified latent functional topography, effectively reconstructing the geometry of brain regions. Furthermore, we achieve functional modularization and sparse coding efficiency of biological networks by constructing the Mixture-of-Experts Transformer network. This dynamic routing mechanism assigns different signal patterns and tasks to specialized neural subnetworks, and effectively preventing task interference while increasing the model capacity to record-breaking 1.9 billion parameters. Uni-NTFM is pre-trained on a diverse corpus comprising 28,000 hours of EEG data, and outperforms existing models across nine distinct downstream tasks under both linear probing and fine-tuning settings, demonstrating that aligning model architecture with neural mechanisms is significant to learn universal representations and achieve generalizable brain decoding.} Our code is available at \url{https://anonymous.4open.science/r/Uni-NTFM-0924}

---

## 论文详细总结（自动生成）

# Uni-NTFM: 面向 EEG 信号表示学习的统一基础模型——中文总结

## 1. 核心问题与研究动机

- 现有 EEG（脑电图）基础模型大多直接借用计算机视觉或自然语言处理领域的架构，把神经信号机械地看作“像素网格”或“token 序列”。
- 这种做法忽略了 EEG 信号的根本特性：大脑活动是在**复杂几何拓扑的皮层**上，由**多样化稀疏编码机制**激活产生的。像素或 token 的建模方式未能反映皮层拓扑结构与稀疏编码的关系。
- 因此，作者提出核心问题：**如何设计一个 EEG 基础模型，使其架构本身内嵌大脑神经机制，而非简单迁移通用序列模型？**
- 整体含义在于：EEG 基础模型不应只追求“规模大、数据多”，还应追求**结构与神经科学原理对齐**，从而学出更通用、更可泛化的脑信号表征。

## 2. 方法论：Uni-NTFM 的核心设计与关键技术

Uni-NTFM（统一神经拓扑基础模型）的架构根植于三条神经科学原则，每个原则对应一项核心设计。

### 2.1 原则一：大脑解耦编码机制 → 异构特征投影模块（Heterogeneous Feature Projection Module）

- 大脑对信号采用“解耦编码”方式处理不同类型的信息，该模块效仿这一机制。
- 模块**并行编码两类互补的 EEG 特征**：
  - **时域非平稳瞬态**：对应事件相关电位、棘波等信号，突出波形形态学信息。
  - **频域稳态节律**：对应 α、β、θ 等节律振荡，突出频谱规则性信息。
- 设计意图是保证信号在**波形形态**和**频谱节律**两个维度上都有高质量表征，避免单一编码造成信息损失。

### 2.2 原则二：皮层几何拓扑结构 → 拓扑嵌入机制（Topological Embedding）

- 大脑皮层具有特定的空间几何拓扑，电极分布在头皮上对应着不同脑区。
- 该机制向模型中**注入结构化的空间先验**，帮助模型理解“哪些通道来自空间邻近的脑区”。
- 同时，它可以将**不同设备、不同数量、不同位置的传感器配置对齐到统一的潜在功能拓扑空间**上，从而提升模型跨数据集、跨采集设备的迁移能力。
- 本质上是重建了脑区的几何关系，使模型“知道”信号的空间来源。

### 2.3 原则三：生物网络的功能模块化与稀疏编码 → 混合专家 Transformer（MoE-Transformer）

- 生物神经系统并非所有神经元同时激活，而是通过**动态稀疏路由**募集特定的功能子网络。
- 模型采用**Mixture-of-Experts（MoE）Transformer 架构**来实现这一机制：
  - 通过动态路由机制，EEG 中不同的信号模式/下游任务被分配到专门化的“专家”子网络中。
  - 稀疏激活模式有效**缓解多任务之间的干扰**。
  - 在保证计算可行性的前提下大幅扩展模型容量，使得参数量达到 **19 亿（1.9 billion）**。

### 2.4 训练策略

- 使用 **28,000 小时**的多样化 EEG 语料进行预训练。
- 论文未提供更底层的数学公式或伪代码细节，核心设计思路以上述三大模块的文字描述为准。

## 3. 实验设计

- **预训练数据**：包含 28,000 小时 EEG 数据的多源语料库（具体数据集名称在原文摘要中未展开）。
- **下游任务数量**：覆盖 9 个不同的下游任务。
- **评测协议**：在两种标准协议下进行评测：
  - **线性探测（Linear Probing）**
  - **微调（Fine-tuning）**
- **基线对比**：与多个现有多模态 EEG 基础模型进行对比，Uni-NTFM 在上述 9 项任务中均取得最优/更优结果。
- 由于当前文本仅包含摘要，具体的数据集名称（如：BCI 竞赛数据、睡眠分期数据、癫痫检测数据等）、任务类型划分、基线模型清单均未完全展开。

## 4. 资源与算力

- 论文明确提到的资源信息包括：
  - 预训练语料规模：**28,000 小时 EEG**
  - 模型规模：**1.9 billion 参数（MoE 架构）**
- **但摘要中未明确指出具体算力信息**，例如：
  - GPU 型号（如 A100/H100）
  - GPU 数量
  - 预训练总时长
  - 能耗与显存开销等

因此，现有信息不足以评价其训练成本与算力可行性。

## 5. 实验数量与充分性

- **正向维度**：
  - 预训练数据规模大（2.8 万小时），具有较好的数据多样性基础。
  - 9 个下游任务 × 2 种评测协议，构成至少 9 组主实验，外加不同任务之间的对比，从广度上看较为充足。
- **可提升维度**：
  - 当前文本（仅摘要）没有给出具体准确率/性能数值，无法独立判断提升幅度。
  - 缺少明确的**消融实验**描述，读者难以界定三大模块（异构投影、拓扑嵌入、MoE）各自的贡献占比。
  - 缺少对基线模型公平性设置的说明（如是否使用相同预训练数据量、相同参数量约束等）。

从摘要可见实验覆盖较广，但受限于信息量，现阶段无法判定为“充分、客观、公平”的完整验证。

## 6. 主要结论与发现

- **架构与神经机制对齐可以显著提升 EEG 表征学习效果**：Uni-NTFM 采用脑启发设计后，在多个下游任务上优于现有基础模型。
- **时域瞬态 + 频域节律并行编码**比单一建模方式更优。
- **拓扑嵌入**使不同设备/电极配置的数据可对齐到统一拓扑空间，提升跨设置泛化潜力。
- **MoE 稀疏路由**能够有效扩大模型容量，同时保持任务间干扰可控，实现通用的脑解码。
- 综合结论：**面向脑拓扑结构建模是提升 EEG 统一基础模型表征能力的关键方向**。

## 7. 优点与亮点

- **新颖的建模视角**：不同于既有工作仅在“模型规模”或“数据规模”上做文章，Uni-NTFM 着眼于将皮层拓扑、稀疏编码等神经科学原理写入架构，是 EEG 基础模型设计的差异化路线。
- **异构双通路编码**：同时关注非平稳瞬态波形与频域稳态节律，信息互补性强。
- **跨设备对齐能力**：拓扑嵌入机制将不同传感器配置映射到共享拓扑空间，对 EEG 领域常见的数据采集设备/导联不一致问题有实际意义。
- **容量扩展与稀疏性兼顾**：MoE 的使用使模型达到 19 亿参数，同时维持稀疏计算效率。
- **大规模预训练 + 多任务验证**：28,000 小时数据规模可观，9 个下游任务覆盖足够宽。代码已开放（匿名链接），利于复现与后续研究。

## 8. 不足与局限性

- **信息不完整**：当前仅能依据论文摘要进行分析，缺少架构详图、损失函数、训练超参数、具体性能数值等，难以对方法做深入的技术评估。
- **数据集透明度不足**：28,000 小时预训练数据未说明来源构成，是否包含临床 EEG、睡眠 EEG、BCI 等多种场景，受众群体是否多样，未见细述，可能存在域偏差风险。
- **消融缺失**：三项核心设计各自贡献多少收益，无定量证据支撑。
- **基线公平性待确认**：对比模型是否在相同数据量和容量条件下进行预训练，未见说明。
- **可解释性与安全性未讨论**：EEG 常用于医疗健康场景，19 亿参数模型的可解释性、推理延迟、隐私合规性、边缘部署可行性均未被摘要覆盖。
- **仅验证了任务层面优越性**：尚未证实这种“神经机制对齐”能在低资源数据、跨设备迁移、长时连续监测等真实临床约束下成立。

（完）
