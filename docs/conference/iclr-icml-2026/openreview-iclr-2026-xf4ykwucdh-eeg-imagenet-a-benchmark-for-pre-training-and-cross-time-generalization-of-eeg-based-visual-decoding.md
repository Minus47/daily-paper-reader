---
title: "EEG-ImageNet: A Benchmark for Pre-training and Cross-Time Generalization of EEG-based Visual Decoding"
title_zh: EEG-ImageNet：面向EEG视觉解码的预训练与跨时间泛化基准
authors: "Shuqi Zhu, Ziyi Ye, Qingyao Ai, Yiqun LIU"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=xf4ykWUcDH"
tags: ["query:eeg-align"]
score: 9.0
evidence: 构建含16名被试EEG的视觉解码基准，支持预训练与跨时间评测，直接服务于EEG与图像语义的对齐和检索研究。
tldr: 针对EEG视觉解码中数据集不足和块设计带来的时间混淆问题，作者提出EEG-ImageNet（CrossPT-EEG）基准，采集16名被试的脑电数据。该基准定义了跨被试与跨时间范围的预训练和评测协议，使模型可以从EEG中识别或重建视觉图像内容。通过提供大规模可复现的数据划分与泛化评测条件，这项研究降低了EEG与图像语义对齐研究的数据门槛，为低成本、高时间分辨率的视觉解码奠定了评测基础。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG视觉解码缺乏大规模高质量数据且块设计产生时间混淆，限制跨被试与跨时间的模型泛化研究。
method: 采集16名被试视觉感知EEG，构建CrossPT-EEG/EEG-ImageNet基准，并制定预训练与跨时间评测协议。
result: 建成16人规模EEG视觉解码基准，可为预训练和跨时间泛化提供标准化测评，支撑EEG与图像语义对应研究。
conclusion: 该基准缓解EEG数据稀缺与时间混淆问题，为脑电解码视觉的表示学习和语义对齐提供公共评测平台。
---

## Abstract
Exploring brain activity in relation to visual perception provides insights into the biological representation of the world. 
While functional magnetic resonance imaging (fMRI) and magnetoencephalography (MEG) have enabled effective image classification and reconstruction, their high cost and bulk limit practical use. 
Electroencephalography (EEG), by contrast, offers low cost and excellent temporal resolution, but its potential has been limited by the scarcity of large, high-quality datasets and by block-design experiments that introduce temporal confounds.
To fill this gap, we present CrossPT-EEG, a benchmark for cross-participant and cross-time generalization of visual decoding from EEG. 
We collected EEG data from 16 participants while they viewed 4,000 images sampled from ImageNet, with image stimuli annotated at multiple levels of granularity. 
Our design includes two stages separated in time to allow cross-time generalization and avoid block-design artifacts.
We also introduce benchmarks tailored to non-block design classification, as well as pre-training experiments to assess cross-time and cross-participant generalization. 
These findings highlight the dataset's potential to enhance EEG-based visual brain-computer interfaces, deepen our understanding of visual perception in biological systems, and suggest promising applications for improving machine vision models.

---

## 论文详细总结（自动生成）

> 注意：你提供的内容实际是 OpenReview 的一个验证拦截页面，而非完整论文全文；但我从中提取到了该论文的“元数据 + Abstract”。以下总结只能基于**标题、作者、摘要、元数据字段**进行推断，无法覆盖正文中的公式、算法和结果细节。凡无法确认的内容我会明确标出“摘要未提供”。

# EEG-ImageNet 中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **研究背景**：
  - 通过脑电等神经信号理解视觉感知，有助于揭示生物系统对世界的表征方式；
  - fMRI 和 MEG 在图像分类/重建上已有较好效果，但**成本高、设备庞大**，难以实用化；
  - EEG 成本低、时间分辨率高，是更具落地潜力的脑信号模态。
- **核心痛点**：
  - EEG 视觉解码领域**缺乏大规模、高质量数据集**；
  - 已有实验常采用 **block-design（块设计）**，会在时间上引入系统性混淆（如类别与时间段相关），导致模型学到时间模式的捷径而非真实视觉语义，进而限制了**跨时间、跨被试**的泛化能力。
- **本文目标**：
  - 构建一个面向 EEG 视觉解码的**基准数据集与评测协议**，支撑 EEG 与图像语义之间的对齐、分类与重建研究，为脑机接口和机器视觉提供新资源。

## 2. 方法论：核心思想与技术细节

- **核心思想**：从数据采集与评测协议两端同时入手——提供“大规模、多被试、多粒度标注、非块设计”的 EEG 视觉数据集，并定义“预训练—跨时间评估”的标准化协议，减少时间混淆、促进跨被试/跨时间泛化研究。
- **数据集构建（CrossPT-EEG，又名 EEG-ImageNet）**：
  - 从 ImageNet 中抽取 **4,000 张自然图像**作为视觉刺激；
  - 招募 **16 名被试**观看图像并同步记录 EEG；
  - 图像刺激提供**多层级标签标注**（对应 ImageNet 的类别语义粒度）；
  - 为避免 block-design 带来的时间混淆，实验采集设计**刻意非块化**；
  - 采集流程分为**两个时间上分离的阶段**，专门用于评估跨时间泛化。
- **评测任务与协议**：
  - 设计适用于**非块设计分类**的 benchmark；
  - 给出基于预训练范式的评测方法，用于测试 EEG 模型在跨时间和跨被试条件下的表现；
  - 整体评测既面向“从 EEG 识别视觉内容”的分类任务，也隐含支撑“从 EEG 重建图像”的生成式视觉解码研究。
- 摘要中**未披露**具体网络结构、损失函数或训练流程的技术公式。

## 3. 实验设计：数据集与 Benchmark

- **实验数据集**：自建的 CrossPT-EEG / EEG-ImageNet；
  - 数据规模：16 名被试；4,000 张 ImageNet 图像刺激；
  - 数据结构：同一图像刺激具备多粒度语义标注、分两阶段时间采集。
- **基准任务场景**：
  1. **非块设计分类**：衡量模型能否从 EEG 中正确识别被试所看的图像类别；
  2. **跨时间评测**：利用两阶段分离的时间结构，检验模型的跨时间泛化能力；
  3. **跨被试评测**：评测模型是否能在不同被试之间泛化；
  4. **预训练模式**：通过预训练-微调范式，评判数据集对 EEG 预训练任务的支撑能力。
- **对比方法**：
  - 摘要**没有列出**具体 baseline（如与已有 EEG 数据集或图像解码模型进行的数值对比），可能位于正文中，但当前文本未提供。

## 4. 资源与算力

- **明确说明**：摘要和元数据中**均未披露任何算力信息**（如 GPU 型号与数量、训练时长、显存开销等）；
- 因此本文**无法总结实验的算力成本**；
- 原因可能是：在 OpenReview 截取的信息不完整，或算力细节本就没有写进摘要，需要看正文。

## 5. 实验数量与充分性（客观评估）

- **已呈现的实验/分析**（摘要层面）：
  - 16 名被试、4,000 张图像的采集实验；
  - 两阶段分离时间采集的设计；
  - 分类下游评测；
  - 预训练与跨时间/跨被试泛化实验；
  - 与脑机接口、视觉感知、机器视觉等应用前景的定性讨论。
- **实验充分性的判断**：
  - 从摘要看，**benchmark 的结构设计和时间混淆的规避是相对成熟的**；
  - 但摘要没有展示任何定量结果（如准确率、与 SOTA 的对比、消融表、统计显著性），因此**无法客观判断实验是否公平、充分**；
  - 具体基线对比、消融分析、时间/被试的泛化量化数据只有在正文中才可获得。

## 6. 主要结论与发现

- **结论一**：本文构建了一个**避免时间混淆、面向跨时间/跨被试泛化**的 EEG 视觉解码基准数据集（CrossPT-EEG / EEG-ImageNet）；
- **结论二**：该数据集可以被用于**预训练 + 下游分类**的研究范式，验证模型在时间与被试维度上的泛化；
- **结论三**：基于该基准的探索性研究结果表明，EEG 视觉解码数据可以推动：
  - EEG 驱动的视觉脑机接口技术；
  - 对生物视觉感知机制的理解；
  - 对机器视觉模型的启发和改进。
- 总体定位：它是一个**数据与评测层面的基础设施性工作**，而非提出某一具体 SOTA 解码算法。

## 7. 优点

- **数据规模与结构**：16 名被试、4,000 张自然图像在多被试 EEG 视觉数据中规模较大；
- **从源头规避时间混淆**：放弃传统 block 设计，采用分阶段、非块化的刺激时序，这在基准建设思路上是重要改进；
- **面向跨时间泛化**：两阶段采集设计为该领域提供了稀缺的“纵向测评”窗口；
- **标注丰富**：ImageNet 提供多粒度类别标签，有助于做语义层级上的对齐分析；
- **对齐当前研究趋势**：支持预训练评测，呼应 EEG 基础模型与 EEG-图像跨模态对齐的研究热点；
- **公共基准价值**：有助于社区将 EEG 视觉解码从“各做各的”转向可复现、可比较的标准化评测。

## 8. 不足与局限

- **信息局限性**：本次可获取内容仅含摘要，未能评估方法细节、结果表格、统计检验和基线对比；
- **被试规模**：16 名被试虽不算少，但对强泛化性的结论而言仍偏小，存在个体差异与脑电噪声带来的偏差风险；
- **设备与标注**：摘要未说明 EEG 设备规格（通道数/采样率），也未说明多粒度标注来自 ImageNet 标签的具体层级——需依赖原文；
- **应用边界**：摘要没有展示在真实脑机接口场景中的在线评测或实际下游任务验证；
- **是否确认论文被拒**：该论文来源标注为 ICLR 2026 Submitted（Public / Rejected），可能存在审稿人关注的问题，例如新意、基线公平性或实验验证深度——此点仅作为上下文参考，不代表原文缺陷；
- **指标与可复现性说明不足**：Crossref 元数据中既无代码链接，也没有在摘要中给出数据可用性计划（如是否公开 Raw EEG）。

---

**总体评价**：从摘要和元数据看，该工作回应了 EEG 视觉解码的两个真实痛点——**数据稀缺与块设计时间混淆**，并以“大规模数据采集 + 规范化泛化评测协议”的方式做出了贡献，提出了一个命名为 CrossPT-EEG / EEG-ImageNet 的基准。它的应用价值更偏向**基础设施与评测标准**，而非某一网络结构的性能突破。受限于可见材料（无正文），对方法细节和实验充分性的最终评判需要进一步阅读论文全文。

（完）
