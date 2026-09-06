---
title: Learning Interpretable Representations Leads to Semantically Faithful EEG-to-Text Generation
title_zh: 学习可解释表征带来语义忠实的脑电到文本生成
authors: "Xiaozhao Liu, Dinggang Shen, Xihui Liu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=mMhlqK79Nm"
tags: ["query:eeg-align"]
score: 9.0
evidence: 将EEG到文本的任务重构为语义摘要，通过学习可解释表征缓解幻觉，属于脑电特征与文本语义对齐的直接工作
tldr: 脑电到文本生成面临幻觉问题，根源往往在于脑电和文本信息容量不匹配，模型即使没有真实语义激活也能生成看似合理的句子。该文以后验坍塌为切入点，把EEG到文本重构为语义摘要任务，提出学习可解释的潜在表征并配合生成语言模型来生成核心语义而非逐词还原。这样可减少由强先验生成模型主导的幻觉，使输出更忠实于脑活动中的语义激活。该工作为低容量脑电信号与高容量语言模型之间的语义对齐提供了可靠策略。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 脑电到文本生成常因后验坍塌产生幻觉，输出未必反映真实脑内语义激活。
method: 将EEG文本解码更名为语义摘要，利用可解释潜在表示与生成语言模型抑制逐词重构。
result: 所提方法在语义保真度上相较逐词脑电文本重构更有优势，可减少幻觉。
conclusion: 学习可解释潜在语义表征是提升EEG到文本生成可靠性的有效方向。
---

## Abstract
Pretrained generative models have opened new frontiers in brain decoding by enabling the synthesis of realistic texts and images from non-invasive brain recordings. However, the reliability of such outputs remains questionable—whether they truly reflect semantic activation in the brain, or are merely hallucinated by the powerful generative models. In this paper, we focus on EEG-to-text decoding and address its hallucination issue through the lens of posterior collapse. Acknowledging the underlying mismatch in information capacity between EEG and text, we reframe the decoding task as semantic summarization of core meanings rather than previously verbatim reconstruction of stimulus texts. To this end, we propose the Generative Language Inspection Model (GLIM), which emphasizes learning informative and interpretable EEG representations to improve semantic grounding under heterogeneous and small-scale data conditions. Experiments on the public ZuCo dataset demonstrate that GLIM consistently generates fluent, EEG-grounded sentences without teacher forcing. More importantly, it supports more robust evaluation beyond text similarity, through EEG-text retrieval and zero-shot semantic classification across sentiment categories, relation types, and corpus topics. Together, our architecture and evaluation protocols lay the foundation for reliable and scalable benchmarking in generative brain decoding.

---

## 论文详细总结（自动生成）

## 论文总结：Learning Interpretable Representations Leads to Semantically Faithful EEG-to-Text Generation

> **说明**：所提供的材料仅包含论文的标题、摘要、作者和少量元数据，不包含完整正文，因此本总结主要依据这些可见信息展开；部分技术细节、实验数字和训练资源在原文中未能获得，将明确标注为“未在材料中说明”。

### 1. 核心问题与整体含义

- **研究背景**：预训练生成模型近年来被用于“脑解码”，可以从非侵入性脑电（EEG）或功能性磁共振等记录中合成较为自然的文本或图像。这种能力带来了新的前沿方向，但也引发可靠性疑问。
- **核心问题**：EEG 到文本（EEG-to-text）生成中，模型生成的句子可能并未真正反映被试大脑中的语义活动，而只是被强大的生成语言模型“脑补”出来的幻觉（hallucination）。
- **问题根源**：作者从“后验坍塌”（posterior collapse）的视角解释该现象。EEG 与文本之间存在明显的信息容量不匹配：文本包含细粒度、逐词的语义和语法信息，而 EEG 是低信噪比、高异构性的神经信号。若强制将 EEG 映射到逐词文本，模型容易“忽略”EEG 输入，直接依赖语言模型的先验生成看似合理的句子。
- **整体含义**：作者主张将 EEG 解码任务从“逐词还原刺激文本”重构为“核心语义摘要”，从而避免模型过度用语言先验掩盖真实脑语义激活。这种重构更符合 EEG 的信息容量，也更利于衡量解码的语义保真度。

### 2. 提出的方法论

- **核心思想**：不再让 EEG 生成与原始刺激文本逐词一致的句子，而是让模型生成能够反映大脑被激活语义的“摘要”；同时强调学习“信息充分且可解释”的 EEG 表征，把 EEG 特征与生成语言模型对接，增强输出对脑信号的语义依赖。
- **提出模型**：作者提出 GLIM（Generative Language Inspection Model，生成式语言检查模型）。摘要中说明：
  - 强调学习可解释的 EEG 表征；
  - 面向异构、小规模 EEG 数据；
  - 生成过程不使用 teacher forcing，也能产生流畅、有 EEG 语义依据的句子；
  - 借助生成语言模型完成从 EEG 语义表征到文本摘要的生成。
- **具体技术细节**：由于材料中未提供正文，以下内容缺失：
  - 网络总体结构（是否编码器—解码器、是否使用对比学习、是否引入 VAE 或离散瓶颈等）；
  - 详细的损失函数与训练策略；
  - 如何处理“后验坍塌”（如 KL 项权重、梯度截断、表征约束等）；
  - EEG 特征如何进行时间/通道/频段聚合并映射到文本空间。
- **大体算法流程**：从摘要可以推知，流程可能是：采集 EEG → 提取可解释的语义相关特征 → 将特征送入生成语言模型 → 输出以语义摘要为目标的文本；训练时可能引入检索或分类辅助目标，使 EEG 表征能够与语义空间对齐。

### 3. 实验设计

- **数据集**：公开的 ZuCo 数据集。ZuCo 通常包含被试在自然阅读任务中的 EEG 和眼动记录，是 EEG-to-text 领域常用的基准数据集。
- **任务设定**：将 EEG 解码定义为“语义摘要”，即在输入与刺激文本相关的 EEG 后，模型应生成该文本的核心语义，而非逐词复述。
- **评测场景**：
  - 文本生成质量：评估生成的句子是否流畅；
  - EEG-文本检索（EEG-text retrieval）：检验 EEG 特征与文本语义之间是否形成可靠对齐；
  - 零样本语义分类：在情感类别、关系类型、语料主题等多个语义维度上进行零样本分类，验证模型学到的是否是泛化的语义表征。
- **对比方法**：材料中未列出与哪些基线方法进行比较（如逐词生成的典型 EEG-to-text 模型、基于 teacher forcing 的解码器、纯语言模型先验等）。
- **Benchmark 说明**：摘要提到该工作提出的评估方式比单纯文本相似度更稳健，但未给出具体得分、效果表或与现有基准的量化对比。

### 4. 资源与算力

- 论文摘要和元数据中均未提及使用多少块 GPU（如 NVIDIA A100/V100）、数量、训练时长、参数规模等具体算力信息。
- 因此，关于算力资源的部分只能判断为：**原文材料未作说明**。

### 5. 实验数量与充分性

- 就可见材料而言，实验设置包括：
  - 在一个公开数据集（ZuCo）上的生成实验；
  - 三类语义化评测：检索、零样本分类（情感/关系/主题）；
  - 可能还有文本相似度之外的对比评价。
- 然而，由于缺少正文，无法获知：
  - 是否进行了多数据集交叉验证；
  - 是否做了被试内/被试外划分；
  - 是否包含详细的消融实验（如去掉可解释表征、改为逐词目标、替换语言模型等）；
  - 是否与已有最先进方法在相同条件下公平比较；
  - 具体统计检验与误差棒。
- **总体判断**：从评估维度看，设计有一定覆盖面，特别是引入零样本语义分类和 EEG-文本检索，显示出对“语义忠实度”的关注；但在数据覆盖面、基线数量和消融实验可见性上并不充分。目前只能认为实验方向有意义，暂时不能判断其完备性与公平性。

### 6. 主要结论与发现

- 作者提出：将 EEG 到文本视为语义摘要任务、并学习可解释潜在表征，能够缓解生成过程对语言先验的过度依赖。
- GLIM 在 ZuCo 数据集上可以生成流畅、且与 EEG 语义活动一致（即“EEG-grounded”）的句子，并且**不需要 teacher forcing**，说明模型不只是在机械复述训练语料。
- 除了文本相似度，通过 EEG-文本检索和零样本语义分类进一步证明，模型提取出的 EEG 表征确实携带了情感、关系、主题等高层语义信息。
- 因此，论文认为“学习可解释的潜在语义表征”是提升 EEG-to-text 生成可靠性的有效路径，也为生成式脑解码提供了更稳健的评估基准思路。

### 7. 优点

- **问题切入角度较好**：以“后验坍塌”解释 EEG-to-text 的幻觉，不是简单把幻觉归因于数据少或模型弱，而是指出任务建模和训练目标中的结构性失真。
- **重构任务定义**：将逐词生成改为语义摘要，承认 EEG 与文本存在信息容量不对称，更符合脑信号的语义特性。
- **强调可解释性**：从表征层面追求语义对齐，而不是只优化句子表面相似度，有助于理解神经元信号中到底“解码出了什么”。
- **评估设计有亮点**：提出超越文本相似度的评测，包括 EEG-文本检索和零样本语义分类，试图直接检验 EEG 表征与语义空间的对齐，思路较新颖。
- **使用公开数据集**：便于后续复现与横向比较。

### 8. 不足与局限

- **可见信息受限**：由于材料中缺失正文，无法评估模型架构的独特性、理论推导、训练稳定性和真正创新点。
- **实验范围较窄**：仅在 ZuCo 一个公开数据集上验证；EEG 个体差异大、噪声高，若缺乏跨数据集或跨任务验证，结论的普适性有限。
- **“语义摘要”的目标建模不够清晰**：如何定义“核心语义摘要”？与原始刺激文本和传统文本摘要有何异同？是否存在人工标注的摘要参考？这些问题在摘要中没有交代。
- **幻觉指标间接**：虽然增加了检索和零样本分类，但这些指标仍属于间接指标，未直接证明“生成文本中每个关键语义都能追溯到对应 EEG 激活”。
- **对比基线和消融不足**：从可见材料来看，未提及与逐词解码模型、不同 EEG 编码器、不同语言模型规模等关键变量的对比；这会限制结论的说服力。
- **应用限制**：EEG 采集设备昂贵、佩戴不便，且个体/跨时段差异大，离实际可用的“脑控文本生成系统”仍有距离。

（完）
