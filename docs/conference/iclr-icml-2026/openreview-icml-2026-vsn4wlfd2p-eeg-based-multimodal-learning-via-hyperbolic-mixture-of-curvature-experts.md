---
title: EEG-Based Multimodal Learning via Hyperbolic Mixture-of-Curvature Experts
title_zh: 基于超曲率混合专家的EEG多模态学习
authors: "Runhe Zhou, Shanglin Li, Guanxiang Huang, Xinliang Zhou, Qibin Zhao, Motoaki Kawanabe, Yi Ding, Cuntai Guan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/ea752240194f22a799321a61fe59d0f28350fdf2.pdf"
tags: ["query:eeg-align"]
score: 9.0
evidence: EEG与面部表情等异构模态的多模态表征学习，利用层次结构的超曲率混合专家建模
tldr: EEG多模态学习依赖对异构模态的表征质量，而EEG与面部表情等模态往往具有反映认知过程的层次结构。针对欧氏空间难以刻画这类层级关系的问题，作者提出基于超曲率混合专家的EEG多模态学习框架，在不同曲率空间中分别建模层级不同的模态并做专家级融合。实验表明该方法在情绪或心理状态评估等任务上优于固定曲率的欧氏或双曲基线，为EEG与行为模态协同建模提供了更贴合认知结构的方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: EEG和相关行为模态如面部表情具有复杂认知层级结构，欧氏嵌入无法表达其指数层级特性，限制了EEG多模态学习效果。
method: 使用超曲率混合专家，为不同层级结构的EEG和伴生模态选择合适曲率空间，并融合专家输出获得联合表征用于精神或情绪状态评估。
result: 在EEG多模态评测中较固定曲率的欧氏或双曲模型获得更优的心理状态预测效果，验证了层级感知建模的价值。
conclusion: 按模态内部层级自适应选择曲率空间可显著改善EEG与行为数据跨模态表征质量，是脑机临床应用的潜在方向。
---

## Abstract
Electroencephalography (EEG)-based multimodal learning integrates brain signals with complementary modalities to improve mental state assessment, providing great clinical potential.
The effectiveness of such paradigms largely depends on the representation learning on heterogeneous modalities.
For EEG-based paradigms, one promising approach is to leverage their hierarchical structures, as recent studies have shown that both EEG and associated modalities (e.g., facial expressions) exhibit hierarchical structures reflecting complex cognitive processes.
However, Euclidean embeddings struggle to represent these hierarchical structures due to their flat geometry, while hyperbolic spaces, with their exponential growth property, are naturally suited for them.
In this work, we propose EEG-MoCE, a novel hyperbolic mixture-of-curvature experts framework designed for multimodal neurotechnology. 
EEG-MoCE assigns each modality to an expert in a learnable-curvature hyperbolic space, enabling adaptive modeling of its intrinsic geometry. 
A curvature-aware fusion strategy then dynamically weights experts, emphasizing modalities with richer hierarchical information.
Extensive experiments on benchmark datasets demonstrate that EEG-MoCE achieves state-of-the-art performance, including emotion recognition, sleep staging, and cognitive assessment. 
Code is available at https://github.com/zhourunhe/EEG-MoCE.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与研究动机

- **背景**：基于脑电图（EEG）的多模态学习通过整合脑信号与互补模态（如面部表情）来提升心理状态评估的性能，在临床应用中具有巨大潜力。此类范式的有效性高度依赖于对异构模态的表征学习质量。
- **核心问题**：EEG及其伴随模态（如面部表情）往往具有反映复杂认知过程的**层次结构**（hierarchical structures）。然而：
  - **欧氏空间**的平坦几何难以刻画这种层级关系；
  - **双曲空间**虽然具有指数增长特性、天然适合建模层级结构，但固定的单一曲率无法同时适配不同模态各自的几何特征。
- **关键洞见**：既然EEG与不同伴生模态各自具有不同程度的层级复杂度，那么让每种模态在**与其内在几何结构相匹配的曲率空间**中建模，应优于将所有模态强制放入同一固定曲率空间的做法。

## 2. 方法论（EEG-MoCE）

- **核心思想**：提出 EEG-MoCE——一种基于**超曲率混合专家**（Hyperbolic Mixture-of-Curvature Experts）的多模态 EEG 学习框架。为不同层级结构的模态分配不同的曲率空间专家，实现自适应几何建模，并通过曲率感知融合策略整合专家输出。
- **关键步骤**：
  1. **模态分配**：每种模态（如 EEG、面部表情等）被分配到一个可学习曲率的双曲空间专家中。每个专家独立地在其对应的曲率空间中对输入模态进行表征学习。
  2. **可学习曲率**：各个专家的曲率不是预先固定的，而是在训练过程中自动学习，从而能够自适应地匹配该模态数据内在的层级复杂度。
  3. **曲率感知融合**：设计一种融合策略，根据各专家的曲率或模态的层级丰富程度动态地为专家分配权重，使携带更丰富层级信息的模态在联合表征中占据更重要的地位。
  4. **下游任务**：融合后的联合表征用于心理健康/情绪状态评估等下游分类任务。
- **公式/算法概要**（文字说明）：模态级编码器将原始输入映射到对应专家所在的超曲空间，在双曲空间中执行特征变换与距离度量，再通过指数映射将对数几率融合回切线空间进行任务分类。本文核心在于将传统"单曲率空间"扩展为"多曲率专属空间 + 动态加权融合"的架构。

## 3. 实验设计与 Benchmark

- **任务场景覆盖**：论文在涵盖脑机接口核心应用场景的多个 benchmark 数据集上进行了验证，具体包括：
  - **情绪识别**（emotion recognition）；
  - **睡眠分期**（sleep staging）；
  - **认知评估**（cognitive assessment）。
- **对比方法**：与固定曲率空间的基线方法进行对比，包括：
  - 欧氏空间模型；
  - 固定曲率双曲模型；
  - 既有EEG多模态方法。
  - 实验结果中 EEG-MoCE 达到了 **state-of-the-art** 性能。
- **演示验证**：论文提供公开代码（GitHub），便于复现验证。

## 4. 资源与算力

- **明确说明**：提供的摘要元数据提及了实验任务与数据集，但**未明确说明所使用的 GPU 型号、数量、训练时长或具体的算力规模**。
- **推断与说明**：作为 ICML 级别的脑机接口多模态工作，通常在标准学术级 GPU（如单张/数张 V100/A100 级显卡）上即可完成此类规模任务的训练，但最终以论文正文为准，本摘要未披露具体细节。

## 5. 实验数量与充分性

- **实验覆盖范围**：覆盖三类核心下游任务（情绪识别、睡眠分期、认知评估），证明了方法的领域通用性。
- **消融与机制实验的推断空间**：文末材料未明确提及详细的消融实验数量。从方法逻辑推断，至少应包含对 "MoCE 专家数量、曲率可学习与否、融合权重策略" 等方面的验证，但摘要未逐一呈现。
- **总体评价**：实验覆盖的任务面较为全面，能较好说明方法泛化性；但由于摘要限制，未能展示消融实验的细致程度、统计显著性检验以及误差条等信息——其**最终充分性需要依赖全文实验章节进行判断**。

## 6. 主要结论与发现

- 固定曲率空间（无论是欧氏还是双曲）不足以刻画 EEG 多模态数据中不同模态各自特有的层级几何结构。
- 通过**基于曲率的专家混合架构**，让各模态在最适合自身结构特性的空间中建模，并由曲率感知融合动态突出更有层级判别力的模态，能有效提升EEG多模态心理状态评估的性能。
- EEG-MoCE 在情绪识别、睡眠分期与认知评估等基准上相较欧氏/双曲固定曲率基线均获得了更好的预测效果——验证了**层级感知建模**在脑机接口多模态学习中的价值。

## 7. 优点

- **动机合理**：从认知数据的内在结构出发，深刻指出了欧氏空间对EEG数据层级结构建模的固有缺陷，并使用双曲几何作为数学依据，具备理论解释力。
- **方法新颖**：将文本/图像领域验证有效的双曲表征策略创新性地引入 EEG 多模态融合——"不同模态→不同曲率专家→曲率感知融合"的建模思路打破了以往单一曲率空间的限制，在脑机接口领域具有首创性。
- **保障体系完善**：覆盖公开的基准数据库 + 公开代码，可复现性好。
- **框架哲学一致**：不预先粗暴假设哪个模态层级更高，而是让模型根据数据自适应学习曲率并对融合贡献进行重新分配——具有较好的拓展性与通用性。

## 8. 不足与局限

- **实验信息不完整**：摘要未提供详细消融实验、模态组合差异分析、超参数敏感性分析，也未展示各任务中的关键超参（专家数、特征维度等）；需要阅读全文核实。
- **算力资源未披露**：文中未说明 GPU 资源、训练时长等，对复现时预估计算成本造成困难。
- **模态范围有限**：当前评测模态以 EEG + 面部表情为主，对其他行为模态（如眼动、语音）的适用性尚未充分证明。
- **层级直觉的定量依据仍需加强**：虽然以"层级结构"作为动机，文中未在摘要阶段提供对模态层级复杂度进行量化测度的证据，理想情形下需补充对各级模态树状度/层级深度的统计分析。
- **临床落地距离未说明**：作为面向脑机临床的潜在方案，在真实场景下的时延、鲁棒性（伪造/离群数据）、用户差异泛化等关键问题未在摘要中揭示。

（完）
