---
title: "ELASTIQ: EEG–Language Alignment with Semantic Task Instruction and Querying"
title_zh: ELASTIQ：基于语义任务指令与查询的脑电-语言对齐
authors: "Muyun Jiang, Shuailei Zhang, Zhenjie Yang, Wu Mengjun, Weibang Jiang, Zhiwei Guo, Wei Zhang, Rui Liu, Shangen Zhang, Yong Li, Yi Ding, Cuntai Guan"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=Qkeu3hMoUr"
tags: ["query:eeg-align"]
score: 9.0
evidence: 通过语义任务指令与查询机制生成语言对齐的脑电嵌入，支持跨模态对齐与统一任务解码
tldr: 脑电基础模型虽能提供可迁移表示，但缺少利用语言指令统一不同标签和任务的能力。ELASTIQ提出脑电-语言对齐基础模型，以任务感知语义指导产生结构化且语言对齐的脑电嵌入，其语义指令与查询机制让多个标签和任务共享语言先验。实验显示这种对齐显著增强脑电解码的鲁棒性与跨任务迁移性。该工作表明语言语义知识可为脑电表示学习提供统一且可查询的约束。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有脑电表示学习难以引入语言指令作为先验，限制了用语义知识统一各种标签和任务。
method: 提出语义任务指令与查询机制，将脑电嵌入与语言语义对齐，形成可统一多标签多任务的基础模型。
result: 脑电解码鲁棒性和跨任务迁移能力得到提升，证明语言约束对表示学习有帮助。
conclusion: 语言语义指令可以成为脑电表示学习的通用先验，为统一脑电解码任务提供方向。
---

## Abstract
Recent advances in electroencephalography (EEG) foundation models, which capture transferable EEG representations, have greatly accelerated the development of brain–computer interfaces (BCI). However, existing approaches still struggle to incorporate language instructions as prior constraints for EEG representation learning, limiting their ability to leverage the semantic knowledge inherent in language to unify different labels and tasks. To address this challenge, we present ELASTIQ, a foundation model for EEG–Language Alignment with Semantic Task Instruction and Querying. ELASTIQ integrates task-aware semantic guidance to produce structured and linguistically aligned EEG embeddings, thereby enhancing decoding robustness and transferability. In the pretraining stage, we introduce a joint Spectral–Temporal Reconstruction (STR) module, which combines frequency masking as a global spectral perturbation with two complementary temporal objectives: random masking to capture contextual dependencies and causal masking to model sequential dynamics. In the instruction tuning stage, we propose the \textbf{Instruction-conditioned Q-Former (IQF)}, a query-based cross-attention transformer that injects instruction embeddings into EEG tokens and aligns them with textual label embeddings through learnable queries.
We evaluate ELASTIQ on 20 datasets spanning motor imagery, emotion recognition, steady-state visual evoked potentials, covert speech, and healthcare tasks. ELASTIQ achieves state-of-the-art performance on 14 of the 20 datasets and obtains the best average results across all five task categories. Importantly, our analyses reveal for the first time that explicit task instructions serve as semantic priors guiding EEG embeddings into coherent and linguistically grounded spaces. The code and pre-trained weights will be released.

---

## 论文详细总结（自动生成）

# ELASTIQ：基于语义任务指令与查询的脑电-语言对齐模型——论文总结

## 引言

本文介绍了一种脑电与语言跨模态对齐的基础模型 **ELASTIQ（EEG–Language Alignment with Semantic Task Instruction and Querying）**，在脑电解码中引入语言指令作为语义先验，以统一多标签、多任务的表示学习。以下从问题动机、方法设计、实验设置与结果、算力资源、实验充分性、主要结论、亮点及局限等方面进行系统总结。

## 1. 核心问题与研究动机

- **研究背景**：脑电（EEG）基础模型能够捕获可迁移的EEG表征，为脑机接口（BCI）研究带来显著进展，但大多数现有方法未能将 **语言指令** 作为先验约束引入EEG表示学习。
- **核心问题**：不同类型脑电任务（如运动想象、情绪识别、隐性语音等）的标签体系差异大，语言中所蕴含的语义知识难以被现有模型利用，从而无法在统一的语义空间中对齐各类任务与标签，限制了模型的跨任务泛化与解码鲁棒性。
- **整体意义**：ELASTIQ 首次系统性地探索 **“语言作为统一先验”** 的EEG基础模型范式，旨在为多任务脑电解码提供一条利用丰富语义知识的新路径。

## 2. 方法论：核心思想与关键技术

### 2.1 总体架构

ELASTIQ 采用“两阶段”训练范式：
1. **预训练阶段**：通过自监督学习建立结构化的EEG时序与谱特征表征；
2. **指令微调阶段**：引入语言指令模态，通过跨注意力机制将EEG表征与文本标签表征对齐，完成统一解码。

### 2.2 预训练：联合谱-时重建模块（STR, Spectral–Temporal Reconstruction）

该阶段的核心设计包含**多粒度的掩码重建机制**：

- **频率掩码（频率遮蔽）**：作为一种**全局频谱扰动**，迫使模型学习不依赖特定频段的鲁棒性表征；
- **随机时序掩码**：用于捕获EEG信号的**上下文依赖关系**；
- **因果时序掩码**：用于建模数据中的**时序动态演化规律**。

三者结合形成“谱-时联合重建”目标，目的是使模型在不同频域扰动和时序遮挡的条件下仍能重建原始信号，从而学到跨尺度、跨时间的强表征基础。

### 2.3 指令微调：指令条件Q-Former（IQF, Instruction-conditioned Q-Former）

- **核心结构**：IQF 是一种基于可学习查询（query）的**交叉注意力Transformer**。
- **关键流程**（文字说明）：
  1. 将**任务指令（task instruction）的嵌入向量**注入到EEG token序列中，使其携带类别/任务的文本语义信息；
  2. 通过可学习查询向量不断从EEG token中“提取”与语义相关的特征；
  3. 将提取到的特征与**文本标签（label）对应的嵌入表示**进行对齐优化，最终在共享语义空间中统一表示EEG与文本。

正是通过这种“指令语义引导 + 查询对齐”的机制，ELASTIQ使不同任务类型、不同标签体系的任务能够共享同一语言先验。

## 3. 实验设计

### 3.1 数据与场景

- 共覆盖 **20个数据集**，涵盖 **5大类任务**：
  - **运动想象（Motor Imagery）**
  - **情绪识别（Emotion Recognition）**
  - **稳态视觉诱发电位（SSVEP）**
  - **隐性语音（Covert Speech）**
  - **医疗健康（Healthcare）相关任务**
- 这种跨任务的数据设置是对EEG基础模型通用性的广度测试，也是该工作的基准（benchmark）核心。

### 3.2 对比方法

- 论文表述为在统一基准下与现有EEG基础模型/脑电解码方法比较；
- 具体对比方法名称在所提供的文本摘要中未列出，但其性能以 **state-of-the-art (SOTA)** 的形式进行了比较。

### 3.3 评估指标与结果

- **性能表现**：
  - 在 **20个数据集中，有14个数据集** 上取得了SOTA结果；
  - 在全部 **五个任务类别** 上获得最佳平均性能；
- 实验结果整体支持“跨任务的语言对齐表征能显著提升解码性能与鲁棒性”。

## 4. 资源与算力

- 论文提供的资料**未明确说明所使用的GPU型号、数量及训练时长**等算力配置。
- 结论中仅提到将公开发布代码与预训练权重，未给出具体的硬件资源指标。如需进行复现或评估可行性，需要进一步查阅论文原文或作者的补充材料。

## 5. 实验数量与充分性评估

- **实验规模充分性**：共20个数据集、5种任务类别，实验覆盖范围广泛，从稳态视觉诱发电位、运动想象到情绪识别和医疗健康任务，既包含传统BCI任务也涵盖临床医疗场景，具备较强的**生态效度**。
- **验证深度**：不仅在多个数据集上检验了整体性能，还进行了“语义先验引导EEG嵌入”的分析性实验，并得到了有价值的现象发现。
- **公平性考量**：由于摘要没有涵盖对比方法细节、评测协议、消融实验的完整矩阵，尚难以完全确认其公平性，但从任务覆盖与结果分布来看，研究设计的体系化程度较好。
- **客观性结论**：若结合“20个数据集中的14个SOTA”和“全部任务类别平均最优”两个指标来看，结论具有一定的说服力，但仍需关注未公开实验细节可能带来的风险。

## 6. 主要结论与发现

- **首先证实了“任务指令语义先验”的可行性**：本研究通过分析实验首次观察到，显式任务指令可以作为一种语义先验机制，将EEG嵌入空间引导到一个**语义一致且具备语言学基础的空间**中。
- **语言语义约束有效**：将脑电信号与语言标签嵌入进行对齐后，不同标签与任务可以在统一的表征空间中共享语义信息，这对于提升脑电解码鲁棒性具有明显作用。
- **基础模型潜力的延伸**：语言可以作为一种全新的“跨任务约束先验”，为脑电基础模型的统一解码提供了新的架构与范式思路。

## 7. 优点与亮点

- **方法设计新颖度高**：将“语义任务指令”与“查询式跨注意力”作为EEG基础模型的统一建模手段，突破了传统脑电模型只关注时间-空间域的局限。
- **预训练任务互补性强**：频率掩码（全局扰动）与随机掩码、因果掩码（局部时序与上下文建模）三个任务互为补充，更好地覆盖了EEG在谱、时域+不同自监督一致性层面的特征。
- **消融设计思路清晰**：通过分析实验首次发现语言指令语义空间与EEG嵌入空间的对应关系，这种“分析验证”在脑电基础模型论文中相对少见，具备较好的科学贡献。
- **任务覆盖面广**：覆盖传统BCI、情感计算与临床健康领域，提升了模型推广的多样性和潜在应用场景。

## 8. 不足与局限

- **算力与实现细节缺失**：未报告模型参数量、训练算力消耗及单位，可能影响工业界/学术界的复现成本评估。
- **对比细节与基准透明度不足**：从摘要与提取文本中，未提供对比方法的版本、试次划分、数据预处理及统计显著性检验等信息，实验公平性仍需进一步核验。
- **消融实验呈现不全面**：STR 与 IQF 各自贡献的独立量化分析在摘要中没有细节，若要确认每部分设计的必要性，还需阅读原论文或附加实验。
- **语言指令的适用范围有限**：目前验证的任务虽然多样，但仍是以“任务级语言指令”为前提。对于更抽象、更开放式的临床问题（如疾病自动诊断报告生成），其效果尚未得到验证。
- **隐私与安全性讨论缺失**：脑电数据本身属于高敏感性生物数据，引入语言对齐与统一表示后如何保证跨域使用中的数据安全，是需要更多研究的边界议题。

## 总体评价

ELASTIQ 提出了一种结构清晰、语义引导机制明确的EEG-语言对齐方案。它不仅在多项数据集上取得高性能，还为“脑电解码如何与自然语言语义融合”提供了新思路。虽然原文仍有一些实验细节与资源算力未公开，但其大规模多任务评估以及“指令语义作为先验”的验证方式，为脑电基础模型的泛化与应用打开了新的方向。

（完）
