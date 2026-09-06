---
title: "ViEEG: Hierarchical Visual Neural Representation for EEG Brain Decoding"
title_zh: ViEEG：面向脑电解码的分层视觉神经表示
authors: "Minxu Liu, Donghai Guan, Chuhang Zheng, Chunwei Tian, Jie Wen, Qi Zhu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/6cc1ca714b70889700eadd325b51f651d9b8c40a.pdf"
tags: ["query:eeg-align"]
score: 8.0
evidence: 把视觉刺激分解成轮廓、物体和场景等语义成分，建立分层神经表示用于脑电解码
tldr: 现有脑电视觉解码方法大多使用扁平表示，忽略大脑视觉皮层的分层组织。ViEEG受视觉皮层层次编码启发，将视觉刺激分解为轮廓、前景物体和上下文场景三类生物学对齐的锚定成分，并据此构造分层神经表示。相比扁平表示，这种表示更贴合大脑由低层到高层的视觉加工路径。该项工作为脑电视觉解码提供了生物启发的表示学习思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 脑电视觉解码中使用的扁平表示忽略视觉皮层分层组织，导致神经表示与脑机制不符。
method: 将视觉刺激分解为轮廓、前景物体和上下文场景语义锚点，引导脑电学习层次化视觉表示。
result: 实验验证分层视觉锚点可改善脑电视觉解码的表现和神经合理性。
conclusion: 借鉴大脑分层加工能提升脑电解码能力，为视觉脑解码提供生物学启发框架。
---

## Abstract
Understanding and decoding brain activity into visual representations is a fundamental challenge at the intersection of neuroscience and artificial intelligence. While electroencephalogram (EEG) visual decoding has shown promise due to its non-invasive and low-cost nature, existing methods suffer from {Hierarchical Neural Encoding Neglect (HNEN)}, a critical limitation in which flat neural representations fail to model the brain’s hierarchical visual processing. Inspired by the hierarchical organization of visual cortex, we propose ViEEG, a neuro-inspired framework that addresses HNEN. ViEEG decomposes each visual stimulus into three biologically aligned components, namely contour, foreground object, and contextual scene, which serve as anchors for a three-stream EEG encoder. These EEG features are progressively integrated via cross-attention routing, simulating cortical information flow from low-level to high-level vision. We further adopt hierarchical contrastive learning for EEG-CLIP representation alignment, enabling zero-shot object recognition. Extensive experiments on THINGS-EEG dataset demonstrate that ViEEG significantly outperforms previous methods by a large margin in both subject-dependent and subject-independent settings. Results on THINGS-MEG dataset further confirm ViEEG's generalization to different neural modalities. ViEEG not only advances the performance frontier but also sets a new paradigm for EEG brain visual decoding. Our code is available at https://github.com/LauMason/ViEEG.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- 论文关注脑电解码（EEG brain decoding），旨在将大脑活动信号转换为相应的视觉表征，是神经科学与人工智能交叉领域的基础挑战。
- 作者指出现有 EEG 视觉解码方法普遍存在 **分层神经编码忽视（Hierarchical Neural Encoding Neglect, HNEN）** 问题：即使用扁平化（flat）的神经表示，未能刻画大脑视觉皮层“由低层到高层”的分层组织方式。
- 由于脑电图具有非侵入性、成本低等优点，该方向有重要应用价值，但建模方式与真实脑机制不符会限制解码性能与神经可解释性，是本文要解决的核心矛盾。

## 2. 论文提出的方法论

- **核心思想**：提出 ViEEG，一种神经启发式（neuro-inspired）框架，受视觉皮层分层组织启发，将视觉刺激解构成三类生物学对齐的视觉语义组件，作为指导 EEG 编码的分层锚点。
- **视觉刺激分解**：每个视觉刺激被分解为三个层次组件：
  - 轮廓（contour）：对应低层视觉特征；
  - 前景物体（foreground object）：对应中层视觉语义；
  - 上下文场景（contextual scene）：对应高层视觉语义。
- **三流 EEG 编码器**：使用三个并行编码器分支（three-stream EEG encoder）分别以上述三种语义成分为锚点进行特征提取。
- **跨注意力路由与渐进融合**：不同分支的 EEG 特征通过交叉注意力机制（cross-attention）进行路由并渐进式整合，模拟大脑皮层从低级到高级视觉的信息流动过程。
- **分层对比学习**：引入分层对比学习策略，实现 EEG 与 CLIP 视觉表示的分层对齐，从而支持零样本（zero-shot）物体识别。
- 论文未给出具体数学公式细节；方法流程可概括为“视觉刺激分解为三层锚点 → 三流 EEG 编码 → 跨注意力渐进融合 → 分层对比对齐”的端到端管线。

## 3. 实验设计

- **主数据集**：在 THINGS-EEG 数据集上进行评估，该数据集是 EEG 视觉解码领域的标准基准。
- **实验设置**：同时覆盖 subject-dependent（依赖受试者）与 subject-independent（跨受试者）两种场景。
- **任务目标**：
  - EEG-CLIP 表示对齐评估；
  - 零样本物体识别性能评估。
- **跨模态验证**：额外在 THINGS-MEG 数据集上实验，以验证 ViEEG 对脑磁图（MEG）等其他神经模态的泛化能力。
- **对比方法**：摘要中只说明“显著优于此前方法”，但**未列出具体基线方法名称**（如未见与 EEG2Image、Brain2Image 等既有方法的逐一对比清单）。
- **Benchmark 说明**：THINGS 数据集（含 EEG 与 MEG 变体）是当前视觉脑解码研究中较通用的 benchmark，但摘要未详述所用子集规模与预训练/微调协议。

## 4. 资源与算力

- 提供的材料中**未提及**任何算力相关信息：例如 GPU 型号、数量、训练时长、参数量或能源消耗等均未说明。
- 当前内容无法判断训练该模型所需的计算资源规模。

## 5. 实验数量与充分性

- 根据摘要明确说明的实验维度包括：
  1. THINGS-EEG 上的 subject-dependent 实验；
  2. THINGS-EEG 上的 subject-independent 实验；
  3. THINGS-MEG 上的泛化实验（跨神经模态）。
- **未明确说明是否包含消融实验**（如去掉分层锚点、去掉三流结构、去掉跨注意力路由等各自的贡献）——通常情况下此类论文会包含消融分析，但当前所提供文本并未给出相关图表或数据。
- **充分性评估**：
  - 优点：同时评估主数据集+跨模态泛化、受试者内/跨受试者两种范式，覆盖了该领域重要的评估维度；
  - 不足：由于未提供完整实验细节（数值结果、统计显著性、基线列表、消融与可视化分析），无法据此判断其全面程度与公平性；报告的实验矩阵看起来合理但“数量”与“深度”均不足以支撑完全客观的评估。

## 6. 论文的主要结论与发现

- ViEEG 在 THINGS-EEG 数据集中的 subject-dependent 与 subject-independent 设置下均显著优于以往方法。
- 在 THINGS-MEG 数据集上的结果进一步验证了 ViEEG 可泛化到其他神经信号模态。
- 生物启发的分层视觉语义锚点（轮廓/物体/场景）能够有效改善脑电解码的性能与神经合理性。
- 作者认为 ViEEG 不仅刷新了性能纪录，也为 EEG 视觉解码确立了新范式。

## 7. 优点

- **生物合理性**：直接借鉴视觉皮层中腹侧通路（ventral stream）由低层到高层的分层加工规律来设计网络结构，弥补了现有扁平表示的不足之处。
- **结构清晰**：三流编码器加跨注意力路由的方案，在结构上天然对应“低层—中层—高层”三级语义，可解释性较好。
- **任务全面**：同时包含分类级别的解码与零样本开放类别识别，兼顾了判别能力与语义泛化能力。
- **跨模态验证**：将方法推广到 MEG 数据，证明其不局限于 EEG 单一信号，具有一定普适性。
- **附带开源代码**：提供了 GitHub 代码链接，便于复现与后续研究。

## 8. 不足与局限

- **信息完整性受限**：当前可供分析的材料仅限于标题、元数据与摘要，方法细节、公式、超参数与完整实验结果均不可见，难以做深度批判性分析。
- **对比基线不透明**：摘要未列出任何基线方法名称与实验数值，因此“大比例提升”的说法在客观公平性上缺乏可查验的证据。
- **实验覆盖仍可扩展**：
  - 未见跨数据集迁移（如训练于 THINGS-EEG 再测试于其他 EEG 数据）；
  - 未见对真实应用场景（如脑机接口在线解码）的讨论；
  - 未见受试者间存在差异较大（如病理、年龄等异质性人群）情况下的鲁棒性分析。
- **消融不明确**：材料中未报告对三类锚点各自贡献、三流结构不同融合方式等的消融比较，方法的每一设计模块是否都必要仍不清楚。
- **零样本识别的语义边界**：所分解的“轮廓—物体—场景”三层是否完备、各层类别设定是否受限于预训练 CLIP 语义空间，文中摘要未能描述，潜在的偏差与限制需要进一步审视。
- **计算成本未知**：未报告训练资源与推理开销，可能限制资源受约束条件下的可复现性和应用部署。

（完）
