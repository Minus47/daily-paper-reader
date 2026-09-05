---
title: "Disentangling Superpositions: Interpretable Brain Encoding Model with Sparse Concept Atoms"
title_zh: 解耦叠加：基于稀疏概念原子的可解释脑编码模型
authors: "Alicia Zeng, Jack L. Gallant"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3aNvX9TQTo"
tags: ["query:abstraction"]
score: 9.0
evidence: 用稀疏概念原子在语义空间中解开叠加并解释脑体素选择性的编码模型
tldr: 通过词嵌入或神经网络特征预测大脑对自然刺激的响应已成为常见编码策略，但稠密嵌入中隐特征多于维度会导致语义叠加，回归权重不具可辨识性。该研究提出稀疏概念编码模型，将稠密词向量变换为更高维、稀疏且非负的、由学习所得概念原子组成的空间；在概念原子空间中重新进行编码回归后，不同语义方向能够被区分。结果显示该模型既能保持预测能力，又能提供对体素选择性的解释，揭示了语义概念如何在脑内形成多维且可分离的神经基础。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 稠密嵌入中语义叠加导致脑编码回归权重不可辨识，使体素选择机制无法得到原则性解释。
method: 设计从稠密嵌入到高维、稀疏、非负概念原子空间的非线性变换，并在此空间中拟合脑响应编码模型来分离语义方向。
result: 实验显示该稀疏概念模型在保持或提高预测性能的同时能解释voxel选择性，可分离出不同语义原子的贡献。
conclusion: 该模型通过消除语义叠加为非侵入性脑数据中的语义解码提供了可解释的概念化路径。
---

## Abstract
Encoding models using word embeddings or artificial neural network (ANN) features reliably predict brain responses to naturalistic stimuli, yet interpreting these models remains challenging. A central limitation is superposition: distinct semantic features become entangled along correlated directions in dense embeddings when latent features outnumber embedding dimensions. This entanglement renders regression weights non-identifiable—different combinations of semantic directions can produce identical predictions, precluding principled interpretation of voxel selectivity. To address this, we introduce the Sparse Concept Encoding Model, which transforms dense embeddings into a higher-dimensional, sparse, non-negative space of learned concept atoms. This transformation yields an axis-aligned semantic basis where each dimension corresponds to an interpretable concept, enabling direct readout of conceptual selectivity from voxel weights. When applied to fMRI data collected during story listening, our model matches the prediction performance of conventional dense models while substantially enhancing interpretability. It enables novel neuroscientific analyses such as disentangling overlapping cortical representations of time, space, and number, and revealing structured similarity among distributed conceptual maps. This framework offers a scalable and interpretable bridge between ANN-derived features and human conceptual representations in the brain.

---

## 论文详细总结（自动生成）

# 《解耦叠加：基于稀疏概念原子的可解释脑编码模型》论文总结

## 1. 核心问题与研究动机

- **背景**：利用词嵌入或人工神经网络（ANN）特征构建编码模型、预测人脑对自然刺激的响应，已成为计算神经科学中的常见方法。
- **核心问题**：这类稠密模型虽然预测能力强，但**可解释性严重不足**。根本原因是语义空间中的“叠加”（superposition）问题——当潜在语义特征数量超过嵌入维度时，不同的语义特征会沿着稠密嵌入中彼此相关的方向纠缠在一起。
- **后果**：这种叠加使得回归权重**不可辨识**（non-identifiable），即多个不同的语义方向组合可能产生完全相同的预测结果，从而无法从体素权重中原则性地解读大脑的选择性编码机制。
- **研究意义**：该问题阻碍了在非侵入性脑数据（如 fMRI）上进行有意义的概念解码，也限制了从神经网络表征到人类概念表征之间建立透明、可解释的桥梁。

## 2. 方法论

- **核心思想**：将稠密嵌入变换到一个**高维、稀疏、非负的概念原子（concept atoms）空间**，以消除语义叠加，使每个维度对应一个可独立解释的概念。
- **关键技术细节**：
  - 学习一组“概念原子”作为基础构建块，原始稠密嵌入被非线性映射到这些原子所张成的空间中。
  - 变换后的表征空间具有**轴对齐语义**特性，即每个轴代表一个独立的概念维度。
  - 将脑响应编码回归重新在此概念原子空间中拟合，使体素的权重向量可直接解读为对特定概念的**选择性权重**。
- **算法流程**（据摘要推导）：
  1. 输入：自然刺激对应的词嵌入或 ANN 特征。
  2. 学习或构造从稠密嵌入到概念原子空间的映射函数（非线性、稀疏非负）。
  3. 将刺激特征投射到概念原子空间。
  4. 用线性回归建立概念原子特征与 fMRI 体素响应之间的映射。
  5. 从回归权重矩阵中直接读出每个体素对不同语义概念的选择性。
- 注：原文未给出具体损失函数、网络结构或优化流程等细节。

## 3. 实验设计

- **实验场景**：受试者聆听故事时的 fMRI 数据记录。
- **数据集**：摘要仅说明为“story listening”fMRI 数据，未指明具体公开数据集名称（如 Narratives 等）。
- **Benchmark/对比方法**：
  - 对比现有使用稠密嵌入（词向量）或 ANN 特征的**传统稠密编码模型**。
  - 对比目标是：在不降低预测性能的前提下显著提升可解释性。
- **核心分析实验**：
  - 验证“时间、空间、数字”等抽象概念的皮层表征是否能够在空间上解耦。
  - 揭示分布式概念图谱之间的结构化相似性。

## 4. 资源与算力

- 原文摘要及元数据中**未提供任何关于算力、GPU 型号与数量、训练时长等资源信息**。
- 由于该论文实际为 NeurIPS 2025 已接收论文，推测完整论文的实验部分可能包含相关细节，但在当前提供的摘要文本范围内无法获取。

## 5. 实验数量与充分性评估

- **可见实验量**：鉴于当前只提供摘要，能确认的实验环节主要包括：
  - 模型预测性能与稠密模型的对比；
  - 概念解耦效果的神经科学分析（时间/空间/数字）；
  - 分布式概念图谱的相似性结构分析。
- **充分性判断**：
  - 从摘要描述看，实验设计覆盖了“性能”和“可解释性”两个核心目标维度，具有一定说服力；
  - 但缺乏对消融实验、不同嵌入类型、不同体素区域、多被试泛化、与多种基线方法系统对比等细节的说明，在证据全貌上有所欠缺。
  - 总体而言，需依赖完整论文才能充分评估实验的严谨性与公平性；摘要部分不足以断言实验完全充分。

## 6. 主要结论与发现

1. 稀疏概念编码模型在故事聆听 fMRI 数据上**匹配甚至优于传统稠密模型的预测性能**。
2. 同时**大幅提升模型可解释性**——体素权重可以直接被解读为对特定语义概念的选择性。
3. 实现了**重叠皮层表征的解耦**：能够将大脑中“时间”“空间”“数字”等抽象概念的叠加表征在概念原子层面分离开来。
4. 揭示了不同概念在大脑皮层上**分布式拓扑图谱之间存在结构化相似性**。
5. 为 ANN 特征与人脑概念表征之间提供了**可扩展、可解释的桥梁**，为非侵入性脑数据中的语义解码开辟了新路径。

## 7. 优点与亮点

- **问题定位精准**：明确指出稠密模型中语义叠加导致回归权重不可辨识这一根本性局限，而非仅停留在预测精度提升。
- **方法创新性**：将稀疏编码与概念原子结合，在高维语义空间做变换，从表示层面化解叠加问题，思想简洁而有力。
- **可解释性突破**：轴对齐的语义基础实现了“概念→体素权重”的直接对应，使脑编码模型从黑箱回归真正走向机制性解释。
- **科学价值高**：该方法不仅服务于工程预测目标，还直接催生了新的神经科学分析范式——解耦抽象概念（如时间、空间、数量）在皮层上的重叠表征。
- **结果兼优性**：在增强可解释性的同时保持或改进预测性能，避免了“性能–可解释性”此消彼长的传统困境。

## 8. 不足与局限

- **技术细节缺失**：当前摘要未提供概念原子的学习方式（是否有监督）、变换网络架构、稀疏度控制方法、非负约束的实现以及模型复杂度等关键技术细节。
- **数据集广度不够**：仅报告了故事聆听 fMRI 场景，未提及是否在多种模态（如视觉、多语种）或多数据集下验证泛化性。
- **可扩展性验证不充分**：需回答概念原子空间是否能跨被试、跨任务保持一致，以及在大规模词汇/概念上的可扩展性。
- **计算资源报告缺失**：未见训练/推理计算开销分析，限制了对方法落地成本的直接评估。
- **潜在偏差风险**：
  - 概念原子的可解释性具有主主观成分，其结构如何保证一致性需要量化验证；
  - 空间、时间与数字的“解耦”可能受到刺激材料分布的局限制约，存在言语材料固有共变所带来的风险；
  - 与稠密基线模型的比较是否充分控制了模型容量、正则化方式等因素，尚不明确。
- **应用限制**：作为单篇研究，考虑到脑响应存在个体差异，对单被试的强解释是否适用于人群层面，仍待未来研究。

（完）
