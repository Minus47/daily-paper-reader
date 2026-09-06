---
title: Cross-Subject Integration of Multi-Region Neural Signals via Functional Embedding.
title_zh: 基于功能嵌入的多脑区神经信号跨被试整合
authors: "Sina Javadzadeh, Rahil Soroushmojdehi, S. Alireza Seyyed Mousavi, Mehrnaz Asadi, Sumiko Abe, Terence Sanger"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=VWuScx0iqH"
tags: ["query:eeg-align"]
score: 8.0
evidence: 用孪生编码器和对比目标为电极学习被试无关功能嵌入，直接服务神经活动表征学习
tldr: 跨被试聚合颅内记录时，电极数量和位置差异使基于解剖坐标的标准化难以反映功能相似性。该文设计可扩展的表示学习框架，利用孪生编码器与对比目标为每个电极学习不依赖被试的功能身份，并保证嵌入在脑区位置上具有局部敏感性。这为跨被试的多脑区神经动态整合提供了统一表示。该方法可迁移到脑电通道对齐等跨被试表示学习问题。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 颅内脑区电极位置和覆盖因人而异，按解剖坐标归一化不能反映真实功能相似性。
method: 使用孪生编码器和对比目标学习电极级别的被试无关功能嵌入，保留脑区位置局部敏感结构。
result: 学到的功能嵌入几何可有效整合多脑区神经信号，实现跨被试统一表示。
conclusion: 以功能身份代替解剖坐标能够提升神经信号跨被试建模的一致性和可迁移性。
---

## Abstract
Aggregating intracranial recordings across subjects is challenging since electrode count, placement, and covered regions vary widely. Spatial normalization methods like MNI coordinates offer a shared anatomical reference, but often fail to capture true functional similarity, particularly when localization is imprecise; even at matched anatomical coordinates, the targeted brain region and underlying neural dynamics  can differ substantially between individuals. We propose a scalable representation-learning framework that (i) learns a subject-agnostic functional identity for each electrode from multi-region local field potentials using a Siamese encoder with contrastive objectives, inducing an embedding geometry that is locality-sensitive to region-specific neural signatures, and (ii) tokenizes these embeddings for a transformer that models inter-regional relationships with a variable number of channels. We evaluate on a 20-subject dataset spanning basal ganglia–thalamic regions collected during flexible rest/movement periods with heterogeneous electrode layouts. The learned functional space supports accurate within-subject discrimination and forms clear, region-consistent clusters; it transfers zero-shot to unseen channels. The transformer, operating on functional tokens without subject-specific heads or supervision, captures cross-region dependencies and enables reconstruction of queried channels, providing a subject-agnostic backbone for downstream decoding. Together, these results indicate a path toward large-scale, cross-subject aggregation and pretraining for intracranial neural data where strict task structure and uniform sensor placement are unavailable.

---

## 论文详细总结（自动生成）

# 论文总结：基于功能嵌入的多脑区神经信号跨被试整合

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **核心问题**：跨被试聚合颅内（intracranial）记录非常困难。不同被试的电极数量、摆放位置和覆盖脑区差异很大，导致传统以解剖坐标（如 MNI 坐标）进行空间归一化的方法无法反映真实的功能相似性。
- **关键痛点**：即便解剖坐标对齐，不同被试对应位置覆盖的脑区或底层神经动态也可能存在明显差异；加之电极定位不精确，单纯空间对齐很难将“同一功能脑区”的记录正确关联起来。
- **整体含义**：作者认为需要将电极表示从“解剖空间”切换到“功能空间”，学习一种**被试无关（subject-agnostic）的功能身份（functional identity）**，从而为大规模跨被试神经数据的统一建模、预训练和下游解码建立通用基础。

---

## 2. 论文提出的方法论

- **核心思想**：让每个电极获得一个不依赖具体被试的功能嵌入（embedding），用对比学习构造嵌入几何，使嵌入在位置上对“特定脑区的神经模式”具有局部敏感性（locality-sensitive），再把嵌入token化为Transformer输入，以建模可变通道数量下的跨脑区关系。
- **技术细节**：
  - **Siamese 编码器 + 对比目标**：使用孪生编码器（Siamese encoder）处理多脑区局部场电位（LFP）信号，通过一致性/判别性对比目标进行训练。摘要未给出具体对比损失公式与正负样本构造规则。
  - **功能嵌入几何**：学习出的空间能够容纳被试内判别（within-subject discrimination），并形成清晰且与脑区一致的聚类结构；对未见过的新通道具有零样本迁移能力。
  - **Transformer tokenization**：将电极嵌入作为token输入Transformer，以处理被试间不同的通道数量。
  - **无被试专用结构**：该Transformer不含subject-specific头或监督标签，可捕捉跨区域依赖，训练后可对查询通道进行重建。
- **算法流程（文字版）**：
  1. 采集来自不同被试、不同脑区的LFP信号片段；
  2. 对每个电极（或每个通道片段）学习一个功能性身份表示；
  3. 通过Siamese编码器拉近同类功能区/通道间的表示，推开不相似通道的表示；
  4. 将嵌入序列输入Transformer，学习跨脑区依赖；
  5. 在下游任务或重建任务中验证这种功能token的有效性。

> 需要注意的是，原文Meta data及Abstract给出的是“可扩展表示学习框架”的整体设计，并未显式列出网络结构细节、损失函数数学形式及超参数配置。核心信息为“Siamese编码器＋对比学习＋Transformer tokenization”。

---

## 3. 实验设计

- **数据来源**：文中提到的一项**20被试数据集**，覆盖基底节–丘脑区域（basal ganglia–thalamic regions），采集过程为**灵活休息/运动时段**（flexible rest/movement periods），且被试间的电极布局高度异质。
- **Benchmark 名**：文内没有为此数据集给出明确的benchmark名称；它本身就是一个跨被试多脑区颅内LFP数据集，没有提到类似“公共数据集对比”。
- **评估场景**：
  1. 被试内判别是否准确；
  2. 学到的嵌入空间是否能形成清晰、且与脑区一致的聚类结构；
  3. 是否可以对未见过的通道零样本迁移；
  4. Transformer在无subject特定头、无监督的条件下，是否可捕获跨脑区依赖并完成对查询通道的重建。
- **对比方法**：本材料中没有提到与其他基线/前人方法的定量对比列表。也就是说，在提供的文本范围内**没有明确给出对比基线**。

---

## 4. 资源与算力

- 论文文本中**未提及**GPU型号、集群、训练时长、参数量或计算量等算力资源信息。
- 只能推断方法属于可扩展表示学习框架，需要较大的深度神经模型训练开销，但无法从现有材料取得具体的工程资源数据。

---

## 5. 实验数量与充分性

- **实验数量与类型**：从摘要看主要是单一数据集上的五类检验：
  - 被试内判别；
  - 跨被试聚类分析；
  - 零样本通道迁移；
  - 跨脑区依赖建模；
  - 通道重建；
- **充分性**：实验覆盖了功能嵌入的基本性质（判别性、聚类性、泛化性）以及后端Transformer的上游能力。但整体上**不够全面**：
  - 仅在一个数据集上进行，且该数据集仅覆盖基底节–丘脑区域，缺乏跨脑区类型、跨数据采集协议的外部验证；
  - 未在摘要中体现对噪声、电极定位误差的鲁棒性分析；
  - 没有消融实验表明去掉对比学习、替换为解剖坐标或随机坐标等方法的效果差异；
  - 作为“跨被试聚合”的论文，最好进一步做开放词汇的下游神经解码（如轨迹解码、行为分类），摘要仅暗示可行，未展开。
- **客观性与公平性**：摘要没有和其他方法在同一基准上比较，无法充分判断其相对提升幅度与是否公平对比。

---

## 6. 论文的主要结论与发现

- 学到的**功能嵌入空间**能够支持准确的被试内判别，也能形成清晰且区域一致的跨被试聚类。
- 通过学习“功能身份”而不仅仅是解剖坐标，模型表现得更好、更稳健。
- 基于功能嵌入 token的 Transformer 在无需被试标识或监督头的条件下可以构建“subject-agnostic backbone”，能捕获跨脑区依赖并重构通道信号。
- 因此该方法指向一条大规模、多被试、跨脑区神经数据聚合与预训练的新路径，适用于任务结构不明确、电极布局不统一的实际神经记录场景。

---

## 7. 优点

- **动机明确且合理**：指出解剖坐标在跨被试功能对齐中的根本性缺陷，直接提出“功能嵌入”替代“空间坐标”的思路。
- **跨被试泛化性**：采用被试无关的表示，能够在不需要对新用户信息进行适配的条件下实现新通道的零样本使用。
- **适配异质电极数量**：通过“嵌入token化”把可变通道长度编码为Transformer输入，从而容忍数据布局不均匀。
- **去除subject-specific head**：学习的backbone能够被复用，适用于后续多种下游任务，具备预训练潜力。
- **方法设计简洁**：对比学习+Transformer已是当前成熟的表征学习组合，易于继续扩展成大模型范式。

---

## 8. 不足与局限

- **实验覆盖范围有限**：仅报告了一个20被试的颅内数据集，脑区限定于基底节–丘脑回路，任务也比较单一；是否可应用到皮层脑电/脑电以及多种目标行为还需进一步验证。
- **缺少可比基线的定量比较**：材料未展示与传统MNI归一化、现有EEG对齐方法或监督方法的直接对比，因而难以客观量化优势。
- **算法细节缺失**：对比学习正负样本、Siamese编码器具体结构、Transformer层数、损失函数、训练与评估协议等关键实现细节都未被详细披露，影响公平复现和评价。
- **鲁棒性未评估**：没有分析电极位置误差、噪声水平、参考方案、休息/运动任务不平衡等真实干扰因素。
- **算力与环境设定未描述**：文中完全没有提供关于计算资源、训练时间和代码可获取性的信息，不利于工程实用性判断。
- **现阶段仅证明表示在判别/聚类/重建上是可行的，距临床应用或大规模脑数据预训练仍有一段距离**：下游解码结论只是隐含而非直接验证。

（完）
