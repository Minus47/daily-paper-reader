---
title: "Polaris: Coupled Orbital Polar Embeddings for Hierarchical Concept Learning"
title_zh: Polaris：面向层级概念学习的耦合轨道极坐标嵌入
authors: "Sahil Mishra, Srinitish Srinivasan, Sourish Dasgupta, Tanmoy Chakraborty"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0c5912a38927dcec6772a063e725bb6a5c88f45b.pdf"
tags: ["query:abstraction"]
score: 8.0
evidence: 极超球面嵌入利用角度与半径分离语义和层级，直接支撑概念树与本体构建
tldr: 针对产品分类树、医学本体等层级知识难以学习非对称结构与噪声语义的问题，提出Polaris极坐标超球面嵌入框架，用角度表示语义、半径表示层级，并通过切空间投影、指数映射与球面线性层学习单位范数表示。结合稳健局部约束与全局正则化抑制几何坍缩。实验表明该方法能够在相互干扰最小的条件下学习概念的意义与层级。该框架可直接服务于大规模类脑概念本体与概念分类体系的构建。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 真实知识常以产品分类树、医学本体等层级形式组织，现有方法难以分离语义与层级结构。
method: 采用极超球面嵌入，将语义置于角度、层级置于半径，并用指数映射和球面线性层学习单位范数表示。
result: 在层级概念表示学习中能有效避免几何坍缩，并提高对非对称层级结构的抗噪能力。
conclusion: 为大规模概念分类体系和本体构建提供了可复用且结构可分解的嵌入方法。
---

## Abstract
Real-world knowledge is often organized as hierarchies such as product taxonomies, medical ontologies, and label trees, yet learning hierarchical representations is challenging due to asymmetric structure and noisy semantics. We introduce Polaris, a polar hyperspherical embedding framework that separates semanticity from hierarchy using angular geometry and radius, enabling the learning of meaning and structure without interference. To map latent representation onto the sphere, we project it to the tangent space at the north pole, apply the exponential map, and learn unit-norm representations using spherical linear layers. Polaris then combines robust local constraints, global regularization that prevents geometric collapse, and uncertainty-aware asymmetric objectives that encourage directional containment. At inference time, Polaris uses structure-guided retrieval to efficiently narrow down candidate parents before final ranking. We evaluate Polaris on different settings of taxonomy expansion -- spanning trees, multi-parent DAGs, and multimodal hierarchies, showing consistent improvements of up to $\sim$19 points in top-$K$ retrieval and up to $\sim$ 60\% reduction in mean rank over fourteen strong baselines.

---

## 论文详细总结（自动生成）

# Polaris：面向层级概念学习的耦合轨道极坐标嵌入——论文总结

## 1. 核心问题与研究动机

- 真实世界的知识（如**产品分类树、医学本体、标签体系**）大多以**层级结构**组织，现有语义嵌入方法难以同时建模这种结构。
- 层级结构本身具有**不对称性**（如"狗"包含于"动物"，反向不成立），语义又常存在噪声，造成学习困难。
- 大多数已有方法无法将**"语义含义"**与**"层级归属"**这两个信息维度有效解耦，导致二者在学习中相互干扰。
- 论文据此提出 Polaris，目标是在同一嵌入空间中**分离语义表征和层级结构**，实现对概念意义与概念层级关系的联合学习。

## 2. 方法论：核心思想与技术细节

**核心思想：耦合轨道极坐标嵌入（Polar Hyperspherical Embedding）**

- 将语义编码在**角度方向**上，将层级深度编码在**半径（极径）**上，实现语义性和层级性的显式分离。
- 这样，语义相近的概念在球面方向上彼此靠近；层级中越具体的概念具有越小的半径，从而自然地嵌入于父概念所张成的方向区域之内。

**关键技术细节：**

- **切空间投影**：将潜在表示先投影到北极点的切空间；
- **指数映射（Exponential Map）**：利用指数映射将切空间表示映射回超球面，获得单位范数向量；
- **球面线性层（Spherical Linear Layers）**：使网络各层保持单位范数约束，强化几何一致性；
- **鲁棒局部约束**：建立局部父子约束（local constraints），用于传递层级包含关系；
- **全局正则化**：设计防止几何坍缩（geometric collapse）的全局正则项，使不同层级和不同语义方向不被压缩到同一区域；
- **不确定性感知的不对称目标**：通过不确定性加权的不对称损失函数，鼓励子概念**方向性地包含于**父概念的极坐标锥形区域内，从而建模层级不对称关系；
- **结构引导检索（Structure-Guided Retrieval）**：推理时先借助学习到的结构约束快速缩小候选父节点集，再做精细排序，降低计算复杂度并提升检索效率。

## 3. 实验设计

- **研究对象（任务）**：分类体系扩展（Taxonomy Expansion），即给定已有概念树或图结构，为新概念寻找正确父节点的表示学习任务。
- **包含三类层级场景（benchmark 设定）**：
  1. **生成树（Spanning Trees）**：每个节点单一父节点的树状分类体系；
  2. **多父 DAG（Multi-parent DAGs）**：节点允许多个父概念的有向无环图结构；
  3. **多模态层级（Multimodal Hierarchies）**：融合文本、图像等多模态信息的概念层级。

- **对比基线**：涵盖 **14 个强基线模型**，包括已有的知识图谱嵌入方法、双曲嵌入和超球面/极坐标嵌入方法等。
- **评估指标**：top-K 检索准确率（Hit@K）与平均秩（Mean Rank）。

## 4. 资源与算力

- 论文中**未明确说明**使用的 GPU 型号、GPU 数量、训练轮数或总训练时长等具体计算资源信息。
- 由于本篇为基于原文元数据与摘要的总结，模型参数量级、训练开销等细节也未见提及，无法做出推断。

## 5. 实验数量与充分性

- 从已提供内容来看，实验覆盖了**三种不同设置的结构场景**（树、DAG、多模态层级），并对 **14 个强基线**进行统一对比。
- 指标采用 top-K 检索和平均秩两方面，兼顾了准确率与排序质量，比较全面。
- **但未列出具体的消融实验、参数敏感性分析和组件贡献量化实验**，因此从可获取的内容来看，实验结果提供了一定证据，但若需进一步验证每个设计组件的必要性，仍需补充更多细粒度实验（原论文实验章节应有更多细节，但此处未提供）。

## 6. 主要结论与发现

- 在层级概念表示学习中，语义和层级可以在**极坐标下解耦**而不是混在一起，这种分解明显降低了两者间的相互干扰。
- Polaris 在三种层级扩展场景中一致性地优于全部 14 个基线模型：
  - top-K 检索得分提升最多约 **19 个百分点**；
  - 平均秩最多降低约 **60%**。
- 该框架具有较好的可扩展性和通用性，有望支撑大规模概念本体与概念分类体系的构建。

## 7. 方法亮点与优点

- **几何设计新颖且直观**：采用"角度表语义 + 半径表层级"，几何意义明确，具有较强的可解释性。
- **数学工具选择恰当**：切空间投影 + 指数映射 + 球面线性层能保证输出稳定落在超球面上，从而维持层级关系的几何一致性。
- **正则化设计具有针对性**：全局正则化显式防止几何坍缩，是针对超球面嵌入常见病态（坍缩）的有效防御。
- **不确定性感知的不对称损失**：能处理层级中的不对称关系与语义噪声，更贴近真实应用场景。
- **推理阶段效率优化**：利用结构引导检索缩小候选空间，在前 k 指标提升的同时降低复杂度。
- **泛化场景广**：覆盖树、DAG 和多模态等不同结构，从建模视角看具有较好的任务覆盖能力。

## 8. 不足与局限

- 文本未提供**消融实验**和**各组件的单独贡献分析**，从而难以严格评判每个模块的相对重要性。
- 对**多父 DAG 和多模态场景**下如何在引入额外模态数据的同时保持语义-层级解耦，摘要中没有给出更深层的机制说明。
- 未见**失败案例或边界情况的分析**，比如高噪声语义、极大层级深度、极端不均衡分布下的表现未知。
- 计算资源、可扩展性分析（如时间、内存随概念规模的增长曲线）没有在提取信息中体现，对大规模实际部署参考有限。
- 当前文本仅确认了检索类任务的性能，缺少**对下游任务（如分类、知识推理、本体对齐）**的实证检验。
- 原文仅基于摘要进行分析，**无法评估其与真值标签的标注方式、数据分布、实验随机性控制等细节是否足够严谨**，因而对其实验公平性只能部分判断。

---

（完）
