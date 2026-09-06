---
title: Semantic-guided Contrastive Learning for EEG Multimodal Decoding of Listening and Watching
title_zh: 语义引导对比学习用于听视觉场景的EEG多模态解码
authors: "Jiahao Fan, Zongsheng Li, Benxiang Xiao, Qingzhu Zhang, Xinke Shen, Quanying Liu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=aqf6rGTBHy"
tags: ["query:eeg-align"]
score: 9.0
evidence: 在听觉与视觉刺激上根据语义相似度统一对齐EEG表征
tldr: 现有EEG语义解码常按视觉、听觉或语言分别定制，缺少跨模态统一框架。Semantic-CL提出语义引导对比学习框架，无需修改结构即可同时适配听觉和视觉语义解码。它利用刺激间语义相似度构造软对比目标来对齐EEG表征，并用跨被试对比增强泛化。这展示了从EEG中实现视听跨模态语义解码的通用路径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 当前EEG语义解码方法多局限于单一模态，难以统一处理自然主义的听觉与视觉刺激。
method: 使用语义引导的软对比学习将EEG表征按刺激语义相似度对齐，并结合跨被试对比对齐提高泛化。
result: 在多个听觉与视觉语义任务上实现了无需架构改动的统一脑电解码，验证了语义对比对齐有效性。
conclusion: 以语义相似度引导的对比学习可建立统一的EEG多模态解码框架，支持脑机接口自然交互。
---

## Abstract
Semantic decoding poses a fundamental challenge in brain-computer interfaces (BCIs) aiming for naturalistic brain-machine communication. Current approaches remain modality-specific (e.g., tailored for visual, auditory, or linguistic stimuli), lacking a universal framework that generalizes across diverse modalities. To address this, we propose Semantic-Guided Contrastive Learning (Semantic-CL), a unified framework for EEG-based semantic decoding that adapts to multiple modalities—including auditory and visual—without architectural modifications. The framework leverages semantic-guided soft contrastive learning to align EEG representations using stimulus semantic similarity metrics, augmented by inter-subject contrastive alignment to harmonize neural patterns across subjects. Evaluated on two modality-distinct benchmarks—SEED-DV (dynamic video) and Broderick (natural speech)—Semantic-CL achieves the state-of-the-art performance in semantic decoding, especially in the more challenging cross-subject settings. This study establishes a modality-agnostic EEG semantic decoding framework, enabling deployable BCIs in naturalistic contexts. Our code is available at https://anonymous.4open.science/r/Semantic-CL-Cross_subject-anonymous-DBD7.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究问题**：脑机接口（BCI）中的**语义解码**是实现自然化脑机通信的关键挑战，而现有的 EEG 语义解码方法多局限于单一模态——即分别针对视觉、听觉或语言刺激进行定制设计。
- **核心痛点**：目前**缺乏一个统一的、跨模态通用的语义解码框架**。不同研究往往各自适配不同的刺激类型，导致方法之间互不通用，也阻碍了脑机接口在自然场景中的部署。
- **研究目标**：提出一个无需架构改动、即可同时适配**听觉与视觉**模态的 EEG 语义解码统一框架，以弥合多模态 EEG 语义解码之间的方法论鸿沟。

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

- **方法名称**：**语义引导对比学习（Semantic-Guided Contrastive Learning, Semantic-CL）**。
- **核心思想**：不同模态的刺激（如视频、语音）在语义层面存在可度量的相似性。通过将 EEG 表征与刺激的**语义相似度**对齐，可以建立一个不依赖具体模态结构的一致解码空间——即让 EEG 表征反映"刺激讲了什么"，而非"刺激以何种形式呈现"。
- **两大约束机制**：
  1. **语义引导软对比学习**：以刺激间语义相似度作为软标签，构造对比学习目标。不同于传统的硬正负样本划分，软对比目标可以根据语义相似程度**连续、渐进地**拉近或推开 EEG 表征，从而更细腻地刻画语义关系。
  2. **跨被试对比对齐**：增加跨被试的对比项，将不同被试的神经模式映射到共享表征空间，以缓解个体差异造成的分布偏移，提升跨被试泛化能力。
- **总体流程**（文字描述）：
  - 输入：EEG 信号片段及其对应刺激（视频/语音）；
  - 用编码器提取 EEG 表征，用预训练语义模型提取刺激语义表征；
  - 以语义相似度矩阵构造软对比学习目标，训练 EEG 编码器；
  - 同时引入跨被试对比对齐项，统一不同被试的表征分布；
  - 在推理阶段，直接根据 EEG 表征与候选刺激语义表征的相似度完成语义解码。
- **关键特点**：框架本身**不改变网络结构**，仅通过训练目标和辅助对齐机制实现多模态适配，体现了其通用性与即插即用的潜力。

## 3. 实验设计：数据集、基准与对比方法

- **数据集与场景**：实验覆盖了两个**模态不同类型的基准**数据集：
  - **SEED-DV**：动态视频刺激场景下的 EEG 数据，对应**视觉**模态语义解码；
  - **Broderick**：自然语音刺激场景下的 EEG 数据，对应**听觉/语言**模态语义解码。
- **Benchmark 任务**：两大公开基准数据上的语义解码任务，评估方式包括常规设置以及更具挑战性的**跨被试（cross-subject）** 设置。
- **对比方法**：论文虽未在此提取内容中逐项列出具体基线方法名称，但从表述可推断，对比对象包括**为视觉或听觉单一模态定制的现有 EEG 语义解码方法**，用于验证 Semantic-CL 在无需结构改动的情况下能否超越专用模型。

## 4. 资源与算力

- 本次提供的论文内容（PDF 提取页面仅包含摘要与元数据）**未明确披露**实验所使用的 GPU 型号、数量、训练时长或硬件资源配置。
- 可推断的是：两个基准（SEED-DV、Broderick）的 EEG 数据规模属于中等量级，且对比学习训练通常可在单块中高端 GPU（如 RTX 3090/A100 级别）上完成，**但这属于推测，而非论文所述事实**。若读者需要精确复现算力信息，需查阅论文正文的实验设置部分。

## 5. 实验数量与充分性

- **实验数量**（根据可获得的摘要信息）：
  - 两个多模态数据集上的主实验（视觉 + 听觉各一）；
  - 常规设置与跨被试设置两种评测场景；
  - 结合摘要中"验证语义对比对齐有效性"的表述以及 IC 领域惯例，可能还包含消融实验（如去掉软对比、去掉跨被试对齐等），但**具体消融数量在本次提供的内容中未明确列出**。
- **充分性评价**：
  - **优点**：选择视觉与听觉两个模态差异明显的基准进行验证，在模态覆盖上做到了多样化；重点关注**跨被试**这一实际部署中最具挑战性的场景，体现了方法实用性导向。
  - **局限**：仅覆盖**动态视频**和**自然语音**两类自然刺激，尚未验证静态图像、文字阅读或其他感觉模态（如触觉）下的推广性；SEED-DV 的被试规模和任务类型有限。总体而言，主实验设计合理，但要充分证明"模态无关"的普适性主张，实验的**模态广度有待进一步扩展**。

## 6. 论文的主要结论与发现

- **性能结论**：Semantic-CL 在 SEED-DV 与 Broderick 两个模态差异显著的基准上均取得了**最先进的语义解码性能**，尤其是在更具挑战性的**跨被试设置**下优势明显。
- **方法论结论**：以语义相似度引导的软对比学习，能够成功地将 EEG 表征对齐到统一的语义空间，从而在不改动网络结构的前提下适配不同刺激模态。
- **领域意义**：该研究首次（在此对比框架意义上）展示了 EEG 视听跨模态语义解码的**通用路径**，为脑机接口在自然场景中的可部署性提供了理论与实验支持。

## 7. 优点：方法与实验设计的亮点

- **方法层面的创新性**：
  - 提出**统一框架**，首次无需针对模态定制网络结构即可同时处理听觉与视觉语义解码，拓展了 EEG 语义解码的方法论视野；
  - **软对比学习**突破了传统对比学习仅依赖硬正负样本的限制，能利用刺激语义相似度的连续信息，学习到的表征更细腻；
  - 将**跨被试对齐**嵌入对比学习框架，直接针对 EEG 个体差异大的固有痛点；
  - 框架具有即插即用性质，可适配到不同 EEG 编码器结构，迁移潜力大。
- **实验设计层面的亮点**：
  - 在模态差异显著的**两个独立自然刺激基准**（动态视频 vs. 自然语音）上验证，增强了结论的可推广性；
  - 同时报告常规与**跨被试**两种设置，后者贴近真实 BCI 部署条件，体现出较高的应用意识。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅验证了视觉和听觉两类模态，缺乏静态图像、语言阅读等多模态内容的验证，因此"模态无关（modality-agnostic）"的主张仍然有待在更广的模态谱系上确证；
  - 数据集类型偏少，若能在更多样化的自然刺激数据集（如记忆检索、想象任务）上验证则更有说服力。
- **基准评测的偏差风险**：
  - SEED-DV 与 Broderick 所采集的设备、被试数量和刺激范式存在差异，统一框架在两个数据集上的可比较性受限于数据集本身可能有不同的预处理流程和难度基线。
- **个体差异的解决程度**：
  - 跨被试对齐虽提升整体泛化，但对于特定被试（如低信噪比被试）是否仍然存在失败案例，摘要中未予以讨论。
- **信息可获取性限制相关局限**：
  - 本次分析基于的文本信息有限，未能覆盖论文中具体的消融实验数量、基线方法细节及统计显著性信息，因而在评价实验充分性时有一定不确定度。
- **应用限制**：
  - 语义解码依赖对应刺激的**语义标签或预训练语义模型**，在真实开放场景中（刺激语义未知）的应用仍需后处理；此外，EEG 采集设备的佩戴不适感、实时性等问题，也尚未在本文的实验范围内论证。

（完）
