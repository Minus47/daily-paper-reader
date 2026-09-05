---
title: In Silico Mapping of Visual Categorical Selectivity Across the Whole Brain
title_zh: 全脑视觉类别选择性的计算机模拟映射
authors: "Ethan Hwang, Hossein Adeli, Wenxuan Guo, Andrew Luo, Nikolaus Kriegeskorte"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=B23WUS3W8Z"
tags: ["query:abstraction"]
score: 5.0
evidence: 全脑视觉类别选择性映射揭示感官驱动的具体概念在皮层中的组织。
tldr: 脑成像研究常依赖预设类别假设，而线性编码模型难以刻画大脑非线性组织。作者提出计算机模拟方法，以编码器-解码器Transformer配合脑区-图像特征交叉注意力，在全脑范围学习视觉刺激到脑活动的非线性映射。该方法能以数据驱动方式发现新的类别选择性区域，而不局限于预先定义的具体视觉类别。这有助于揭示类似具体概念在皮层中的选择性分布，为类脑概念空间提供神经先验。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 预设类别假说和线性编码难以无偏刻画视觉类别在大脑中的复杂分布。
method: 结合脑区-图像特征的交叉注意力，用编码器-解码器Transformer建立非线性映射以扫描全脑类别选择性。
result: 数据驱动地发现了新的视觉类别选择性区域，比传统线性模型更具表达能力。
conclusion: 提供一种可扩展的脑映射工具，有助于了解具体视觉概念如何在大脑皮层中组织。
---

## Abstract
A fine-grained account of functional selectivity in the cortex is essential for understanding how visual information is processed and represented in the brain. Classical studies using designed experiments have identified multiple category-selective regions; however, these approaches rely on preconceived hypotheses about categories. Subsequent data-driven discovery methods have sought to address this limitation but are often limited by simple, typically linear encoding models. We propose an in silico approach for data-driven discovery of novel category-selectivity hypotheses based on an encoder–decoder transformer model. The architecture incorporates a brain-region to image-feature cross-attention mechanism, enabling nonlinear mappings between high-dimensional deep network features and semantic patterns encoded in the brain activity. We further introduce a method to characterize the selectivity of individual parcels by leveraging diffusion-based image generative models and large-scale datasets to synthesize and select images that maximally activate each parcel. Our approach reveals regions with complex, compositional selectivity involving diverse semantic concepts, which we validate in silico both within and across subjects. Using a brain encoder as a “digital twin” offers a powerful, data-driven framework for generating and testing hypotheses about visual selectivity in the human brain—hypotheses that can guide future fMRI experiments. Our code is available at: https://kriegeskorte-lab.github.io/in-silico-mapping-web/.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：大脑皮层对视觉刺激的功能选择性是理解视觉信息处理与表征的关键，但传统脑成像研究通常依赖设计好的实验和**预先设定的类别假设**（如面孔、场景等），这限制了新发现的可能。
- **现有局限**：后续的数据驱动发现方法虽试图克服预设偏差，但大多局限于**简单线性编码模型**，难以刻画大脑中复杂、非线性的语义组织。
- **整体含义**：论文提出一种**计算机模拟（in silico）** 的脑映射框架，旨在不依赖先验类别的情况下，全脑范围数据驱动地发现视觉类别选择性区域，为后续假设生成和fMRI实验设计提供指导。

## 2. 论文提出的方法论

- **核心思想**：利用深度生成式Transformer建立视觉刺激到脑活动的非线性映射，并通过合成图像来“激活”特定脑区，从而表征脑区的选择性。
- **关键技术细节**：
  - 采用**编码器–解码器Transformer架构**。
  - 引入**脑区-图像特征的交叉注意力（brain-region to image-feature cross-attention）机制**，实现高维深度网络特征与脑活动语义模式之间的非线性映射。
  - 结合**基于扩散模型的图像生成器**和大规模图像数据集，为每个脑区（parcel）合成并筛选能最大程度激活该脑区的图像，从而刻画其选择性。
- **算法流程（文字说明）**：
  1. 输入图像特征（来自深度网络）和采样到的脑活动数据。
  2. 通过交叉注意力将脑区信息与图像特征对齐，学习从图像到全脑活动模式的非线性编码模型。
  3. 利用训练好的脑编码器作为“数字孪生”，对每个脑区进行“反向”搜索：使用扩散模型生成候选图像，并筛选出使该脑区预测激活最大化的图像。
  4. 对筛选图像进行语义分析，揭示该脑区的组合性、构成性选择性。

## 3. 实验设计

- **数据集**：摘要及元数据中提及使用“大规模数据集”进行图像合成与选择，但未给出具体数据集名称（如自然图像数据集、特定的fMRI数据集等）。
- **验证方式**：
  - 在**被试内（within subjects）** 和**被试间（across subjects）** 进行计算机模拟验证。
  - 使用脑编码器作为“数字孪生”来生成和检验关于视觉选择性的假设，而非直接开展新的fMRI实验。
- **Benchmark与对比方法**：未在摘要中明确指出对比了哪些具体基线方法，但暗示与“传统的简单线性编码模型”相比具备更高表达力；具体对比细节需查阅论文正文。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明**所使用的算力资源，如GPU型号、数量、训练时长、参数量等。
- 如果需要获取该信息，需查阅论文正文或附录。

## 5. 实验数量与充分性

- 从摘要内容看，实验设计包括：
  - 全脑范围内进行类别选择性扫描与映射。
  - 使用扩散模型生成并选择激活图像。
  - 在被试内/被试间进行验证。
- **充分性评估**：由于仅提供了摘要，无法判断具体实验组数、消融实验或统计检验细节。摘要声明其方法“揭示了……区域”并“验证了”结果，但**缺少定量指标和对比实验的公开描述**，其客观性和公平性难以据此全面评判。

## 6. 论文的主要结论与发现

- 该方法能够**数据驱动地发现新的视觉类别选择性区域**，不局限于预先定义的具体视觉类别。
- 发现的脑区表现出**复杂的、组合性的选择性**，涉及多种语义概念的混合，而非单一类别。
- 通过“数字孪生”脑编码器生成图像并选择最强激活图像，可有效表征单个parcel的选择性特征。
- 该框架可为未来fMRI实验提供**可扩展的假设生成工具**，有助于揭示具体视觉概念在皮层中的组织原则。

## 7. 优点

- **摆脱预设类别**：无需先验假设即可发现新区域，避免实验设计偏差。
- **非线性建模能力强**：使用Transformer和交叉注意力，克服了传统线性编码模型的表达限制。
- **可解释的假设生成**：借助扩散模型合成图像，能生成直观、语义上可解读的激活刺激，助于理解脑区功能。
- **跨被试验证**：在同一被试和不同被试间进行验证，增强结果可靠性。
- **应用价值**：为脑成像实验提供“数字孪生”前置筛选，提升实验效率与发现潜力。

## 8. 不足与局限

- **描述完整性**：当前仅提供摘要，方法论细节、网络架构具体配置、训练策略、脑区划分标准、数据预处理等均未展示。
- **实验验证**：没有提供与传统方法的定量比较、消融研究或统计显著性信息，难以衡量性能提升幅度与稳健性。
- **数据偏差风险**：使用深度网络特征和大型图像数据集，可能继承数据本身的分布偏差；同时扩散模型生成图像可能引入生成伪影或非自然的刺激，影响选择性评估。
- **跨被试验证范围**：仅提及验证策略，未报告样本量或是否覆盖多个独立数据集，可能导致结论泛化性有限。
- **应用限制**：作为“in silico”方法，其假设仍需实际fMRI实验检验，可能存在编码模型预测误差放大风险。

（完）
