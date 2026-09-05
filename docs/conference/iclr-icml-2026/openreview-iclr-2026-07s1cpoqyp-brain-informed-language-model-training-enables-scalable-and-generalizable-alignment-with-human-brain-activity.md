---
title: Brain-Informed Language Model Training Enables Scalable and Generalizable Alignment with Human Brain Activity
title_zh: 脑信息引导的语言模型训练实现与大脑活动的可扩展且可泛化对齐
authors: "Isil Poyraz Bilgin, Marie St-Laurent, Lune P Bellec, Leila Wehbe"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=07S1CPoQYP"
tags: ["query:abstraction"]
score: 8.0
evidence: 利用fMRI引导语言模型训练并与脑活动对齐，直接支撑大模型与人脑的结构对应研究
tldr: 针对如何将脑记录主动用于引导语言模型表征的问题，本文使用50多小时fMRI数据为预训练或随机初始化语言模型增加脑对齐模块并比较多种训练策略。结果显示脑信息微调能持续提升语言表示与自然观影脑动态的对齐，并可泛化到留出影片。研究证明神经反馈可直接引导模型语义表示，为建立类脑与语言模型概念对应关系提供了可扩展路径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 语言模型表征只部分对齐脑活动，但很少有研究用脑记录主动引导模型训练。
method: 为LM引入脑对齐模块，利用大量观影fMRI数据进行脑信息监督微调，并与多种训练策略比较。
result: 脑信息微调稳定提升语言模型与留出fMRI数据的对齐，并表现出更强的泛化能力。
conclusion: 证明大脑记录可作为可扩展训练信号指导语言模型表征向脑活动靠拢。
---

## Abstract
Language models (LMs) provide rich representational spaces that partially align with neural activity during naturalistic experiences such as movie watching. Yet leveraging brain recordings to actively guide LM training remains underexplored. Here, we address this question by investigating whether functional MRI recordings can guide LLM training by aligning language representations with brain dynamics. Using over 50 hours of fMRI data from six participants watching Friends, plus 10 hours of held-out movies, we augmented pre-trained and randomly initialized LMs with a brain alignment module and compared multiple training strategies. Our results show three main findings. First, brain-informed fine-tuning consistently outperforms text-only baselines and brain-from-scratch models, with voxel-level gains that scale with both model size (GPT-2 124M, LLaMA-2 7B) and training duration (1–40 hours). These improvements generalize across participants and out-of-sample movies, yielding robust cross-subject and cross-stimulus encoding. Second, a dual-objective loss that balances language modeling with brain alignment surpasses brain-only optimization, producing more stable and generalizable encoders. Finally, brain supervision enriches LM representations with multisensory inductive biases: brain-fine-tuned models outperform unimodal baselines on VL-Commonsense, better capturing perceptual and associative properties (e.g., color, shape, co-occurrence) that text-only training underrepresents. Together, these results establish cortical dynamics as an effective supervisory signal, enabling scalable, generalizable, and brain-aligned LMs that internalize aspects of human-like multimodal representation.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：已有研究表明，语言模型（LM）的表征空间在自然主义刺激（如观影）下与人类神经活动存在部分对齐，但脑记录大多是作为**事后评估工具**（即在预训练之后比较模型激活与脑信号），很少被用来**主动引导或约束模型训练**。本文正是针对这一空白，提出核心问题：是否可以使用大规模fMRI记录作为监督信号，**直接引导语言模型的训练过程**，从而增强语言表示与大脑动态的对齐。
- **更广泛含义**：这项工作将神经科学数据从“评估基准”转变为“可扩展的训练信号”。如果大脑活动可以充当监督源，就能让模型在文本之外内化**多感觉整合和感知-关联属性**，这对于构建更具类人语义表征、更接近认知结构的大模型具有重要价值。

## 2. 方法论

- **总体思路**：在语言模型之上增加一个**脑对齐模块（brain alignment module）**，用大量观影时的fMRI信号作为监督，对模型进行“脑信息引导的微调”。
- **模型架构**：以下两种模型设定均被考虑：
  - **预训练语言模型**：GPT-2（124M）和 LLaMA-2（7B）；
  - **随机初始化的语言模型（brain-from-scratch）**，从零开始在脑信号的引导和语言目标下训练。
- **核心训练策略**：对比了多种训练方式：
  - **纯文本基线（text-only baselines）**：仅用语言建模目标；
  - **纯脑对齐优化（brain-only optimization）**：仅最小化模型表征与fMRI响应的差异；
  - **双目标联合损失（dual-objective loss）**：同时优化语言建模损失与脑对齐损失，使用损失加权策略平衡二者，该策略是文中重点推荐并表现最优的方法。
- **训练数据与流程**：使用六名被试观看《老友记》（Friends）的**超过50小时的fMRI数据**，在**逐体素（voxel-level）** 水平上对模型编码进行监督回归（编码模型框架），即将语言模型的内部表征映射到脑体素活动上。评估时使用**留出影片集（10小时）** 来测试跨刺激泛化。

## 3. 实验设计

- **数据集**：
  - 训练集：六名被试观看情景喜剧《Friends》的fMRI记录，总计超过50小时；
  - 留出/评测集：额外10小时的fMRI记录（观看其他电影），用于评估**跨刺激、跨被试泛化能力**；
- **评测任务**：
  - **脑编码任务**：在体素水平上比较模型预测的脑响应与实际fMRI信号的匹配度，报告跨被试和跨影片的泛化效果；
  - **VL-Commonsense 下游评测**：验证脑信息微调后的模型对**感知和联想属性**（如颜色、形状、共现关系等）的捕捉能力，与单模态基线比较。
- **对比方法**：
  - 文本训练基线（无脑信息）；
  - 从随机初始化直接用脑信息训练（brain-from-scratch）；
  - 单目标脑对齐优化（无语言建模损失）；
  - 双目标联合训练模型。

## 4. 资源与算力

- 论文提取内容中**未明确说明具体GPU型号、数量、总训练算力或硬件集群信息**。仅能推断训练规模不小，因为在两个模型规模（GPT-2 124M 和 LLaMA-2 7B）上、以1–40小时（总训练时长变化范围）进行了系统性训练。关于能耗/显存/GPU数目等物理资源指标，论文摘要及元数据未给出可靠数字，需要查阅全文方能确认。

## 5. 实验数量与充分性分析

- **实验维度较为丰富**：主要变化维度包括——
  - 模型规模（124M 到 7B）；
  - 训练时长（从1小时到40小时）；
  - 训练策略（纯文本、纯脑、双目标）；
  - 训练起点（预训练权重 vs 随机初始化）；
  - 数据来源/刺激、被试（训练被试 vs 留出被试，训练影片 vs 留出影片）。
- **跨被试、跨刺激泛化测试**是实验设计中的重要强项，能较好排除过拟合特定数据集的嫌疑。
- **充分性与局限判断**：虽然仅从摘要看实验覆盖面较广（通过系统改变训练时长、模型大小与目标函数来检验可扩展性），但受限于摘要文本，无法判断是否系统报告了全部消融组合、是否存在多次重复实验、标准误是否合理、脑体视频次是否做过多重比较校正等细节。就方法论验证而言：**公平性整体较好**（对比了单模态微调、从零训练等对照组），但**对评估数据集和下游任务的广度披露不足**（评测范围略显聚焦）。

## 6. 主要结论与发现

- **脑信息微调持续有效**：脑信息引导的微调在体素对齐上始终优于纯文本基线，且在模型规模扩大（GPT-2 124M → LLaMA-2 7B）和训练数据量增加（1–40小时）时均呈现持续增益，说明该方法具有**可扩展性**。
- **泛化能力强**：此类增益可泛化到留出的被试与留出的影片，说明脑信号中蕴含的监督信息不依赖于特定个体的低层噪声，而能捕捉相对**稳定、跨个体**的神经响应模式。
- **双目标损失更优**：同时优化语言建模与脑对齐的模型，比仅优化脑对齐的模型表现更稳定、泛化更强——表明语言建模目标起到**正则化与语义保持**作用，防止脑信号过拟合。
- **脑监督赋予多感觉归纳偏置**：脑信息微调后的模型在VL-Commonsense上优于单模态（纯文本）模型，能更好地表征视觉感知类特征（颜色、形状）及文本中少见的联想语义关系（共现等），说明来自fMRI的神经信号可将**非文本模态信息**反向注入语言模型。
- **宏观提示**：皮质动态可作为语言模型的有效监督信号，允许构建**类脑、可扩展且泛化**的多模态语义模型。

## 7. 优点

- **问题新颖**：将脑数据从评测基准转向训练信号，直接针对语言模型与脑活动“部分对齐但难以主动利用”的问题。
- **方法路径清晰**：通过脑对齐模块+联合损失设计，绕开了难以获取的大规模成对文本-脑数据限制，显示了实用可行性。
- **实验设计具有层级性**：同时在两个层面验证有效性：(a) 表征层面的脑对齐（编码精度）；(b) 行为/语义层面的下游任务（VL-Commonsense），形成“神经信号—模型表征—任务能力”的完整验证链。
- **可扩展性验证可靠**：系统探索不同模型大小和不同训练时长，超出单一模型、单一数据量的局限。
- **泛化指标扎实**：注重跨被试、跨影片测试，结论不受单一被试、单一刺激驱动。

## 8. 不足与局限

- **资源与实现细节缺失**：摘要或元数据未提供硬件配置、训练开销和代码开源与否等信息，可复现性表述不足。
- **脑模态仅限fMRI**：fMRI 的时间分辨率较低，无法捕捉语言加工中毫秒级的神经动态。文中似乎未涉及MEG/ECoG等更高时间分辨率模态的比较，限制了结论的时效性推断。
- **下游评测范围有限**：只在VL-Commonsense上验证了感知属性的优越性，缺少其他如具身推理、视觉问答、脑—行为一致性等更广评测目标。
- **文本中心设置可能限制泛化**：虽然模型使用了脑信息，但基础任务仍是语言建模，对于真实多模态场景（视觉/听觉输入处理）中的适用程度尚不清楚。
- **被试规模小、刺激域窄**：六名被试且主要刺激来自《老友记》与人脑观影数据集，属于特定自然主义语境，**有可能引入语料或社会情境偏差**。
- **机制解释仍不明朗**：脑信息微调为何能加强多感觉表征——是导入了图像的统计特征、还是强化了物体关联网络——仍需更精细的机制分析。

（完）
