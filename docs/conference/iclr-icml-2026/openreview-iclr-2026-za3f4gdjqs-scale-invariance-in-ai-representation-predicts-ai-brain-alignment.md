---
title: Scale-Invariance in AI Representation Predicts AI-Brain Alignment
title_zh: AI表征的尺度不变性预测AI与大脑的对齐
authors: "Junjie Yu, Wenxiao Ma, Chen Wei, Jianyu Zhang, Haotian Deng, Zihan Deng, Yi Guo, Quanying Liu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=Za3f4GdjqS"
tags: ["query:abstraction"]
score: 6.0
evidence: 发现嵌入尺度不变性与脑活动对齐有关，为设计类脑和语义表征提供可检验的性质
tldr: 为何某些网络表征与大脑更对齐是类脑AI的关键问题。论文转向嵌入层面的尺度不变性，在60个预训练视觉模型和fMRI自然图像响应上发现尺度不变性越强则对齐越好；大预训练集增强该性质，微调削弱它。这表明尺度不变性可作为构造与脑对齐表征的通用指导原则。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有工作从模型大小或任务设计解释AI表征为何与大脑对齐，忽略嵌入本身的尺度不变性等属性。
method: 利用60个预训练视觉模型和fMRI自然图像响应，分析嵌入尺度不变性与脑对齐程度的关系。
result: 发现尺度不变性越强，与fMRI的对齐越好；更大预训练数据集增强、微调削弱该性质。
conclusion: 说明嵌入层性质是理解和构建类脑表征的关键因素。
---

## Abstract
Understanding why some neural network representations align better with brain activity is essential for uncovering neural coding principles and developing human-like AI. While prior work has largely focused on model-level factors, such as dataset scale and task design, we focus on the rarely explored, yet more in-depth embedding level. Motivated by evidence that scale-invariance is widespread in biological neural systems, we identify it as a key embedding-level property. Analyzing 60 pretrained visual models and fMRI responses to natural images, we find that embeddings with stronger scale-invariance align better with fMRI. Training strategies modulate scale-invariance, with larger pretraining datasets enhancing it and fine-tuning reducing it, thereby affecting alignment performance. These findings establish scale-invariance as a fundamental embedding-level property that links training strategies to brain-like representations and suggest its potential as a guiding principle for designing more human-like AI.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- 该论文旨在回答一个类脑 AI 与神经科学交叉领域的关键问题：**为什么某些神经网络表征与大脑活动更对齐？**
- 已有研究主要从模型级因素（例如模型规模、预训练数据量、任务设计）解释脑对齐差异；本文则转向更细粒度、更少被探索的**嵌入（embedding）层面**，探索表征本身几何/统计性质的影响。
- 其生物学动机来自于“尺度不变性（scale-invariance）”在生物神经系统中广泛存在的证据，作者据此提出：**尺度不变性可能是决定模型表征与大脑对齐程度的一个嵌入级关键属性**。
- 整体含义：如果尺度不变性确实能稳定预测脑对齐，那么它既可以作为理解神经编码原则的新线索，也可以作为设计更类人 AI 表征的一个通用而可检验的指导原则。

## 2. 方法论

- **核心思想**：不直接比较模型在任务上的表现或模型架构，而是检查预训练视觉模型内部产生的嵌入表征在**尺度变换下的不变性程度**，并将这一性质与模型表征对 fMRI 数据的对齐度联系起来。
- **嵌入级性质定义（推测性）**：尺度不变性通常指表征或隐藏状态在乘以一个标量、全局缩放或某种尺度的几何变换后，仍能保持其方向、语义结构或判别信息。具体度量公式在摘要中未给出。
- **分析路径**（依据摘要推断）：
  1. 收集 60 个预训练视觉模型。
  2. 对每个模型提取自然图像输入对应的嵌入表征。
  3. 量化每个嵌入表征的尺度不变性强弱。
  4. 计算每个模型嵌入与人类 fMRI 对同一批自然图像响应的对齐程度（脑对齐分数）。
  5. 考察嵌入尺度不变性强度与脑对齐分数之间的统计关系。
  6. 进一步分析训练策略（预训练数据集规模、微调与否）对尺度不变性及其脑对齐效果的影响。
- **公式与算法流程**：论文摘要未提供完整的数学定义或具体算法步骤，因此无法以文字方式详细重现其度量公式；只能明确其研究路径为“表征性质 → 脑对齐”的关联分析。

## 3. 实验设计

- **模型集**：60 个预训练视觉模型，涵盖不同架构和训练设置。
- **脑数据基准**：人类 fMRI 对自然图像的响应数据。
- **评测指标**：模型嵌入表征与 fMRI 响应的对齐程度（即脑对齐分数）。
- **实验场景/对比对象**：
  - 不是与其他脑对齐预测方法进行直接竞赛，而是比较不同模型在“尺度不变性”这一性质上的差异，并检验其与脑对齐的关联。
  - 训练策略对比：考察更大预训练数据集（例如模型是否在大规模数据上预训练）对尺度不变性的影响；考察微调是否会削弱该性质，进而影响脑对齐。
- **总结**：实验总体上属于大规模预训练模型库上的相关性研究，而非新算法在标准 benchmark 上的竞赛式评测。

## 4. 资源与算力

- **论文摘要及元数据中未明确提及任何算力信息**，包括 GPU 型号、数量、训练时长、显存占用等。
- 由于实验主要通过调用/分析 60 个预训练模型的特征与 fMRI 数据完成，理论上不需要大规模训练计算；但表征提取、相关性统计等仍需一定计算资源，只是原文没有披露。

## 5. 实验数量与充分性

- **实验规模规模**：60 个预训练视觉模型是一个较大的跨模型样本，覆盖多种模型族，增强了结论的普适性。
- **实验维度**：
  - 尺度不变性强度与脑对齐的相关性。
  - 预训练数据集大小对尺度不变性的调节作用。
  - 微调对尺度不变性的削弱作用。
- **充分性评估**：
  - 由于仅提供摘要，无法判断是否包含消融实验、控制变量实验、显著性检验、多个 fMRI 数据集交叉验证等。
  - 脑数据模态单一（仅 fMRI 自然图像响应），未包含 EEG、MEG、人类行为一致性或更广泛视觉刺激集。
  - 因此实验设计的大思路具有说服力，但公开信息不足以全面评估其统计稳健性和公平性；尤其是“尺度不变性”与“脑对齐”的关系可能受模型规模、预训练数据量等混淆变量影响，摘要中未给出因果性证明或严格的混淆控制说明。

## 6. 论文的主要结论与发现

- **核心发现**：在 60 个预训练视觉模型中，嵌入的尺度不变性越强，该模型表征与 fMRI 响应的对齐越好。
- **训练策略与表征性质的因果链**：
  - **更大的预训练数据集会增强嵌入的尺度不变性**；
  - **微调会降低嵌入的尺度不变性**；
  - 而尺度不变性的增强/削弱进一步正向/负向影响脑对齐程度。
- **理论意义**：尺度不变性可以作为一个将“训练策略”与“类脑表征”联系起来的**底层嵌入级属性**，比单纯关注数据集大小或任务指标更具机制性解释力。
- **应用意义**：提出“评估/增强嵌入尺度不变性”可作为构造更可解释、更类人的 AI 表征的一种指导原则。

## 7. 优点

- **新颖的研究视角**：从模型级外部变量转向嵌入级内在属性，为“为什么类脑表征会这样”提供了更底层的解释。
- **跨尺度的生物学动机**：借鉴生物神经系统中普遍存在的尺度不变性，具有神经科学理论支撑。
- **大规模模型分析**：基于 60 个预训练视觉模型，样本覆盖面比常见的 2~5 个模型更广，结论更有说服力。
- **连接抽象性质与实际训练变量**：将“预训练数据多少”和“是否微调”这两个已广泛认知的训练因素，传导到嵌入的尺度不变性，再到脑对齐，形成一个可操作、可干预的链路。
- **对 AI 设计有实用启发**：不需要重新训练新颖架构，只需评估并调整嵌入性质，就能提升模型与脑数据的兼容性，可能成为未来类脑 AI 评测/训练中的一个可优化指标。

## 8. 不足与局限

- **公开信息有限**：当前文本仅包含摘要，缺乏对尺度不变性的严格数学定义、脑对齐方法的具体细节（如编码模型还是 RSA）、回归/相关分析的控制变量等，导致难以复现和评估其可靠性。
- **只覆盖视觉和 fMRI**：未验证该性质是否适用于语言模型、语音模型或其他脑成像模态（EEG/ECoG/MEG），结论的通用性受限。
- **相关性不等于因果性**：发现尺度不变性与脑对齐相关可能是一种均伴现象，替代变量（如表征的层级、模型复杂度、预训练数据多样性）可能同时驱动二者；需要更多因果/干预实验。
- **微调削弱尺度不变性并降低对齐并不一定代表坏事**：微调常能提升具体任务性能，但会损失通用表征的性质；这可能意味着尺度不变性与下游任务性能存在权衡，文中若未讨论则不够全面。
- **实验场景单一**：脑数据仅来自自然图像响应，没有纳入人体行为判断、对抗攻击、类别层次结构等可进一步验证尺度不变性真实功能意义的设置。
- **未提供资源计算细节**：不利于评估该方法在大规模应用时的实际可行性，但这对于此类分析性论文通常不是核心问题。

（完）
