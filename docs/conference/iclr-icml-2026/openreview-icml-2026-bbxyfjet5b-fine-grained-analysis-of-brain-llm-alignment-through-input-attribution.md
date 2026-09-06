---
title: Fine-grained Analysis of Brain-LLM Alignment through Input Attribution
title_zh: 通过输入归因对大脑与大语言模型对齐进行细粒度分析
authors: "Michela Proietti, Roberto Capobianco, Mariya Toneva"
date: 2026-04-30
pdf: "https://openreview.net/pdf/67e2f8af6c767a89c19a0cd9326998db4649c0e2.pdf"
tags: ["query:eeg-align"]
score: 8.0
evidence: 用归因找出大脑与LLM对齐的关键词，揭示语义和篇章信息在脑对齐中的作用
tldr: 理解LLM与大脑活动的对齐有助于揭示语言处理的计算原理。该工作将输入归因应用到脑-LLM对齐场景，识别对对齐最重要的词，并研究其与下一词预测的关系。在两个fMRI数据集上发现二者依赖不同的词子集：下一词预测偏向近因或首因与句法，脑对齐则更侧重语义与篇章信息。这为神经语义对应关系和语言模型的可解释性提供了细粒度方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 需要理解LLM与大脑语言处理的对齐机制，但未知驱动对齐的具体词汇与信息类型。
method: 提出归因分析管线，在脑-LLM对齐中定位最关键的词，并对比脑对齐与下一词预测依赖的词子集。
result: 在fMRI数据中发现脑对齐更依赖语义与篇章信息，下一词预测更依赖句法和近因效应。
conclusion: 提供细粒度分析脑-LLM对齐的方法，推进语言机制与类脑语义建模理解。
---

## Abstract
Understanding the alignment between large language models (LLMs) and human brain activity can reveal computational principles underlying language processing. This work describes a pipeline to apply attribution methods to the brain-LLM alignment setting to identify the specific words most important for this alignment. As a case study, we leverage it to study a contentious research question about brain-LLM alignment: the relationship between brain alignment (BA) and next-word prediction (NWP). Across two naturalistic fMRI datasets, we find that BA and NWP rely on largely distinct word subsets: NWP exhibits recency and primacy biases with a focus on syntax, while BA prioritizes semantic and discourse-level information with a more targeted recency effect. This work advances our understanding of how LLMs relate to human language processing and highlights differences in feature reliance between BA and NWP. Beyond this study, our attribution method can be broadly applied to explore the cognitive relevance of model predictions in diverse language processing tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究动机

- **背景**：大语言模型（LLM）与人类大脑活动之间存在可测量的“对齐”（brain-LLM alignment），理解这种对齐有助于揭示人类语言处理背后的计算原理。
- **争议问题**：大脑对齐（brain alignment, BA）与下一词预测（next-word prediction, NWP）之间究竟存在怎样的关系？此前研究对此存在分歧。
- **核心问题**：
  - 在大脑与 LLM 的对齐中，哪些**具体词汇**起着最关键的作用？
  - BA 与 NWP 所依赖的词汇子集和信息类型是否相同？
- **整体含义**：通过细粒度归因分析，可更精确理解 LLM 表征与神经语义表征的对应关系，也为语言模型的可解释性提供新工具。

## 2. 方法论

论文提出一个**将输入归因方法应用于脑-LLM 对齐场景的通用分析管线**，核心流程如下：

1. **设定脑-LLM 对齐范式**：
   - 使用自然刺激下采集的功能性磁共振成像（fMRI）数据。
   - 将 LLM 的深层表征与大脑体素（voxel）活动进行编码模型映射，衡量二者对齐程度。
2. **输入归因（Input Attribution）**：
   - 对输入文本中的每个词计算其对脑对齐得分的贡献/重要性。
   - 从而找出对 BA 最重要的“关键词子集”。
3. **对比 BA 与 NWP 的归因模式**：
   - 对“下一词预测”任务做相同的归因分析。
   - 比较两者依赖的词汇是否一致，并进一步分析这些词在句法、语义、篇章层面以及句内位置上的特征。
4. **归因分析维度**：
   - **近因/首因效应**：关键词是否集中在句子开头或结尾。
   - **句法与语义**：关键词是否偏向功能词/句法结构词，还是实义词/语义内容词。
   - **篇章信息**：关键词是否与跨句、篇章层面的信息相关。

（公式与具体算法细节在所提供的摘要与元数据中未展开，文中未提供显式公式。）

## 3. 实验设计

- **数据集**：
  - 使用了两个**自然主义 fMRI 数据集**（naturalistic fMRI datasets），即被试在自然语言刺激（如故事/连续文本）下记录的大脑活动数据。
  - 论文未在摘要中给出具体数据集名称，但从上下文推断这类研究常使用如 Pereira、Blank 等公开神经语言数据集。
- **Benchmark / 任务设定**：
  - 并非传统 NLP benchmark，而是以“大脑体素预测”为评价任务，即评估 LLM 表征能否通过线性映射预测 fMRI 响应。
  - 核心 benchmark 是“脑对齐分数”以及“下一词预测”的表现，用于比较二者的归因差异。
- **对比方法 / 分析对象**：
  - 将“大脑对齐（BA）”的归因结果与“下一词预测（NWP）”的归因结果进行对比。
  - 按词汇的语言属性和位置进行分层对比，分析其差异。
- **未提及传统基线模型对比**：即没有详细列出其他归因方法或基线 LLM 的对比。

## 4. 资源与算力

- **原文未明确说明**使用的 GPU 型号、数量、训练/推理时长或具体算力开销。
- 仅能推断该工作涉及 fMRI 数据 + LLM 编码 + 归因计算，对计算资源有一定需求；
- 论文的元数据和时间线亦未提供实验训练成本细节。
- **结论：无法从现有文本中评估算力消耗，需查阅原文或补充材料。**

## 5. 实验数量与充分性

- **实验组数**：从摘要看，实验覆盖了：
  - 两个独立 fMRI 数据集；
  - 两类任务的对比：BA vs NWP；
  - 多个归因维度：近因/首因、句法、语义、篇章信息；
  - 多个词汇层面的对比分析。
- **充分性评估**：
  - **优点**：双数据集能够在不同受试者和刺激材料间验证结论的一致性，增强可信度。
  - **潜在不足**：
    - 未提及消融实验（如不同归因方法、不同 LLM 结构、不同脑区/RoI 的单独分析）。
    - 未给出统计显著性的量化细节（如是否做了置换检验、多重比较校正）。
    - 未提到是否控制了句子长度、词频、语义范畴等混淆因素。
  - **客观性与公平性**：设计思路本身具有对比性，但若缺少基线归因方法（如梯度 × 输入、LIME、SHAP 等）的横向比较，则难以判断该归因方法的优势。

## 6. 主要结论与发现

1. **BA 和 NWP 依赖基本不同的词子集**。
2. **NWP 的特性**：
   - 存在**近因效应**和**首因效应**（关注句子开头和结尾附近的词）；
   - 更加关注**句法信息**（如句法结构相关词汇/功能词）。
3. **BA 的特性**：
   - 优先关注**语义信息**和**篇章/语篇层面的信息**；
   - 近因效应更加**选择性/有针对性**，不像 NWP 那样广泛依赖边缘位置。
4. **方法论贡献**：该归因管线可推广到其他语言处理任务中，用于判断模型预测的“认知相关性”。

## 7. 优点

- **细粒度视角**：不是整体比较表征相似度，而是定位到具体哪些词驱动了脑对齐，提供更细致的解释。
- **切入争议问题**：直接针对 BA 与 NWP 关系这一开放争议，用归因证据给出新回答。
- **双数据集验证**：提高结论的外推能力。
- **通用分析管线**：不仅适用于该问题，还能用于其他 NLP 任务的认知相关性分析。
- **揭示信息类型差异**：将词级归因与语言层次（句法/语义/篇章）结合，增强结论的可解释性。

## 8. 不足与局限

- **信息缺失**：当前文本只包含摘要和元数据，缺少：
  - 具体归因算法公式与实现细节；
  - 神经网络架构选择和词嵌入/隐状态提取方式；
  - 脑成像预处理与编码模型细节。
- **实验覆盖有限**：
  - 仅使用 fMRI，未扩展到 MEG/EEG 等高时间分辨率数据；
  - 只有两个 NLP 方向的任务对比（BA vs NWP），未探讨其他认知任务；
  - 未对不同 LLM（如不同规模和训练目标）做敏感性分析。
- **潜在偏差风险**：
  - 归因方法本身可能对模型内部机制存在简化；
  - “词汇重要性”可能受词频、共现、文本结构影响，若不控制容易产生混淆；
  - BA 的脑区选择（全脑还是特定语言网络）没有在摘要中提供，可能影响结论适用范围。
- **应用限制**：结果基于英语自然主义文本与特定 LLM 族群，是否适用于其他语言和跨模态刺激仍待验证。

（完）
