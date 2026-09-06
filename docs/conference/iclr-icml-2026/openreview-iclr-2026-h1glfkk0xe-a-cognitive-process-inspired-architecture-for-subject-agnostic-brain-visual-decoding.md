---
title: A Cognitive Process-Inspired Architecture for Subject-Agnostic Brain Visual Decoding
title_zh: 受认知过程启发的被试无关脑视觉解码架构
authors: "Jingyu Lu, Haonan Wang, Qixiang ZHANG, Xiaomeng Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=H1GLFKk0xE"
tags: ["query:eeg-align"]
score: 6.0
evidence: 将视觉皮层腹侧-背侧通路结构作为大脑先验引入多层次解码，提升脑信号的跨被试视觉重建/解码性能。
tldr: 无被试专属训练的脑视觉解码在临床和脑机接口中很有价值，但跨被试泛化困难。作者提出VCFlow，一种模拟人类视觉系统腹侧-背侧通路的分层解码架构，显式解耦早期视觉皮层、腹侧与背侧流的特征并加以融合。该设计以类脑认知结构作为归纳偏置来增强跨被试的表征一致性，可提升fMRI视觉重建的泛化效果，为从脑信号中获取视觉语义提供了可迁移的认知先验范式。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 跨被试无个性化训练的脑视觉解码受制于脑内信号复杂性与被试差异，现有方法尚未充分引入认知结构先验。
method: 提出VCFlow神经架构，显式建模视觉皮层腹侧-背侧流，通过分离早期视觉皮层等不同层级的特征进行跨被试解码。
result: 在框架设计上通过分离背侧与腹侧流特征提高跨被试视觉表征的一致性与语义重建能力，摘要未给出定量结果。
conclusion: 将认知加工通路结构纳入网络设计，能改善被试无关脑解码的泛化能力，为神经信号与视觉语义的对齐提供类脑建模思路。
---

## Abstract
Subject-agnostic brain decoding, which aims to reconstruct continuous visual experiences from fMRI without subject-specific training, holds great potential for clinical applications. However, this direction remains underexplored due to challenges in cross-subject generalization and the complex nature of brain signals.
In this work, we propose Visual Cortex Flow Architecture (VCFlow), a novel hierarchical decoding framework  that explicitly models the ventral-dorsal architecture of the human visual system to learn multi-dimensional representations. By disentangling and leveraging features from early visual cortex, ventral, and dorsal streams, VCFlow captures diverse and complementary cognitive information essential for visual reconstruction.
Furthermore, we introduce a feature-level contrastive learning strategy to enhance the extraction of subject-invariant semantic representations, thereby enhancing subject-agnostic applicability to previously unseen subjects. 
Unlike conventional pipelines that need more than 12 hours of per-subject data and heavy computation, VCFlow sacrifices only 7\% accuracy on average yet generates each reconstructed video in 10 seconds without any retraining, offering a fast and clinically scalable solution.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究动机**：实现一种无需被试专属训练、可直接用于新被试的脑视觉解码（Subject-agnostic brain decoding），即从 fMRI 信号中重建连续的视觉体验。此类技术具有重要临床潜力（如帮助瘫痪或意识障碍患者恢复视觉感知）。
- **核心难点**：现有方法面临两大障碍——
  - **跨被试泛化难**：不同被试的脑信号存在显著个体差异；
  - **脑信号本身复杂**：时空分辨率有限且噪声较高。
- **整体含义**：论文试图引入**认知结构先验**（人类视觉系统的腹侧–背侧通路结构）来约束和引导解码模型，从而提升跨被试视觉重建的能力，为脑视觉解码提供一个可迁移的“类脑”建模范式。

### 2. 论文提出的方法论

- **核心思想**：提出 **Visual Cortex Flow Architecture (VCFlow)**，一种分层式解码架构，将神经网络的内部结构显式地与大脑视觉皮层的组织方式对齐。
- **关键技术细节**：
  - 显式建模人类视觉系统的**腹侧–背侧通路**（ventral–dorsal architecture）；
  - **解耦并利用不同层级特征**：分别处理并提取 **早期视觉皮层（early visual cortex）**、**腹侧流（ventral stream）** 和 **背侧流（dorsal stream）** 的信息；
  - 腹侧流主要负责物体形状/语义识别，背侧流主要负责空间/运动信息处理，二者互补，有助于重建更完整的视觉内容；
  - 引入**特征级对比学习策略**（feature-level contrastive learning），用于增强**被试不变语义表征**（subject-invariant semantic representations），使模型在未见过的被试上也能泛化。
- **流程概述（文字版）**：
  1. 输入：某被试的 fMRI 脑活动数据；
  2. 分别提取早期视觉皮层、腹侧流区域、背侧流区域的信号特征；
  3. 对各分支特征进行解耦表示，保留各自特有的认知信息；
  4. 通过对比学习在不同被试之间对齐语义表征，削弱个体差异；
  5. 将多路互补特征融合并送入生成解码器，重建视觉视频/图像。
- 注：摘要未提供具体公式或数学表达，属于架构级描述。

### 3. 实验设计

- **数据集 / 场景**：论文实验场景为 **fMRI 连续视觉体验重建**，且要求**跨被试、无重训**地应用于新个体。
- **Benchmark**：由于摘要内容有限，未明确具体数据集名称（如常见的 GOD、NSD 等）、任务细节和评估指标的具体名称。
- **对比方法**：摘要仅提及 “不同于传统流程”，并称传统方法需要超过 12 小时的每个被试专属数据且计算量大。但未列出具体对比基线（如哪些 SOTA 解码模型）。
- **已知结论性数据**：
  - VCFlow 相比常规方法平均仅损失 7% 的准确率；
  - 无需任何重训练即可为新被试重建视频，每段 10 秒完成。

### 4. 资源与算力

- **文中有明确信息**：传统方法需要 **多于 12 小时/被试的数据采集** 和 “heavy computation”。
- VCFlow 的推理成本较低：每段重建视频约需 **10 秒**。
- **未明确信息**：论文摘要未给出模型训练的 GPU 型号、数量、显存、总训练时长等具体算力描述；也未提供参数量或 FLOPs。因此，无法评估其训练端的总计算开销。

### 5. 实验数量与充分性

- **可判断的实验数**：从摘要看，至少包含了一组跨被试视觉重建实验（推测有）：被试无关测试 + 对比传统流程，展示了准确率损失和耗时改善。
- **缺失的细节**：
  - 未报告具体数据集数量、被试人数、重建视频个数；
  - 未展示消融实验（如去掉背侧流/腹侧流、去掉对比学习等）；
  - 未展示与多个前沿方法的完整比较；
  - 未给出定量重建质量指标（如结构相似度、语义准确率、相关性等）的完整结果。
- **充分性判断**：由于呈现给审稿人的只包含摘要，具体实验设计暂不可见；从现有信息判断，论文声称的是“架构创新 + 一定性能损耗换跨被试泛化与计算效率”，但实验完整性和公平性需要看全文才能确认—— 摘要中仅一个“7% 准确率”数据，不足以全面证明优势。

### 6. 论文的主要结论与发现

- 将视觉皮层腹侧–背侧通路结构显式注入神经网络，能作为**强归纳偏置**提升跨被试脑解码的语义一致性。
- 通过**解耦早期视觉皮层、腹侧流和背侧流特征**，可提取到互补的认知信息，从而更好重建视觉内容。
- 通过**对比学习增强被试不变的语义表征**，可使模型适应未见过的被试，而无需重训练。
- 总体权衡：VCFlow 以**平均约 7% 的重建准确率下降**，换取了**免重训练的跨被试适用性**和仅 10 秒/视频的快速重建能力，被认为具有临床可扩展性。

### 7. 优点

- **认知先验创新**：引入视觉系统的双层流结构（ ventral-dorsal）作为架构设计基础，比单纯数据驱动方法更具可解释性，也更容易对齐已知的神经科学知识。
- **层级解耦设计**：显式分离早期视觉皮层、腹侧、背侧三条路径，功能区分清晰，有助于保留不同认知属性。
- **跨被试泛化导向**：目标明确针对临床应用中的无被试训练场景，与脑机接口的实用需求契合。
- **计算效率可观**：无需逐被试重新训练，每段视频重建仅需 10 秒，说明推理过程轻量、资源友好。
- **性能–效率折中可控**：以较小平均精度损失（7%）换取泛化能力，是允许的代价。

### 8. 不足与局限

- **定量证据不足**：摘要中只提到“7% accuracy loss”，未提供完整的定量评估指标（如视频重建的 SSIM、PSNR、语义相关性）以及跨被试泛化的统计显著检验。
- **数据集与规模未公开**：没有说明具体数据集、被试数量、类别分布等，实验可重复性无从判断。
- **消融实验缺失**：未见对腹侧/背侧流分离价值、对比学习贡献等方面的逐项消融，难以判断各组件因果作用。
- **底模型约束隐性限制**：跨被试效果可能仍依赖训练集被试多样性，若训练集合覆盖不足，未见被试上的泛化未必稳定。
- **“准确率”定义模糊**：对于视觉重建任务，准确率一词不常见，可能指语义分类准确率或像素级相似度，但摘要未明确定义。
- **临床应用差距**：对真实临床场景中的噪声、病变脑信号会如何表现，仍需进一步验证。
- **算力信息不完整**：训练端资源消耗（GPU、时间、能效）未披露，限制了对其总资源开销的评估。

（完）
