---
title: "EmBrace: A Collective Knowledge Fusion Framework Toward Unified EEG Foundation Models"
title_zh: EmBrace：面向统一EEG基础模型的集体知识融合框架
authors: "Ziyu Jia, Junyi Lin, Pu Wan, Jinxin Pi, Jingying Ma, Peiliang Gong, Xinliang Zhou, Yi Ding, Chenyu Liu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a1d66506a9ff2a7833c7a5d974c1a48b9a356393.pdf"
tags: ["query:eeg-align"]
score: 8.0
evidence: 对多个EEG基础模型的判别表征做样本级融合，以提升下游任务性能
tldr: 不同EEG基础模型在任务上各有优势且样本级表现互补，但逐一微调计算开销大。EmBrace提出以表征为中心的样本感知知识融合框架，不依赖参数级或输出级对齐，而是同步优化判别性表征并动态选择各模型优势。实验表明其能够在多种下游EEG任务上取得优于单模型或参数融合的效果，为EEG基础模型集成提供了轻量方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 单个EEG基础模型无法在所有下游任务取胜，全量微调所有模型则带来高计算成本。
method: 提出以表征为中心的样本感知知识融合，跳过参数与输出对齐，并使多个EEG基础模型的判别表征协同。
result: 在多个下游EEG任务上超越单一最优模型并显著降低模型选择与微调开销。
conclusion: 样本级表征融合是低成本利用多个EEG基础模型的有效策略。
---

## Abstract
Electroencephalography (EEG) foundation models (EFMs) have achieved strong performance across a wide range of downstream EEG tasks via pretraining and fine-tuning. Through empirical analysis, we observe that (i) no single EFM consistently dominates all tasks, yet identifying the task-specific optimal model by fine-tuning all EFMs introduces substantial computational overhead; and (ii) models with inferior task-level performance still exhibit strengths at the sample level as distinct architectures induce diverse inductive biases. These observations motivate EmBrace, a representation-centric framework for sample-aware knowledge fusion that avoids the constraints of parameter-level or output-level alignment. EmBrace synchronizes discriminative intermediate representations into a unified manifold and adaptively weights multiple EFMs at the sample level while selecting the most compatible model as the carrier. Extensive experiments across multiple EEG benchmarks demonstrate that EmBrace consistently improves over SOTA EFMs and generalizes effectively under cross-task settings.

---

## 论文详细总结（自动生成）

# EmBrace：面向统一EEG基础模型的集体知识融合框架——中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：脑电图（EEG）基础模型（EFMs）通过大规模预训练加下游微调，已在睡眠分期、癫痫检测等多种下游任务上取得较好效果。
- **核心问题**：在实际应用中，是否存在一个“全能”EFM可以在所有任务上都达到最优？论文通过实证观察得出了两个关键现象：
  - **任务级现象**：没有任何单一 EFM 能在所有任务上稳定胜出；但若想找到最适合某一任务的模型，逐一微调并评估所有 EFM 会带来很高的计算开销。
  - **样本级现象**：任务级表现较差的模型，在样本级上仍然可能“擅长”某些困难样本——不同模型架构带来不同的归纳偏置，使它们在不同样本上各有所长。
- **整体含义**：既然单个基础模型存在能力盲区，而全量微调多个模型又代价过高，则需要一种**低成本、灵活**的知识融合方式，使多个已有 EFM 的能力形成互补，从而逼近或超过“任务专属最优模型”的性能。

## 2. 论文提出的方法论：核心思想与关键技术细节
- **框架名称**：EmBrace（面向统一 EEG 基础模型的集体知识融合框架）。
- **核心思想**：以**表征为中心（representation-centric）**、**样本感知（sample-aware）** 的方式融合多个 EFM 的知识。
  - 该方法被设计为**绕开两类传统融合约束**：
    - **参数级对齐**（例如模型参数融合、权重平均等），此类方法通常要求模型结构或参数空间具有可比性；
    - **输出级对齐**（例如 logit 集成、投票等），此类方法往往只利用最终预测而丢失中间表征信息。
- **技术流程（文字说明）**：
  1. **提取表征**：分别从多个预训练 EFM 中提取中间层的判别性表征；
  2. **统一流形对齐**：将这些来自不同架构的中间表征**同步映射到一个统一流形**上，使各模型的特征空间具有可比性；
  3. **动态样本级加权**：在统一表征空间中，通过某种注意力/加权机制，针对**每一个输入样本**动态判断哪个模型贡献最大，并据此组合各个模型的判别信息；
  4. **载体模型选择（Carrier Selection）**：同时选择某一“最兼容”的 EFM 作为主载体，用于承载后续分类头或任务输出，以降低结构不一致带来的适配难度。
- **算法流程归纳**：多模型并行前向 → 中间表征抽取 → 统一流形同步 → 样本级动态权重分配 → 载体模型输出。论文未在摘要中给出具体的损失函数或数学公式细节。

## 3. 实验设计：数据集、基准与对比方法
- **实验总体规模**：论文提及进行了“广泛的实验”，覆盖 **多个 EEG 基准数据集（multiple EEG benchmarks）**，并包含**跨任务（cross-task）**迁移/泛化设置。
- **benchmark 构成**：至少包括多种下游任务场景（如分类任务）以及跨任务的模型泛化测评；具体的 EEG 数据集名称、通道数、受试者数量、任务类型（如睡眠分期、癫痫检测、运动想象等）在摘要中未能完整呈现。
- **对比方法**：
  - 与 **SOTA EFMs**（当前最优的单模型 EEG 基础模型）进行比较；
  - 对比对象应包含单模型微调、可能的参数级/输出级融合基线等，但具体基线列表需查看全文才可确认。

## 4. 资源与算力
- 摘要和论文元数据中**没有明确提及**实验所使用的 GPU 型号、GPU 数量、训练时长、显存占用等算力信息。
- 论文提出的动机之一是“降低逐一微调所有 EFM 的计算开销”，因此在推理成本上具有轻量优势；但具体能耗与算力对比数据在现有文本中未给出。

## 5. 实验数量与充分性
- **实验数量**：从摘要可见，作者在多个 EEG 基准上进行了系统评估，并单独设计了跨任务泛化实验，说明实验覆盖不止一个数据集/任务。
- **充分性与公平性评价（基于现有文本）**：
  - 摘要声称“一致优于 SOTA EFMs”（consistently improves over SOTA EFMs），这是较强的结论；
  - 但是，由于当前提取内容未包含具体数据集名称、任务清单、基线细节、统计显著性检验，**无法对实验公平性做最终裁定**；
  - 目前文本中没有看到明确的消融实验描述，例如“是否验证统一流形映射的必要性”“动态样本级加权相比固定融合的增益”等；若全文包含此类消融，则证据链会更完整。

## 6. 论文的主要结论与发现
- **核心结论**：在多个下游 EEG 任务上，EmBrace 能够胜过单一最优 EFM，并表现出良好的跨任务泛化能力。
- **机制性发现**：不同 EFM 在任务级上存在性能差异，但在样本级上具有互补性——这为“集体知识融合”提供了实证基础。
- **方法论结论**：绕过参数级与输出级对齐、直接在表征层面进行样本感知融合，是一种有效且低成本的利用多个 EEG 基础模型的策略。
- **实际价值**：为“在算力有限条件下使用多基础模型”的场景（如脑机接口、临床辅助诊断）提供了可落地的轻量方案。

## 7. 优点
- **问题切入点务实**：同时抓住了任务级“模型选择困难”和样本级“能力互补”两个关键观察，动机清晰、有实证支撑。
- **方法论设计有新意**：采用表征中心范式，避免了对多模型参数空间不可比问题的纠缠，具有较好的通用性。
- **动态细粒度融合**：从“整体任务选模型”走向“单个样本动态配权”，更贴合脑电信号高异质性、样本间差异大的特点。
- **低成本取向明确**：相比逐个微调全部 EFM，该方法有望显著压缩调参开销，契合实际工程部署中对算力的约束。
- **泛化性得到初步验证**：论文特别提到跨任务设置下的有效性，增加了方法的可信度与推广潜力。

## 8. 不足与局限
- **信息透明性不足（就当前摘要而言）**：未列出具体数据集、任务定义、基线与实现细节，难以完整复现或横向对比。
- **载体模型选择机制存在潜在开销**：在推理时仍需对所有待融合模型进行一次前向计算，因此虽免去了多模型微调的高昂训练成本，但**内存占用和推理延迟依然是多模型系统的固有代价**。若载体选择本身也需要校验集，则还会引入额外的数据依赖。
- **可解释性有限**：动态样本级加权机制在“为什么某个样本应信任某模型”上缺乏可解释性，而临床 EEG 场景往往对决策依据有较高要求。
- **仅在跨任务能力上做泛化验证**，尚未看到跨数据集、跨噪声环境、跨受试者群体（跨域）等更丰富泛化测试的证据。
- **未报告计算资源与运行时间**，不利于判断其“轻量”优势到底体现在训练阶段、推理阶段，还是二者兼有。

（完）
