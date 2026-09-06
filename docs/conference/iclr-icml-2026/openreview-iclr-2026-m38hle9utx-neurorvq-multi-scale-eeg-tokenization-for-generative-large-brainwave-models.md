---
title: "NeuroRVQ: Multi-Scale EEG Tokenization for Generative Large Brainwave Models"
title_zh: NeuroRVQ：面向生成式大型脑波模型的多尺度EEG标记化
authors: "Konstantinos Barmpas, Na Lee, Alexandros Koliousis, Yannis Panagakis, Dimitrios Adamos, Nikolaos Laskaris, Stefanos Zafeiriou"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=m38Hle9Utx"
tags: ["query:eeg-align"]
score: 8.0
evidence: 面向EEG表征学习的多尺度离散标记化方法，解决高频动态保持问题
tldr: 现有EEG标记化方法难以保留高频动态，制约了基础模型学习通用表征。文章提出NeuroRVQ，一种以码本为中心的生成式大型脑波模型标记器，集成多尺度特征提取与层次化量化来覆盖完整神经频谱。该方法能提高脑电重建保真度，并为下游EEG分类与对齐任务提供更丰富、更可用的神经表征。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有EEG神经标记器无法保留高频动态，限制了大型脑波模型的表征学习与信号重建能力。
method: 提出NeuroRVQ码本标记器，集成多尺度特征提取和层次化量化以覆盖完整多频率神经频谱。
result: 该方法能改善脑电重建保真度，为下游任务提供保留高频动态的离散神经表征。
conclusion: 高质量的脑电标记化是提升EEG基础模型表征泛化能力与实用价值的关键。
---

## Abstract
Electroencephalography (EEG) captures neural activity across multiple temporal and spectral scales, yielding signals that are rich but complex for representation learning.  Recently, EEG foundation models trained to predict masked signal-tokens have shown promise
for learning generalizable representations. However, their performance is hindered by their signal tokenization modules. Existing neural tokenizers fail to preserve high-frequency dynamics, limiting their ability to reconstruct EEG signals with high fidelity. We introduce NeuroRVQ, a scalable Large Brainwave Model (LBM)  centered  on a codebook-based tokenizer. Our tokenizer integrates: (i) multi-scale feature extraction modules that capture the full frequency neural spectrum; (ii) hierarchical residual vector quantization (RVQ) codebooks for high-resolution encoding; and, (iii) an EEG signal phase- and amplitude-aware loss function for efficient training. This design enables efficient EEG compression while supporting accurate reconstruction across all frequency bands, leading to robust generative masked modeling. Our empirical results demonstrate that NeuroRVQ achieves lower reconstruction error and outperforms existing LBMs on a variety of downstream tasks. More broadly, NeuroRVQ tokenizer establishes a strong prior for codebook-based general-purpose brainwave models, enabling advances in neural decoding, generative modeling and multimodal biosignal integration.

---

## 论文详细总结（自动生成）

好的，我会基于提供的论文内容，为您生成一份结构化的中文总结。

---

## NeuroRVQ：面向生成式大型脑波模型的多尺度EEG标记化 — 论文总结

### 1. 核心问题与整体含义（研究动机与背景）
- **背景**：脑电图（EEG）能够捕捉跨多个时间尺度和频谱尺度的神经活动，信号信息丰富但结构复杂，对表征学习构成挑战。近期，通过预测掩码信号token来训练的EEG基础模型在获取可泛化表征方面展现出潜力。
- **核心问题**：当前EEG基础模型的性能受制于信号标记化（Tokenization）模块。**现有的神经标记器无法有效保留高频动态信息**，这限制了模型对EEG信号的高保真重建能力，并进一步制约了基础模型学习通用表征的能力。
- **整体含义**：高质量的EEG标记化被认为是提升EEG基础模型表征泛化能力与实用价值的**关键**。该研究直接回应了EEG基础模型中的瓶颈——数据压缩与高频信息丢失的矛盾。

### 2. 提出的方法论
- **核心思想**：提出 **NeuroRVQ**，一种以码本为中心的可扩展大型脑波模型（LBM）标记器。其目标是学习一个能覆盖完整多频神经频谱的离散token空间，从而作为通用脑波模型的强先验。
- **关键技术细节**：该标记器集成了三个核心模块：
    1.  **多尺度特征提取模块**：用于同时捕捉覆盖全频段的神经活动动态，以保留高频细节。
    2.  **层次化残差向量量化（RVQ）码本**：通过多层级的残差量化实现高分辨率编码，逐步减少表征误差。
    3.  **EEG信号相位与幅度感知损失函数**：在训练过程中同时考虑信号相位与振幅信息，指导模型更高效地逼近真实信号，以便在所有频段进行精确重建。
- **训练流程**：方法遵循生成式掩码建模策略——模型通过预测被掩蔽的token进行训练。通过结合上述损失函数，NeuroRVQ在有效压缩EEG信号的同时，保证了高保真度重建，进而支持稳健的生成式掩码建模。

### 3. 实验设计
- **数据集与场景**：由于文中未详细说明，**具体使用的数据集与下游任务场景无法确认**。
- **Benchmark**：文中未列出具体的基准测试数据集或评估协议的详细信息。
- **对比方法**：文中明确提到，与**已有的EEG基础模型（LBMs）** 进行了对比，并宣称实现了更低的类别重建误差和更好的下游任务性能。

### 4. 资源与算力
- 论文的给定内容中，**完全没有提及**所用GPU型号、数量、训练时间或总计算量等资源信息。
- 从已知实践中推测，训练基础模型需要大量并行GPU，但这仅为推测，而非文本中的事实。

### 5. 实验数量与充分性
- **文本证据不足**：目前提供的文字只提到“在多种下游任务上表现优异”和“更低重建误差”这类概述性结论，**具体的实验组数、数据集种类、消融研究数量都未列出**。
- **客观性与公平性判断**：由于缺少实验细节，很难从当前文本中客观评估实验是否充分、对照是否公平，无法判断是否存在过度声明或关注点的遗漏。

### 6. 主要结论与发现
- NeuroRVQ设计的码本标记器通过保留全频段动态信息，实现了比现有LBM更低的EEG重建误差。
- 在多种下游任务上（如神经解码、生成式建模、多模态生物信号整合），该方法优于现有的EEG基础模型，被认为是迈向码本类通用脑波模型的有力一步。
- 核心结论重申：**EEG 基础模型的表征泛化能力与实用价值严重依赖于高保真的信号标记化**。

### 7. 优点
- **多尺度整合策略**：方法明确将多尺度特征提取、RVQ 和高保真损失函数整合入一个统一的标记器中，在设计上直面EEG频谱复杂性问题。
- **有潜力的方向定位**：其强调高频动态保留与基础模型之间的因果关系，定位了一个容易被低估的重要研究切口。
- **面向通用性**：论文不仅面向单点分类，更尝试为多生物信号整合与生成式建模奠定基础，具有前瞻性和通用性。

### 8. 不足与局限
- **细节缺失（实验层面）**：提供信息中没有任何关于数据来源、通道数、频段定义、下游任务类型以及基线模型的细节。
- **可复现性受限**：缺少对模型参数量、层数、码本大小与维度等关键架构配置的描述，外加缺少算力说明，复现门槛高。
- **验证偏差风险**：由于只有概要式的“表现更低误差、更优性能”断言，而没有给出具体数值和统计显著性说明，可能带有选择性报告的风险。
- **应用范围限制未讨论**：尽管声称支持神经解码、生成式建模等场景，但文本中并未给出真实病例或低信噪比EEG环境下适用性的讨论。

---

（完）
