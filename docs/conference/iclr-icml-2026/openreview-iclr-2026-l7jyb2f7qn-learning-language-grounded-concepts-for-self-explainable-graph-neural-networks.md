---
title: Learning Language-grounded Concepts for Self-explainable Graph Neural Networks
title_zh: 面向自解释图神经网络的语言锚定概念学习
authors: "Xiaoxue Han, Libo Zhang, Zining Zhu, Yue Ning"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=l7jyB2f7Qn"
tags: ["query:abstraction"]
score: 5.0
evidence: 将图映射到自然语言表达的概念瓶颈，通过信息瓶颈构造可解释因果概念空间，可用于概念结构构建
tldr: 现有图神经网络解释大多以子图为主，缺少人类可理解的概念级语义。本文提出图概念瓶颈GCB，将图映射为自然语言短语组成的概念空间，并利用信息瓶颈原则过滤虚假概念、突出因果概念。实验表明GCB能产生更紧凑且更忠实的解释，同时引导模型朝正确方向决策。该范式为概念级自解释图神经网络提供了新的构建思路。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 图神经网络的可解释性通常限于子图，无法给出概念层级的人类可读解释。
method: 将图表示为语言短语概念瓶颈，用信息瓶结构筛选因果相关概念，再基于概念预测结果。
result: 产生更简洁、更聚焦因果的概念解释，并引导模型作出更准确决策。
conclusion: 语言化概念瓶颈为图神经网络的概念级自解释提供了可复用方案。
---

## Abstract
We introduce Graph Concept Bottleneck (GCB) as a new paradigm for self-explainable Graph Neural Networks. GCB  maps graphs into a concept space—a concept bottleneck—where each concept is a natural language phrase, and predictions are made based on these concepts. Unlike existing interpretable GNNs that primarily rely on subgraphs as explanations, the concept bottleneck provides a more human-understandable form of interpretation. To refine the concept space, we apply the information bottleneck principle to encourage the model to focus on causal concepts instead of spurious ones. This not only yields more compact and faithful explanations but also explicitly guides the model to think toward the correct decision. We empirically show that GCB achieves intrinsic interpretability with accuracy on par with black-box GNNs. Moreover, it delivers better performance under distribution shifts and data perturbations, demonstrating improved robustness and generalizability as a natural byproduct of concept-based reasoning.

---

## 论文详细总结（自动生成）

# 论文简要总结

## 1. 核心问题与研究动机

- **背景**：图神经网络（GNN）在诸多图结构数据任务上表现优异，但传统可解释方法通常提供子图级别的解释，这类解释缺乏高层次、人类可读的语义。
- **核心问题**：如何让 GNN 以**概念级**（concept-level）而非仅子图级的方式实现自解释？也就是让解释形式从“哪些边/节点重要”提升为“哪些人类可理解的语义概念驱动了预测”。
- **核心动机**：概念瓶颈（concept bottleneck）方法在图像等领域已被证明能提供更直观的解释；将该范式引入图数据，有潜力增强解释的**可读性**与**可信度**。

## 2. 方法论：Graph Concept Bottleneck（GCB）

- **核心思路**：将图映射到一个“概念空间”中，使每个概念都用**自然语言短语**表示；模型的预测直接基于这些概念进行，而非仅基于原始图结构。
- **技术框架要点**：
  1. **概念瓶颈构造**：将输入图编码到由自然语言短语组成的概念瓶颈（concept bottleneck）中，每个概念对应一个可理解的语义标签。
  2. **信息瓶颈原则**（information bottleneck principle）：在精炼概念空间时，通过信息瓶颈原则筛选与预测任务**因果相关**的概念，抑制虚假关联或伪相关（spurious）的概念。
  3. **基于概念的预测**：最终预测在概念层上进行，使得模型的决策路径自身具备可解释性，即“以概念为依据做推理”。
- **方法论意义**：这种设计让解释与预测共享同一表征通道，属于**内生可解释**（intrinsic interpretability）而非事后解释。

## 3. 实验设计

- **数据与场景**：原文在摘要中明确提及了以下两类评估场景：
  - 分布偏移（distribution shifts）下的性能表现；
  - 数据扰动（data perturbations）下的鲁棒性表现。
- **Benchmark**：与**黑盒 GNN**（black-box GNN）进行对比，验证 GCB 在保持可解释性的同时是否牺牲准确性。
- **对比方法**：由于有效的论文正文未提供完整内容，无法确认具体对比了哪些现有可解释 GNN 方法（如 GNNExplainer、PGExplainer、GraphLIME 等），文本中仅明确提到与黑盒 GNN 做基准比较。

> **注**：当前提供的文本资料仅包含摘要与元数据，未包含正文的实验章节，因此无法提供更详细的数据集名称、统计信息或对比方法清单。

## 4. 算力与资源

- 当前提取到的论文内容**未明确说明**所使用的 GPU 型号、数量、训练轮次、训练时间等资源信息。
- 因此，无法从现有文本中获知具体算力投入情况。

## 5. 实验数量与充分性

- 基于现有的文本信息，摘要部分显示实验覆盖了：
  - 准确性对比（与黑盒 GNN 相近）；
  - 分布偏移下的性能；
  - 数据扰动下的鲁棒性。
- **充分性判断**：受限于文本资料不足，无法判断是否做了消融实验、多数据集泛化实验、概念质量人工评测等。但从方法论本身来看，GCB 涉及概念空间构建、信息瓶颈筛选等多个核心设计环节，若要证明各个组件的必要性与有效性，**消融实验是必不可少的**，目前摘要中未提及此类内容。
- **客观性与公平性**：仅凭摘要难以全面评估实验公平性，需结合正文的 baseline 设置与超参配置来判断。

## 6. 主要结论与发现

- GCB 能够在保持与黑盒 GNN 相当精度的前提下，提供**概念级**的自解释能力。
- 通过信息瓶颈原则筛除伪相关概念，使解释更**紧凑**与**忠实**（faithful）。
- GCB 在**分布偏移**和**数据扰动**场景下表现更好，说明概念级推理能作为副产品提升模型的**鲁棒性与泛化性**。
- 整体上，GCB 展示了概念瓶颈方法在图神经网络中实现“人类可理解”的自解释是可行且有前景的方向。

## 7. 优点与亮点

- **新颖性较强**：将自然语言概念瓶颈引入图神经网络领域，突破了传统子图解释的局限，让解释能直达“因果概念”。
- **可读性突出**：以自然语言短语作为概念载体，解释对于非专家用户更加友好。
- **内生可解释设计**：预测本身建立在概念之上，而非“先预测再事后解释”，有助于避免事后解释不忠实的问题。
- **泛化增益**：通过信息瓶颈筛选因果概念，不仅提升了可解释性，还带来了鲁棒性和分布外泛化能力提升，具备“解释与性能兼得”的潜力。
- **元数据的额外证据提示**：该工作可复用于概念结构构建，说明方法具有一定的通用性或可扩展性。

## 8. 不足与局限

- **方法侧潜在局限**：
  - 概念空间需要预定义或学习一组自然语言短语，若概念字典不完备，可能限制表达能力；
  - 如何保证学习到的“概念”确实对应真实因果机制，而非拟合了概念标签的统计相关性，仍需进一步论证；
  - 信息瓶颈原则在筛除伪相关概念时，若压缩过于激进，也可能丢失有效任务信息。
- **实验侧局限**：
  - 当前提供的材料里**缺少具体数据集、baseline 细节与消融实验**，难以全面判断方法的普适性与各部分设计的贡献度；
  - 未提供人工评估或用户研究等直接验证“解释可理解性”的实验证据；
  - 未明确说明是否覆盖多种图类型（如分子图、社交图、知识图谱等）。
- **应用侧局限**：
  - 要求概念本身可被表示为自然语言短语，在某些科学或工业场景中（如节点特征高度数值化），构建概念字典的可行性仍有待验证；
  - 概念瓶颈方法若需要外部概念标注，可能引入额外的人工标注成本。

---

（完）
