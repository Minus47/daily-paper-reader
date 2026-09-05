---
title: Dimensionality Mismatch Between Brains and Artificial Neural Networks
title_zh: 人脑与人工神经网络之间的维度失配
authors: "Santiago Galella, Maren Wehrheim, Matthias Kaschube"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=fyp34w19N2"
tags: ["query:abstraction"]
score: 8.0
evidence: 系统比较人脑腹侧视觉流与ANN的表征维度，显示语义和抽象表征随视觉层级涌现，为从具体到抽象的类脑概念组织提供证据
tldr: 人类腹侧视觉通路沿层级逐步提升表征维度，反映出语义和抽象表征的涌现，但人工神经网络并不总是与这种几何变化一致。论文系统比较了多种ANN在自然图像观看中的线性和非线性维度，发现两者存在明显的维度失配。研究强调合适的特征读出对大脑-模型对齐的重要性，并为构建从具体到抽象的类脑概念空间提供了维度约束。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 生物与人工视觉系统均具层级结构，但表征几何随层级的演化差异尚不明确。
method: 系统比较人类fMRI与多种ANN在自然图像观看下的线性与非线性表征维度。
result: 人类腹侧通路维度沿视觉层级上升，指示语义与抽象表征涌现；ANN趋势仅在池化特征下类似。
conclusion: 脑与模型在表征维度上的差异凸显特征读出方式的重要性，可为类脑概念空间设计提供约束。
---

## Abstract
Biological and artificial vision systems both rely on hierarchical architectures, yet it remains unclear how their representational geometry evolves across processing stages, and what functional consequences may arise from potential differences. In this work, we systematically quantify and compare the linear and nonlinear dimensionality of human brain activity (fMRI) and artificial neural networks (ANNs) during natural image viewing. In the human ventral visual stream, both dimensionality measures increase along the visual hierarchy, supporting the emergence of semantic and abstract representations. For linear dimensionality, most ANNs show a similar increase, but only for pooled features, emphasizing the importance of appropriate feature readouts in brain–model comparisons. In contrast, nonlinear dimensionality shows a collapse in the later layers of ANNs, pointing at a mismatch in representational geometry between the human and artificial visual systems. This mismatch may have functional consequences: while high-dimensional brain representations support flexible generalization to abstract features, ANNs appear to lose this capacity in later layers, where their representations become overly compressed. Overall, our findings propose dimensionality alignment as a benchmark for building more flexible and biologically grounded vision models.

---

## 论文详细总结（自动生成）

# 《人脑与人工神经网络之间的维度失配》中文总结

> 说明：可供分析的完整内容有限（仅有标题、作者、摘要与元数据），以下总结严格基于摘要信息展开；具体实验细节若摘要未给出，会明确标注“未在可用材料中说明”。

## 1. 核心问题与整体含义

- **研究背景**：生物视觉系统（人脑腹侧视觉通路）与人工神经网络（ANN）都采用层级化结构，但两者在逐级处理过程中的**表征几何（representational geometry）如何演化**，以及潜在差异会带来何种功能后果，仍不清楚。
- **核心问题**：人脑与 ANN 在自然图像观看过程中的表征维度是否存在系统性的“失配”？这种失配是否会影响模型的灵活性与泛化能力？
- **整体含义**：论文提出将“维度对齐”（dimensionality alignment）作为构建更具灵活性、更符合生物视觉机制的人工视觉模型的基准。其工作联系了类脑计算、表征几何分析和视觉感知建模三个方向。

## 2. 方法论

- **核心思想**：系统量化并比较人类 fMRI 大脑活动与多种人工神经网络在**自然图像观看**条件下的线性与非线性表征维度，并沿视觉处理层级追踪两种维度指标的变化趋势。
- **技术细节（根据摘要可提取的内容）**：
  - 使用**fMRI**记录人脑腹侧视觉流（ventral visual stream）的活动；
  - 对比多种 ANN 的中间层表征；
  - 分别计算**线性维度**与**非线性维度**，以覆盖表征几何的不同属性；
  - 对 ANN 特征考虑了**池化（pooled）与非池化**两种情况，考察特征读出方式对脑-模型比较的影响；
  - 比较大脑与模型在由浅至深层级上的维度变化轨迹。
- **公式/算法细节**：摘要中未给出具体定义或数学表达式；线性与非线性维度的具体计算方式（如本征维数估计方法、相似性核等）在可用材料中无法查证。

## 3. 实验设计

- **数据/场景**：采用**自然图像观看**任务；人类侧使用 **fMRI** 记录腹侧视觉流神经活动；ANN 侧输入相同或对应的自然图像。
- **Benchmark**：没有传统分类或识别 benchmark；其基准是**人脑—模型在层级维度变化上的一致性**，即大脑沿视觉层级维度的变化作为模型评估的参考锚点。
- **对比方法**：论文没有指定某一特定 ANN 架构，而是概述性地比较“多种人工神经网络”，并将 ANN 不同层与人脑不同视觉区域做对应分析；同时考察了**池化特征**与**原始特征**两类读出方式。
- **具体数据集与模型清单**：摘要未列出使用的 fMRI 数据集名称（如是否使用自然场景数据集 NSD 等）及 ANN 架构列表（如 ResNet、ViT 等）。

## 4. 资源与算力

- 可用材料（摘要与元数据）中**没有提及**任何算力信息，包括 GPU 型号、数量、训练/推理时长或能耗等。
- 由于该论文偏重分析（而非训练大型模型），资源消耗可能集中在 ANN 前向推理与特征计算上，但这一推测无法从论文提取内容中得到确认。

## 5. 实验数量与充分性

- **已知的实验内容**：
  - 人类与 ANN 的线性维度比较；
  - 人类与 ANN 的非线性维度比较；
  - ANN 特征在“池化/非池化”两种条件上的比较。
- **未知细节**：未给出不同数据集的交叉验证、参数扫描或消融实验信息；也未说明统计显著性检验方式、被试人数、ANN 架构数量等。
- **充分性判断**：仅凭摘要无法判断实验是否充分、客观和公平。摘要结论较清晰，但由于具体实验数量与实现细节缺失，**无法独立评估其结果的可重复性和鲁棒性**。

## 6. 主要结论与发现

- **人脑侧**：人类腹侧视觉流在从初级视觉皮层到高级语义区域的层级递进中，**线性维度和非线性维度均增加**，提示语义和抽象表征在此过程中涌现。
- **ANN 侧（线性维度）**：大多数 ANN 在“池化后的特征”上也呈现随层级升高而维度增加的趋势，但该趋势依赖于适当的特征读出——说明在使用 ANN 与大脑比较时，不能忽略读出方式的影响。
- **ANN 侧（非线性维度）**：在 ANN 的后期层中，非线性维度发生**坍缩（collapse）**，表征被过度压缩，这与人类大脑的表现形成明显失配。
- **功能后果**：人类大脑高维表征有利于对抽象特征进行灵活泛化；而 ANN 后期层因维度坍缩，可能丧失这种灵活性。
- **总体建议**：将“维度对齐”作为未来视觉模型评估与设计的一个重要约束或 benchmark，推动更具生物合理性和灵活性的模型发展。

## 7. 优点

- 选题具有跨学科价值，连接神经科学、表征几何与深度学习；
- 同时考察**线性和非线性维度**，比单纯使用某一维度指标更能反映表征的空间结构；
- 注意到**特征读出方式**（池化 vs 非池化）对脑-模型比较的干扰，体现了方法上的严谨性；
- 提出从人类腹侧视觉流维度变化中提炼可量化的约束，为类脑概念空间的构建提供了可行方向。

## 8. 不足与局限

- **信息完整度受限**：本次整理仅能依赖摘要，论文全文中的方法细节、数据来源、模型集合和统计分析均未给出；
- **模型范围不明确**：“多种 ANN”未列出具体架构，无法判断覆盖广度及公平性；
- **任务域单一**：仅涉及自然图像观看，未检验在其他视觉任务（如分类、分割、对抗鲁棒性、开放世界识别）中是否仍存在相同失配；
- **脑信号自身的局限性**：fMRI 空间分辨率与血流动力学延迟可能影响维度估计的精度，摘要未讨论这一影响；
- **缺乏干预性验证**：摘要没有提及是否通过操纵模型维度（如在损失中加入维度约束）来验证维度对齐的实际收益，因此因果性不足；
- **实际应用未展开**：尽管提出维度对齐可作为构建灵活视觉模型的指标，但没有给出如何将该约束引入网络训练的具体方案。

（完）
