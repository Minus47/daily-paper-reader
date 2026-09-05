---
title: "BrainJanus: A Unified Model for Understanding and Generation across Brain, Vision, and Language"
title_zh: BrainJanus：跨脑、视觉与语言的统一理解与生成模型
authors: "Haitao Wu, Qirui Zhang, Zhouheng Yao, Shangquan Sun, Qihao Zheng, Mianxin Liu, Chi Zhang, Wanli Ouyang, Chunfeng Song, Changqing Zhang, Jiamin Wu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d0a7245e4c5c2c2d5f4558a2ce9a2ef184f6bdf5.pdf"
tags: ["query:abstraction"]
score: 8.0
evidence: 将大脑、视觉和语言统一到单一类脑框架，属于类脑AI模型与认知架构
tldr: 当前脑感知与生成研究把脑编码和解码割裂，缺少体现大脑多模态整合性质的统一模型。BrainJanus提出统一脑分词器，将连续神经动态量化为离散token，并在共享Omni空间中对齐视觉和语言表示，由此在同一模型中支持脑-视觉-语言的理解与生成。该模型为多模态类脑AI架构提供了新范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 脑编码与解码被割裂处理且依赖单模态先验，与大脑多模态整合机制不符。
method: 提出统一脑分词器与共享Omni空间，将脑信号离散化并与视觉或语言表征对齐。
result: 实现脑、视觉、语言间的双向理解与生成，统一了脑的多模态建模。
conclusion: 为多模态脑机接口和类脑AI提供了统一模型基础，支持后续跨模态检索应用。
---

## Abstract
Modeling the bidirectional correspondence between external sensory stimuli and internal neural activity has emerged as a critical frontier in neuroscience.
However, existing approaches predominantly treat brain encoding and decoding as isolated tasks, relying heavily on unimodal alignment and external priors while overlooking the brain's intrinsic nature as a multimodal integration system. To address these limitations, we propose BrainJanus, the first unified brain model that integrates brain, vision, and language within a single framework. Specifically, we introduce a Unified Brain Tokenizer to quantize continuous neural dynamics into discrete tokens aligned with visual and linguistic representations in a shared Omni space. Building on this, we utilize an All-in-One autoregressive architecture that leverages next-token prediction to enable seamless any-to-any generation, which encompasses image-to-brain and text-to-brain encoding, and brain-to-image and brain-to-text decoding. Extensive experiments demonstrate that BrainJanus achieves superior performance across diverse benchmarks. Furthermore, our framework exhibits zero-shot generalization and preserves interpretable biological topography, highlighting its potential as a general-purpose brain modeling paradigm. The code is available at \href{https://github.com/HaitaoWuTJU/BrainJanus}{GitHub}.

---

## 论文详细总结（自动生成）

## 论文详细总结

### 1. 核心问题与整体含义
- **研究动机**：建模外部感官刺激与内部神经活动之间的双向对应关系，是神经科学的前沿方向。然而，现有研究将脑编码（stimulus → brain）与脑解码（brain → stimulus）视为相互割裂的任务，严重依赖单模态对齐与外部先验（如预训练的视觉/语言特征），忽略了大脑本质上是多模态整合系统这一关键特性。割裂式建模无法体现大脑的综合性质，也限制了模型在真实神经场景下的通用性与泛化能力。
- **整体含义**：论文提出一个统一的类脑智能框架，将大脑、视觉、语言纳入同一模型，以模拟大脑的多模态整合性质，并支持任意模态之间的相互理解与生成。

### 2. 方法论
- **核心思想**：将脑信号当作一种“语言”来建模——通过统一的离散 token 化方案，把脑活动纳入与视觉、语言共享的表示空间中，从而把脑-视觉-语言之间的映射统一为序列间的“下一 token 预测”问题。
- **关键技术细节**：
  - **Unified Brain Tokenizer（统一脑分词器）**：将连续的神经动态信号量化为离散 token。通过量化操作，使脑信号表示与视觉、语言表示对齐到一个共享的 Omni 空间。
  - **共享 Omni 空间**：将脑、图像、文本三类模态的表征投射到同一特征空间中进行联合对齐，避免了以往依赖单模态特征的外部先验。
  - **All-in-One 自回归架构**：在统一 token 空间中使用一个自回归模型执行 next-token prediction，从而支持任意输入模态到任意输出模态的生成，覆盖四条通路：
    - 图像→脑（脑编码）
    - 文本→脑（脑编码）
    - 脑→图像（脑解码）
    - 脑→文本（脑解码）
  - **流程说明**：输入数据（脑信号/图像/文本）经各自编码器处理 → 经统一 tokenizer 映射为离散 token → 自回归模型进行统一的 next-token 学习 → 按目标模态输出生成结果。整个流程端到端联合训练，无需将脑编码和解码拆分为独立子任务。

### 3. 实验设计
- **数据集 / 场景与 benchmark**：论文所给摘要中没有逐一列出所使用的具体神经影像数据集及对应 benchmark 名称（如常见的 fMRI 数据集 NSD、BOLD5000 等尚未在提取文本中出现）；但从任务设置来看，实验覆盖了**脑编码（图像到脑刺激模式预测）与脑解码（脑活动重建图像 / 生成文本）两大类下游任务**。
- **对比方法**：摘要未列出逐一方法名，但从问题取向推断，对比对象大致包括传统独立脑编码模型与独立脑解码模型，以及依赖单模态预训练对齐的方法。
- **评估指标**：主要依据各类基准任务上的性能进行评判，并额外报告了零样本泛化能力的表现。

### 4. 资源与算力
- 论文提取文本（摘要部分）**未报告任何算力信息**，包括 GPU 型号、数量、训练时长、参数规模等细节均未提及。若要复现，还需要查阅论文全文的附录或实验环境部分。

### 5. 实验数量与充分性
- **实验规模**：根据摘要，论文声称在多个多样化 benchmark 上均取得了领先性能，并验证了零样本泛化能力与脑图拓扑可解释性。但由于提取内容有限，无法确知具体做了多少个数据集上的主实验、消融实验组数以及逐项统计分析。
- **充分性评估**：从摘要结论看，实验设计在覆盖面（同时涵盖脑编码、脑解码、零样本迁移、生物可解释性）上有一定广度。但要严格判断其公平性与统计显著性（如与 SOTA 方法差异的显著性检验、跨被试泛化等），仍需依靠全文补充信息才能充分评估。

### 6. 主要结论与发现
- BrainJanus 作为首个将脑、视觉、语言三模态统一在同构自回归框架中的模型，实现了任意模态间的双向理解和生成。
- 在多个基准上达到最佳效果，优于将脑编码与脑解码割裂处理的方式。
- 支持零样本泛化，意味着模型在未见过的条件/任务下亦具有合理的映射能力。
- 能够保持脑图中有意义的神经拓扑结构，说明其学习到的脑特征具有生物学上的可解释性，而非纯粹统计拟合。

### 7. 优点
- **架构创新**：首次将脑信号编码、脑刺激重建、跨模态文本生成纳入一个统一的脑分词器 + 共享 Omni 空间 + 自回归循环框架中，范式上有较高的原创性。
- **体现脑的多模态整合本质**：让“理解”与“生成”共享一套表示，避免了以往单模态预训练先验带来的偏置，理论上更适合模拟真实大脑的工作机制。
- **任务泛化能力强**：统一的 next-token 建模天然支持图像↔脑、文本↔脑的任意方向映射，甚至可扩展到未来的多模态组合输入。
- **具备可解释性**：脑拓扑结构的保留证明了模型学到的是有神经学意义的结构特征，而不仅是抽象的黑盒映射，这有助于后续神经科学分析。
- **统一框架具有扩展潜力**，可直接衍生到跨模态检索、脑机接口内容生成等应用方向；且作者已公开代码，便于后续研究者复现与改进。

### 8. 不足与局限
- **算力与训练开销细节缺失**：摘要未给 GPU 数量、训练时间、模型参数等关键资源的说明，使复现成本和可重复性评价受阻。
- **具体实验细节不足**：提取内容中缺少对参与对比的基线方法、具体神经影像数据集、评价指标细节与统计显著性分析的明确表述，难以全面验证其相较现有方法的公平性和优势幅度。
- **数据层面潜在局限**：神经影像数据通常集个体差异大、采集成本高，训练数据是否跨被试、跨扫描仪扩展，以及跨模态 token 化是否会丢失细粒度时空信息（如高时间分辨率信号）等潜在问题，文中摘要未涉及。
- **零样本泛化的边界未说明**：摘要中仅笼统提出具备零样本能力，但具体零样本条件如何定义（新刺激类型？新被试？新视觉类别？）及其失败边界不明确。
- **实际应用限制**：脑信号解码依赖高性能采集设备，面向实际脑机接口落地（低信噪比、实时处理约束）仍可能有较大差距。

（完）
