---
title: A Unified Framework for EEG–Video Emotion Recognition with Brain Anatomy Guidance
title_zh: 脑解剖引导的EEG-视频情绪识别统一框架
authors: "JangHyun Kim, Seongro Yoon, Temo Saghinadze, Aowen Shi, Mingyun Jeong, Donghyeon Cho, Jinsun Park, Francois Bremond"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=hga7TjP3uD"
tags: ["query:eeg-align"]
score: 9.0
evidence: 结合脑电神经动态与视频行为线索，以脑解剖启发的图卷积实现多模态情绪识别
tldr: 现有视频-EEG多模态情绪识别研究不足。EVER提出脑解剖感知的模态间层级图卷积网络（BIH-GCN），先把EEG通道特征聚合成脑区级表示，再与视频行为线索融合。该框架同时利用可观测行为与内部神经动态进行互补表征，能更全面稳健地刻画人类情绪，是脑电与行为模态多模态学习的典型工作。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 视频-EEG多模态情绪识别中，观测行为线索与神经动态的互补融合仍未得到充分探索，缺乏统一有效框架。
method: 提出EVER框架，用脑解剖感知的BIH-GCN将EEG通道聚合为脑区级表示，再与视频特征跨模态融合。
result: 该框架可整合EEG与视频模态互补信息，为情绪识别提供更全面稳健的多模态表征。
conclusion: 说明从脑解剖结构出发设计层间融合能有效提升EEG-视觉行为多模态识别性能。
---

## Abstract
Recent studies in video- and EEG-based emotion recognition have shown notable progress. However, multi-modal emotion recognition remains largely unexplored, particularly the integration of physiological signals with video. This integration is crucial, as EEG–video fusion combines observable behavioral cues with internal neural dynamics and enables a more comprehensive and robust characterization of human emotion. To this end, we propose EVER, a novel EEG–Video Emotion Recognition framework that effectively integrates complementary information from both modalities. Specifically, EVER employs a Brain anatomy-aware Inter-modal Hierarchical Graph Convolution Network (BIH-GCN), which aggregates EEG channel features into region-level representations guided by anatomical priors. These region-level features are combined with global EEG and video embeddings to form a unified representation for emotion classification. Furthermore, we introduce a correlation-based distribution alignment loss to reconcile modality-specific embeddings and reduce cross-modal discrepancies. To provide a comprehensive evaluation, we conduct comprehensive benchmark across three public EEG-video paired datasets---Emognition, MDMER, and EAV. We evaluate 12 representative models, consisting of 5 EEG-only, 5 video-only, and 2 audio-video models, and report their performance under EEG, video, and EEG–video settings. Our benchmark highlights the strengths and limitations of both unimodal and multi-modal approaches across diverse environments. Extensive experiments demonstrate that the proposed EVER achieves state-of-the-art performance by jointly modeling behavioral cues from video and physiological responses from EEG, thereby enabling the recognition of emotional patterns unattainable by either modality alone.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 论文的核心问题与整体含义

- 情绪识别研究中，基于视频的视觉行为识别与基于 EEG 的神经信号识别各自发展较快，已取得明显进展。
- 然而，**多模态情绪识别**——特别是将 EEG 生理信号与视频行为线索结合——仍未被充分探索。
- 作者认为这一融合至关重要，因为视频与 EEG 各自捕捉情绪的不同侧面：视频提供**可观测的外部行为线索**，EEG 反映**内部的神经动态**；二者结合有望实现对人类情绪更全面、更稳健的表征。
- 为此，论文提出统一框架 **EVER（EEG–Video Emotion Recognition）**，目标是在同一模型中有机整合两类互补信息，实现超越单一模态的情绪识别性能。

### 2. 论文提出的方法论

- **核心思路**：以脑解剖学知识为引导，设计层级图卷积结构，先刻画 EEG 通道间的空间关系，再与视频特征做跨模态融合，形成统一表示用于情绪分类。
- 核心模块是 **BIH-GCN**（Brain anatomy-aware Inter-modal Hierarchical Graph Convolution Network，即“脑解剖感知的模态间层级图卷积网络”），其关键技术细节包括：
  - **脑区级聚合**：基于脑解剖先验，将 EEG 通道特征聚合为脑区级表示，而不是直接在通道级做全图卷积；这种聚合更符合大脑的功能组织方式。
  - **层级建模**：在通道级与脑区级之间建立层次结构，使网络能够同时利用局部通道信息与区域级语义信息。
  - **模态融合**：将脑区级特征与全局 EEG 特征、视频全局嵌入结合，形成统一的多模态表示，用以进行情绪分类。
- 为缓解模态间差异，作者引入**基于相关性的分布对齐损失（correlation-based distribution alignment loss）**，用于调和 EEG 与视频的特征分布，降低跨模态差异。
- 总训练目标可理解为：多模态分类损失 + 跨模态分布对齐损失的联合优化。

> 由于论文正文被 OpenReview 拦截页遮蔽，未能提取到具体损失函数公式、图卷积邻接矩阵定义等细节。

### 3. 实验设计

- **评测数据集**：共 3 个公开的 EEG–视频配对数据集：
  1. **Emognition**
  2. **MDMER**
  3. **EAV**
- **Benchmark 范围**：论文建立了较为全面的 benchmark，覆盖三种输入配置：
  - EEG-only
  - Video-only
  - EEG–Video（多模态）
- **对比方法**：共评估 12 个代表性模型，包括：
  - 5 个 EEG-only 模型
  - 5 个 Video-only 模型
  - 2 个 Audio-Video（音视频）模型
- 多数据集、多模态配置、多基线模型的设计使得比较结果的参考价值较高。

### 4. 资源与算力

- 在可获得的论文文本（摘要与元数据）中，**未提及**使用的 GPU 型号、数量、训练时长或任何计算资源说明。
- 由于 PDF 正文内容被拦截，也可能正文中确有算力说明但当前无法获取。需要指出：**该信息在现有文本中缺失，无法总结**。

### 5. 实验数量与充分性

- 从现有信息看，实验覆盖了：
  - 3 个真实世界 EEG-视频数据集；
  - 12 个代表性对比模型；
  - 3 种输入模态设置；
  - 加上 EXTREMELY 自身的 SOTA 报告。
- 因此实验范围在数据集数量与基线丰富度上较为充分。
- 但需注意：摘要中没有给出消融实验、超参分析、特征可视化等细节；由于正文无法访问，不能确认是否包含这些实验。
- 公平性方面，论文报告了单模态与多模态设置下的统一 benchmark，有利于客观对比；但论文由作者自己建立 benchmark 并报告自己方法达到 SOTA，仍需在获得可复现代码或更细实验表格后才能完全确认公正性。

### 6. 论文的主要结论与发现

- 提出的 EVER 框架通过联合建模视频中的行为线索与 EEG 中的生理响应，**在三个数据集上取得了当前最优（state-of-the-art）性能**。
- 结果表明，EEG 与视频多模态融合能识别出单模态难以捕捉的情绪模式。
- 验证了脑结构启发的层级图卷积与跨模态分布对齐在多模态情绪识别中的有效性。

### 7. 优点

- **多模态互补视角清晰**：将外部行为信号与内部神经信号结合，动机自然，研究空白定位明确。
- **方法设计有脑科学依据**：基于脑解剖先验将 EEG 通道聚合为脑区特征，较直接对全脑通道使用图卷积更具可解释性与结构合理性。
- **统一多模态基准**：同时提供 EEG-only、Video-only 与多模态性能对比，考察模态互补增益与饱和效应。
- **数据集覆盖面广**：在 3 个独立、不同环境下采集的 EEG-视频配对数据上验证，有助于证明方法的一般性。
- **对齐损失引入合理**：直接针对多模态学习中的“模态差异”问题设计辅助训练信号。

### 8. 不足与局限

- **正文信息不可得**：OpenReview 页面存在 CAPTCHA/访问限制，完整算法细节、公式、实验表格难以核对，难以深入评估其具体实现、消融设计与统计显著性。
- **摘要未报告消融细节**：未知 BIH-GCN 各组件（脑区聚合、层级图卷积、分布对齐损失）的独立贡献。
- **缺乏对失败场景的讨论**：未见对不同情绪类别或困难样本的分类表现分析。
- **数据方面**：三个数据集是否存在环境迁移问题；EEG 设备类型不同等情况下的鲁棒性需进一步说明。
- **算力不可知**：未报告计算资源要求，影响该方法的工程复现与实用评估。
- **基线范围仍有限**：12 个基线以单模态为主，对比的多模态（EEG+video）方法数量偏少，未来还需与更多多模态方法比较。

> 说明：以上总结仅基于论文标题、作者、摘要和元数据编写；完整的公式、表格、实验细节需通过获得完整 PDF 正文后补充。

（完）
