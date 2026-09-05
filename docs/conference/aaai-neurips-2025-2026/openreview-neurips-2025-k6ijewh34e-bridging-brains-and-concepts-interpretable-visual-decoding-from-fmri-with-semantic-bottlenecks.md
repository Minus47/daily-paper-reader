---
title: "Bridging Brains and Concepts: Interpretable Visual Decoding from fMRI with Semantic Bottlenecks"
title_zh: 连接大脑与概念：基于语义瓶颈的fMRI可解释视觉解码
authors: "Sara Cammarota, Matteo Ferrante, Nicola Toschi"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=K6ijewH34E"
tags: ["query:abstraction"]
score: 8.0
evidence: 将fMRI映射到二值语义特征空间，以语义瓶颈实现可解释视觉解码
tldr: 非侵入性fMRI视觉刺激解码性能在近年大幅提升，但高水平解码模型大多依赖不可解释的潜在空间。该研究在BrainDiffuser线性解码流程中插入语义瓶颈，先将图像表达为214维二值可解释语义空间，每个维度回答一个图像语义问题，并以岭回归把体素活动映射至该语义空间。由于瓶颈维度具有明确语义，后续图像解码既能维持较高精度，也能让研究者看到大脑激活对应哪种显式概念，在脑-概念解码之间架起可解释桥梁。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 高性能fMRI视觉解码通常使用复杂且不透明的潜空间，难以提供关于视觉概念的可解释信息。
method: 在BrainDiffuser中设置语义瓶颈层，先用214维二值问题维表示图像，再用岭回归从体素映射到该语义空间并继续生成图像。
result: 实验表明该方法在保持解码质量的同时提供语义解释，可重建或识别图像中的显式概念维度。
conclusion: 该框架将神经信号映射到可解释语义维度，为脑-视觉概念关系研究提供有效工具。
---

## Abstract
Decoding of visual stimuli from noninvasive neuroimaging techniques such as functional magnetic resonance (fMRI) has advanced rapidly in the last years; yet, most high-performing brain decoding models rely on complicated, non-interpretable latent spaces. In this study we present an interpretable brain decoding framework that inserts a semantic bottleneck into BrainDiffuser, a well established, simple and linear decoding pipeline. We firstly produce a $214-\text{dimensional}$ binary interpretable space $\mathcal{L}$ for images, in which each dimension answers to a specific question about the image (e.g., "Is there a person?", "Is it outdoors?"). A first ridge regression maps voxel activity to this semantic space. Because this mapping is linear, its weight matrix can be visualized as maps of voxel importance for each dimension of $\mathcal{L}$, revealing which cortical regions influence mostly each semantic dimension. A second regression then transforms these concept vectors into CLIP embeddings required to produce the final decoded image, conditioning the BrainDiffuser model. We found that voxel-wise weight maps for individual questions are highly consistent with canonical category-selective regions in the visual cortex (face, bodies, places, words), simultaneously revealing that activation distributions, not merely location, bear semantic meaning in the brain. Visual brain decoding performances are only slightly lower compared to the original BrainDiffuser metrics (e.g., the CLIP similarity is decreased by $\leq 4$% for the four subjects), yet offering substantial gains in interpretability and neuroscientific insights. These results show that our interpretable brain decoding pipeline enables voxel-level analysis of semantic representations in the human brain without sacrificing decoding accuracy.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **背景与动机**：基于非侵入性功能磁共振成像（fMRI）的视觉刺激解码技术近年来取得显著进展，但现有高性能解码模型大多依赖复杂的、不可解释的潜在空间（latent space），难以揭示大脑在处理视觉信息时具体编码了哪些可解释的概念维度。
- **核心问题**：如何在保持较高解码精度的同时，为fMRI视觉解码提供显式、可解释的语义信息，从而在“大脑活动和视觉概念”之间建立可理解的桥梁。

## 2. 方法论

- **核心思想**：在已有BrainDiffuser（一个简单线性解码流程）中插入一个“语义瓶颈”（semantic bottleneck），使解码过程先经过一个可解释的语义空间，再生成图像。
- **关键技术细节**：
  - 为图像构造一个**214维二值可解释语义空间** \(\mathcal{L}\)，每个维度对应一个关于图像的特定语义问题（如“图中是否有人？”“是否为户外场景？”），答案以二值（0/1）表示。
  - **第一级岭回归**：将fMRI体素活动映射到这个214维语义空间。由于该映射是线性的，权重矩阵可直接可视化为“体素重要性图”，从而定位影响每个语义维度的皮质区域。
  - **第二级回归**：将上述语义概念向量转换为CLIP嵌入，用于条件化BrainDiffuser模型，生成最终的解码图像。
- **流程总结**：fMRI体素 →（线性岭回归）→ 可解释语义空间 →（第二级回归）→ CLIP嵌入 → BrainDiffuser图像生成。

## 3. 实验设计

- **数据集/场景**：从摘要推断，实验基于fMRI视觉刺激解码的标准范式，涉及四名被试（subjects）的脑活动数据。
- **Benchmark**：以原始BrainDiffuser解码流程作为基准，衡量其性能表现。
- **对比方法**：主要与原始BrainDiffuser（无可解释瓶颈）进行对比，以评估插入语义瓶颈后对解码质量的影响。
- **评估指标**：使用了CLIP相似度（CLIP similarity）等视觉解码常用指标。

## 4. 资源与算力

- 论文元数据和摘要中**未明确说明**使用的GPU型号、数量或训练时长等算力信息。
- 仅提及方法为线性回归和已有生成模型的条件化，整体计算开销可能相对较小，但不能据此断定实际训练规模。

## 5. 实验数量与充分性

- **实验规模**：摘要中提到四名被试的CLIP相似度对比（下降≤4%），并验证了体素权重图与视觉皮层经典类别选择性区域（面孔、身体、地点、文字）的一致性。
- **充分性评估**：
  - 优势：通过四名被试的一致结果，以及可解释权重图与已知神经科学发现的匹配，初步证明了方法的有效性。
  - 不足：元数据未给出具体图像数据集规模、消融实验（如语义维度数量变化、不同回归方法比较）等细节，实验覆盖范围有限，不足以全面评估方法在不同刺激类型、不同脑区、不同个体间的泛化性能。
  - 总体而言，实验设计指向明确但信息不完整，需依赖论文正文进一步判断公平性与充分性。

## 6. 主要结论与发现

- **性能与可解释性的平衡**：插入语义瓶颈后，视觉解码性能较原始BrainDiffuser仅略有下降（CLIP相似度降低≤4%），但可解释性大幅提升。
- **神经科学洞察**：各语义问题对应的体素权重图与视觉皮层中经典的类别选择性区域高度一致，表明大脑激活的“分布模式”（而非仅位置）包含语义信息。
- **方法价值**：该框架能够将神经信号映射到明确的语义维度上，为研究大脑与视觉概念之间的关系提供了有效工具，同时仍能完成高质量的图像重建。

## 7. 优点

- **创新性**：提出“语义瓶颈”概念，在高精度解码流程中引入显式、可解释的中间表示，克服了传统黑箱隐空间的问题。
- **简洁有效**：基于线性映射即可实现体素层级语义分析，方法简单且具有一定神经科学依据。
- **可解释性强**：214维二值语义空间具有天然的语义可读性，使得每个维度的脑区映射可以直接回答“哪些概念由哪些脑区编码”。
- **实用价值**：为后续脑-视觉概念关系研究、脑机接口中的可解释解码提供了新思路。

## 8. 不足与局限

- **信息缺失**：论文素材中未提供完整实验设置、数据规模、训练细节、消融实验和统计显著性检验，难以充分评估方法的适用范围。
- **性能上限**：解码精度仍略低于原BrainDiffuser，语义瓶颈可能丢失部分难以用二元语义描述的视觉信息。
- **语义空间设计主观性**：214个维度的问题定义主要由人工预设，可能无法覆盖所有视觉概念，且存在概念间关联或冗余。
- **线性映射假设**：体素到语义空间采用岭回归，可能难以捕捉复杂的非线性神经编码关系。
- **被试数量有限**：仅提及四名被试，样本量较小，个体差异和泛化能力有待更大规模验证。
- **未讨论计算资源**：缺乏算力和训练时间说明，影响可重复性评估。

（完）
