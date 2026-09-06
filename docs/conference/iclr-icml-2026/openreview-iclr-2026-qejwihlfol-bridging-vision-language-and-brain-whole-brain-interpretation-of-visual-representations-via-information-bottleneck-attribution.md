---
title: "Bridging Vision, Language, and Brain: Whole-Brain Interpretation of Visual Representations via Information Bottleneck Attribution"
title_zh: 桥接视觉、语言与大脑：基于信息瓶颈归因的视觉表征全脑解释
authors: "Haoyu Li, Hao Wu, Liangjun Chen, Badong Chen"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=qEjWihLFol"
tags: ["query:eeg-align"]
score: 8.0
evidence: 将脑功能磁共振影像信号与CLIP图像文本语义嵌入对齐，建立脑信号与多模态语义的公共表征空间
tldr: 理解人脑如何整合视觉与语言信息历来困难。该工作先用保持解剖拓扑的整脑表征模块，将fMRI体素信号与CLIP图像和文本嵌入对齐，同时保留体素空间拓扑与分布式脑动态；再提出基于信息瓶颈的脑归因方法IBBA，解释视觉表征在皮层上的归因。结果显示其对齐与归因较基线更符合脑响应，为脑信号与图像文本语义的跨模态对齐提供了可复用的通用框架。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 神经科学需要解释大脑如何将视觉与语言信息整合到皮层表征中。
method: 设计保拓扑的整脑表征模块将fMRI体素与CLIP图文嵌入对齐，并提出信息瓶颈脑归因方法IBBA。
result: 在脑影像与视觉语言语义的对齐和归因解释上优于已有基线，符合皮层分布式动态。
conclusion: 脑信号可与大规模多模态语义模型直接对齐，用于视觉感知的脑机制解释。
---

## Abstract
Understanding how the human brain processes and integrates visual and linguistic information is a long-standing challenge in both cognitive neuroscience and artificial intelligence. In this work, we present two contributions toward attributing visual representations in the cortex by bridging brain activity with natural modalities. We first align fMRI signals with image and text embeddings from a pre-trained CLIP model by proposing a whole-brain representation module that follows anatomical alignment, preserves voxel spatial topology, and captures distributed brain dynamics. Building on this foundation, we further develop an Information Bottleneck-based Brain Attribution (IB-BA) method, which extends information-theoretic attribution to a tri-modal setting. IB-BA identifies the most informative subset of voxels for visual tasks by maximizing mutual information with image and text embeddings while enforcing compression relative to perturbed brain features. Experiments demonstrate superior cross-modal retrieval performance and yield more interpretable cortical attribution maps compared to existing approaches. Collectively, our findings point to new directions for linking neural activity with multimodal representations.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究背景**：理解人脑如何加工并整合视觉与语言信息，是认知神经科学与人工智能领域的长期挑战。
- **核心问题**：现有将大脑 fMRI 信号与多模态语义嵌入对齐的工作缺乏全脑空间拓扑保持能力，也难以在脑区细粒度上定位视觉表征，无法回答“哪些皮层区域承担哪些视觉语义成分”这一问题。
- **整体含义**：该工作试图在脑影像数据与多模态语义空间之间建立结构化、可解释的对应关系，为构建类脑概念层级及其神经基础提供可行的分析范式。

## 2. 方法论

论文包含两个主要贡献，构成“先对齐、后归因”的完整流程：

- **全脑表示模块（Whole-Brain Representation Module）**
  - 将 fMRI 信号与预训练 CLIP 模型的图像嵌入和文本嵌入进行对齐。
  - 设计上遵循**解剖对齐**原则，按大脑解剖结构组织模型模块，而不是把全脑体素当平铺向量处理。
  - 保留**体素空间拓扑**，避免传统方法中空间结构信息的丢失。
  - 能够捕获**分布式脑动态**，而非孤立的局部响应。

- **信息瓶颈脑归因方法（Information Bottleneck-based Brain Attribution, IB-BA）**
  - 把信息论归因扩展到“脑—图像—文本”三模态设置中。
  - 核心思路是：最大化所选体素子集与图像/文本嵌入之间的互信息，以保留与视觉任务相关的语义信息；同时相对于扰动后的脑特征施加压缩约束，去除冗余。
  - 最终目标是找出对视觉任务**信息量最大且最紧凑的体素子集**，从而定位承载特定视觉语义的皮层区域。

## 3. 实验设计

- **数据与基准**：论文使用 fMRI 信号与对应图像/文本描述进行跨模态对齐与归因实验，涉及视觉刺激下的脑活动记录。由于摘要内容有限，未明确列出数据集名称（但从实验内容推断应为包含“图像—文本—fMRI”三元组的大规模公共神经成像数据集）。
- **评测任务**：
  1. **跨模态检索性能**，衡量 fMRI 与图像/文本嵌入的对齐质量；
  2. **皮层归因图的可解释性**，即生成的脑区级归因结果是否具有清晰的神经生物学语义。
- **对比方法**：与已有脑—语义对齐与归因方法进行比较；摘要未给出具体基线名称，但表明 IB-BA 在检索性能和归因可解释性上均优于现有方法。

## 4. 资源与算力

- 所提供的论文提取文本中**未明确说明**使用的 GPU 型号、数量、训练时长或算力开销，因此无法给出具体算力总结。
- 实验中涉及的 fMRI 数据预处理和 CLIP 嵌入推理通常要求较高显存，但具体配置需要参考论文原稿的实验设置部分。

## 5. 实验数量与充分性

- 论文进行了跨模态检索和脑归因图两类核心实验，但摘要中未展示实验数量表格，也未明确列出消融实验的组数。
- 已知比较了归因方法和检索性能，且确认在可解释性上优于现有方法，说明有相对照的基准。
- **充分性评估**：摘要层面的信息不足以判断消融设计是否覆盖所有模块（如：去除拓扑约束、去除解剖对齐、改变压缩强度等）；同时缺少统计显著性、逐体素验证或行为学交叉验证细节，故**实验的全面性有待原文验证**。

## 6. 主要结论与发现

- 所提出的全脑表示模块能够有效将 fMRI 信号与 CLIP 图像/文本嵌入对齐，并获得**优于现有方法的跨模态检索性能**。
- IB-BA 方法能识别出对视觉任务最具信息量的体素子集，生成比现有方法**更可解释的皮层归因图**。
- 总体来说，该工作证明了将多模态（视觉—语言）语义空间与全脑活动建立细粒度对应是可行的，为脑—多模态表征链接提供了**新的研究方向**。

## 7. 优点

- **方法新颖性**：引入信息瓶颈概念用于体素级归因，并将归因问题在三模态（脑—图—文）下正式化。
- **空间拓扑保留**：相比常用的体素平铺或 ROI 平均方法，全脑模块保持了解剖与空间拓扑，更贴合神经科学现实。
- **系统完整性**：采用“表示对齐 + 归因分析”两层架构，既解决嵌入空间连通性，也解决语义定位可解释性，逻辑连贯。
- **跨学科价值**：工作同时面向 AI（多模态对齐）和认知神经科学（皮层语义映射）两个群体，具有较强的交叉领域溢出效应。

## 8. 不足与局限

- **完整学习内容有限**：当前提供的内容以摘要为主，缺少数据划分、预处理细节、具体实验配置和伪代码，故无法全面验证方法与结果。
- **依赖预训练 CLIP**：整体对齐效果受限于 CLIP 表征的质量与模态覆盖范围；若脑活动包含 CLIP 未覆盖的视觉信息，可能会被丢失。
- **压缩与任务依赖的平衡**：IB-BA 中选择信息量与压缩强度的平衡是方法关键，但摘要未展示该超参数对结果的敏感性分析。
- **潜在偏差风险**：fMRI 数据通常存在个体差异、头动伪影、生理噪声等干扰，论文若未采用额外去噪或健壮性分析，归因图可能部分受噪声影响。
- **应用局限**：若未来希望用于临床（如视觉障碍患者的语义定位），还需在更多群体、任务模式和长时程数据上做外部验证。

（完）
