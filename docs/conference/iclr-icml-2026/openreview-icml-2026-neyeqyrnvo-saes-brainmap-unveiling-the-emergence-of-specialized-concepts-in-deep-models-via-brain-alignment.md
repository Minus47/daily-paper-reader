---
title: "SAEs-BrainMap: Unveiling the Emergence of Specialized Concepts in Deep Models via Brain Alignment"
title_zh: SAEs-BrainMap：通过大脑对齐揭示深度模型中特殊概念的出现
authors: "Ziming Mao, Jia Xu, Wenxuan Pan, Mufan Xue, Yaochu Jin, Guoyuan Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/54474867a0eaf203d20043de21c83d4771bf410f.pdf"
tags: ["query:abstraction"]
score: 8.0
evidence: 以人类腹侧视觉通路脑激活为探针，将深度模型稀疏概念特征与生物脑区层级对齐
tldr: 深层视觉模型的概念涌现难以直接解释，且缺少与神经科学对接的客观指标。SAEs-BrainMap提出利用人脑腹侧视觉通路的激活模式作为客观探针，去引导和标注稀疏自编码器分解出的模型特征。结果显示这些稀疏特征与多个生物视觉脑区呈现稳定的表征对齐，可由此追踪专门概念在深度网络中的层级化涌现。该工作展示了用脑信号刻画深度学习概念结构并建造类脑概念空间的路径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 深度网络中视觉概念如何涌现仍不透明，缺少与人类脑区结构的客观校准。
method: 利用腹侧视觉通路脑激活指导稀疏自编码器特征提取，以脑区对齐识别深度模型中的专门概念。
result: 稀疏模型特征与多个视觉ROI存在稳健对齐，并可追踪概念层级化形成。
conclusion: 脑激活可作为刻画深度学习概念结构的探针，促进可解释AI与类脑视觉表示研究。
---

## Abstract
Understanding the internal mechanisms of deep neural networks remains a significant challenge, particularly in elucidating how generic visual concepts emerge within latent spaces. In this work, we propose SAEs-BrainMap, a novel framework that utilizes human brain activation patterns from the ventral visual pathway as objective probes to guide the identification of features decomposed by Sparse Autoencoders (SAEs). Our quantitative and qualitative empirical results demonstrate a robust representational alignment between sparse model features and biological Regions of Interests (ROIs), confirming the feasibility of utilizing brain signals to characterize model functionality. By leveraging this alignment, we trace the hierarchical trajectory of generic concepts cross layers and utilize the brain's hierarchical structure to visualize the model's global processing flow, providing novel insights into model interpretability. Our code is available at [this repository](https://github.com/Matzohe/SAEs-BrainMap).

---

## 论文详细总结（自动生成）

# SAEs-BrainMap 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- 深度神经网络（尤其是视觉模型）的内部机制仍然是一个黑箱，学界难以直接解释**通用视觉概念（generic visual concepts）如何在深层网络的潜在空间（latent space）中涌现**。
- 现有可解释性方法（如特征可视化、探针分析）大多缺少**与神经科学对接的客观基准**，难以判断模型中的概念是否与人类认知结构一致。
- 人类大脑的腹侧视觉通路（ventral visual pathway）已被证实是处理物体识别的层级化神经结构，可为深度模型的层级表示提供天然的“参照系”。
- 因此，该论文提出用**人类脑激活模式作为客观探针**，去“校准”深度模型内部的概念表征，从而揭示深度模型中的专门化概念及其层级化涌现路径。

## 2. 方法论

- **核心思想**：将人脑腹侧视觉通路的激活模式作为监督信号或对齐目标，引导和标注由 Sparse Autoencoders（SAEs，稀疏自编码器）分解得到的深度网络稀疏特征；通过大脑 ROI（Region of Interest，感兴趣区）与模型特征之间的表征对齐，来识别和定位模型中的专门概念。
- **关键组件**：
  - **稀疏自编码器（SAEs）**：用于将深层模型的特征分解为稀疏、可解释的组件特征（构成概念雏形）。
  - **脑激活数据**：来自人类腹侧视觉通路的 fMRI 或类脑响应数据，覆盖多个视觉 ROI（如早期视觉皮层、V4、LO、FFA、PPA 等）。
  - **表征对齐技术**：通过表征相似性分析（RSA）或线性/非线性映射等方法，度量模型稀疏特征与各个脑区激活模式的相似度。
- **流程（文字描述）**：
  1. 输入自然图像到深度视觉模型，提取中间层特征。
  2. 用 SAE 对特征进行稀疏分解，获得大量稀疏特征向量。
  3. 获取同一批图像对应的人类腹侧视觉通路脑激活数据。
  4. 计算每个稀疏特征与各个脑 ROI 激活模式之间的表征对齐程度。
  5. 将对齐程度高的稀疏特征标注为“与特定脑区相关的专门概念”。
  6. 跨层追踪这些特征，观察其层级化涌现轨迹，并以大脑的层级结构为模板，可视化模型的全局信息处理流。

## 3. 实验设计

- 由于本文提供的原始内容仅为摘要和元数据，**未包含完整的实验细节**，无法得知具体的数据集、场景与基线方法。但可合理推断：
  - 使用的数据会涉及：深度模型特征提取用图像数据集（如 ImageNet 等）、人类脑激活数据集（如 NSD、BOLD5000 等公开视觉脑成像数据集）。
  - Benchmark 主要为模型特征与生物视觉 ROI 之间的表征对齐程度（如 RSA 相关系数）等指标。
  - 对比方法可能包括：不引入脑对齐的普通 sparse feature 分析、其他可解释性方法（如激活最大化、概念激活向量等）、或不同 SAE 配置的对比。
- 需要说明的是，摘要中并未报告多个实验组的具体数量与对比细节。该工作属于 **ICML-2026 接收论文**，完整实验应在正文中详细展开。

## 4. 资源与算力

- 论文提供的文本中**没有明确说明**使用了多少 GPU（型号、数量）或训练时长等信息。
- 因此无法从该内容中获取具体算力开销；估计涉及大规模深度模型特征提取、SAE 训练以及脑数据回归分析，通常需要较高算力，但具体数值未知。

## 5. 实验数量与充分性

- 从摘要可知，实验包含**定量（quantitative）和定性（qualitative）**两种结果，覆盖多个视觉 ROI，并进行了跨层追踪与分析。
- 但受限于文本信息，**无法判断完整的实验数量**、是否包含消融实验、对照实验是否全面等。
- 初步从结果描述看，实验能支撑“稀疏模型特征与生物 ROI 存在稳健对齐”这一核心主张，但由于缺少具体数据，其充分性尚不能全面评估。公平性方面有待阅读全文后确认。

## 6. 主要结论与发现

- 深度模型中的 SAE 稀疏特征与人脑腹侧视觉通路的多个 ROI 之间表现出**稳健的表征对齐**。
- 该对齐关系可用于**追踪通用概念随网络深度逐层涌现的层级轨迹**。
- 借助大脑本身的层级组织，可以**可视化模型内部全局处理流程**，为模型可解释性提供全新视角。
- 验证了“用脑信号刻画深度学习概念结构”这一路径的可行性。

## 7. 优点与亮点

- **跨学科创新**：将人工智能可解释性与认知神经科学结合，以生物脑作为“地标”揭示人工网络的概念结构，具有较强的原创性。
- **客观指标引入**：与主观视觉解释不同，脑对齐提供一种生物上可验证的定量标准，减少人为偏差。
- **借助 SAE**：SAE 能获得更细粒度、更稀疏的概念因子，比直接分析特征图更易与脑区对应。
- **层级映射**：利用大脑腹侧通路的天然层级结构，为深度模型的深度维度提供有意义的坐标轴，有助于理解层级化概念形成。

## 8. 不足与局限性

- **脑数据的覆盖范围有限**：仅使用腹侧视觉通路，主要对视觉对象识别相关概念有效，对其他模态或更抽象概念（如场景语义、语言、数学推理）适用性未知。
- **对齐可能存在混淆因素**：图像的低级统计特征也可能导致部分脑区激活相似，需要控制低级视觉属性才能确保对齐反映的是“概念”层级。
- **解释粒度受限**：SAE 特征数量巨大，哪些特征真正对应于某一脑区的“专门概念”仍需启发式筛选，存在模糊性。
- **计算成本高**：训练 SAE 并匹配多维脑激活数据需要大规模数据与算力，实际应用门槛较高。
- **缺乏下游验证**：摘要未说明这种脑对齐是否能够提升模型性能或推动更进一步的可解释任务，例如概念干预、模型编辑等。
- **实验信息在所提供的文本中严重缺失**，包括数据集规模、对比基线、消融实验及统计显著性的具体数值，限制了对结论稳健性的判断。

（完）
