---
title: The Deleuzian Representation Hypothesis
title_zh: 德勒兹式表征假说
authors: "Clément Cornet, Romaric Besançon, Hervé Le Borgne"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=10JEfJtiJM"
tags: ["query:abstraction"]
score: 6.0
evidence: 从激活差异中无监督提取概念，可用于构建概念词汇表
tldr: 针对神经网络中可解释概念提取问题，本文提出类稀疏自编码器的无监督方法，通过判别分析框架聚类激活差异并利用激活偏度加权增强概念多样性。实验覆盖五个模型和视觉、语言、音频三种模态。结果显示该方法在概念质量上超越先前无监督SAE变体且接近有监督基线。作为一种通用概念抽取手段，可为构建大规模概念词汇表或语义概念空间提供支撑。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有稀疏自编码器等方法提取神经网络概念时质量有限，且缺乏无需标注的概念发现方式。
method: 将激活差异进行聚类，并以激活偏度加权提高概念多样性，在判别分析框架下说明概念作为差异表示的合理性。
result: 在五种模型三种模态上的概念质量超过无监督SAE变体，并接近有监督基线。
conclusion: 建立一种无监督通用概念抽取路径，可用于构建面向可解释性与下游语义空间的概念资源。
---

## Abstract
We propose an alternative to sparse autoencoders (SAEs) as a simple and effective unsupervised method for extracting interpretable concepts from neural networks. The core idea is to cluster differences in activations, which we formally justify within a discriminant analysis framework. To enhance the diversity of extracted concepts, we refine the approach by weighting the clustering using the skewness of activations. The method aligns with Deleuze's modern view of concepts as differences. We evaluate the approach across five models and three modalities (vision, language, and audio), measuring concept quality, diversity, and consistency. Our results show that the proposed method achieves concept quality surpassing prior unsupervised SAE variants while approaching supervised baselines, and that the extracted concepts enable steering of a model’s inner representations, demonstrating their causal influence on downstream behavior.

---

## 论文详细总结（自动生成）

# 中文总结

> 说明：本次输入内容仅包含论文元数据与摘要，PDF 全文被 OpenReview 的浏览器验证页拦截。因此，以下总结以所给摘要和 metadata 为基础；凡原文未提供的细节均明确标注为“未提供”，不作臆测。

## 1. 核心问题与整体含义
- **研究动机**：可解释人工智能领域需要从神经网络内部提取“概念”。现有方法（如稀疏自编码器）虽然是目前主流工具，但概念提取质量仍然有限，且缺少一种完全不需要标注、同时又能产出高质量概念的通用思路。
- **核心问题**：能否提出一种不同于稀疏自编码器的、简单且有效的无监督可解释概念提取方法？
- **整体含义**：论文主张将“概念”理解为激活向量之间的“差异”，并在判别分析框架中为这种观点提供理论依据。该视角与德勒兹（Deleuze）关于“概念即差异”的哲学观点相呼应。
- **意义**：提供了一种通用的无监督概念抽取路径，可能用于构建大规模概念词汇表、语义概念空间，或支持对模型内部表示进行可控编辑。

## 2. 方法论
- **核心思想**：不再像稀疏自编码器那样从单个激活向量中重构/稀疏编码出特征，而是对“激活差异”进行聚类；聚类结果被视为概念。
- **理论支持**：将概念提取问题置于判别分析（discriminant analysis）框架下进行形式化论证，说明聚类激活差异为何能够对应到具有判别意义的概念方向。
- **多样性优化**：
  - 直接聚类激活差异可能造成概念偏向某些高频模式；
  - 论文使用**激活的偏度（skewness）**作为权重对聚类过程进行加权，从而增强提取概念的多样性。
- **哲学映射**：将概念定义为差异而非实体，因此模型某层中的“概念”可以表现为一组典型差异方向/差异原型。
- **整体算法流程（依据摘要推断，原文未给公式）**：
  1. 采集模型在不同输入下的激活值；
  2. 构造激活之间的差异向量；
  3. 依据激活偏度对差异向量加权；
  4. 对加权后的差异向量执行聚类；
  5. 将聚类中心（或类代表）作为提取到的概念；
  6. 利用提取的概念做内部表征操控（steering）以验证其因果作用。

> 注：具体聚类算法、特征层选择、差异构造方式、偏度权重的具体形式等在所给文本中未提供。

## 3. 实验设计
- **覆盖范围**：
  - 共 5 个模型；
  - 跨越 3 种模态：视觉（vision）、语言（language）、音频（audio）。
- **评估指标**：摘要中明确提到了三个层面：
  - 概念质量（quality）；
  - 概念多样性（diversity）；
  - 概念一致性（consistency）。
- **对比方法**：
  - 先前已有的无监督稀疏自编码器变体（prior unsupervised SAE variants）；
  - 有监督基线（supervised baselines）。
- **下游验证**：还进行了模型内部表征的“转向/操控”（steering）实验，以验证提取的概念是否对下游行为具有因果影响力。
- **Benchmark/数据集**：所给材料中未列出具体数据集名称、benchmark 名称以及涉及的具体模型架构。

## 4. 资源与算力
- **原文未提供**关于 GPU 型号、数量、训练时长、显存占用、能耗或总计算量的任何信息。
- 仅凭现有摘要无法估算该工作的算力成本。

## 5. 实验数量与充分性
- **已有实验量**：
  - 横跨 5 个模型、3 个模态，属于多模型、多模态的广度验证；
  - 在 3 个不同维度（质量、多样性、一致性）上进行了定量评测；
  - 加入了一个干预性实验（steering），用于验证概念的方向是否真正影响下游行为。
- **充分性评估**：
  - 从摘要看，实验覆盖面较宽，且同时报告了“超越无监督 SAE 变体”与“接近有监督基线”的对比结果，能够在一定程度上说明方法的通用性和竞争力。
  - 但所给文本没有提供具体实验数量、消融实验、统计显著性检验、多种超参数报告等细节；
  - 因此仅凭摘要无法判断实验在统计层面的充分性，也难以核实与基线比较时超参调优是否完全公平。
  - 若要在学术层面“充分”，还需补充逐数据集结果、不同层的差异、聚类数敏感性、偏度权重是否带来稳定增益等分析——这些在本文档中均未出现。

## 6. 主要结论与发现
- 所提出的基于激活差异聚类的无监督方法，在概念质量上**超越先前的无监督稀疏自编码器变体**。
- 同时，其质量**接近有监督基线**，说明监督信号并非不可替代。
- 提取到的概念可以用于**有效地操控模型内部表示**，并对下游行为产生可测的因果影响。
- 总体上，论文建立了一种“无监督通用概念抽取”的可行路径，可服务于可解释性研究和语义概念空间构建。

## 7. 优点
- **概念新意**：将“概念 = 差异”引入神经网络可解释性，并用判别分析而非稀疏重构为其建模，规避了稀疏自编码器的一些固有缺陷。
- **简洁有效**：所提方法被描述为“simple and effective”，避免了稀疏自编码器中复杂的字典学习/正则化设置。
- **多样性机制**：引入激活偏度作为权重，在聚类框架中自然增强概念的多样性，思路直观。
- **多模态验证**：实验覆盖视觉、语言、音频三类信号，五个模型，比仅在单一模态上验证的传统可解释性工作更具一般性。
- **因果验证**：不只做相关分析或特征可视化，还通过 steering 实验验证概念具备因果影响力，提升了概念解释的可信度。
- **结果有竞争力**：能在无监督条件下接近有监督基线，说明实用性较高。

## 8. 不足与局限
- **原文信息受限**：由于本次只拿到摘要和元数据，无法对方法细节做真正的技术和算法级评审。
- **哲学框架的可操作性问题**：“德勒兹式表征”在论文中主要作为启发式框架存在；摘要未明确说明该哲学观点如何具体转化为损失函数或聚类目标，容易显得偏隐喻。
- **数据集与模型细节缺失**：未列出使用了哪些视觉/语言/音频数据集、模型具体是哪些（如 ViT/BERT/clap 等），也未说明是从哪些网络层提取激活，导致结论的直接可复现性不明。
- **对比公平性难以确认**：与无监督 SAE 变体和有监督基线比较时，是否使用了统一的概念数量、同一特征分辨率和相同评测协议，从摘要中无法判断。
- **缺少消融验证**：摘要没有说明“偏度加权”是否单独做了消融，因此无法确认该组件对多样化提升的独立贡献。
- **下游应用范围有限**：除 steering 实验外，没有在其他可解释性任务（如模型编辑、去除概念、推荐系统、偏见检测等）上的表现。
- **评估客观性风险**：概念质量本身难以定义，摘要未说明评测工具（如是否引入人类评估、是否使用真实标签概念字典），可能存在主观基准偏差。

---

（完）
