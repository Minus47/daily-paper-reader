---
title: "Neural Brain Fields: A NeRF-Inspired Approach for Generating Nonexistent EEG Electrodes"
title_zh: 神经脑场：受NeRF启发的生成虚拟EEG电极方法
authors: "Shahar Ain Kedem, Itamar Zimerman, Eliya Nachmani"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=NB86kKgN60"
tags: ["query:eeg-align"]
score: 6.0
evidence: 受NeRF启发的EEG生成建模可合成不存在的电极信号，为下游EEG深度模型提供数据增强与稳健性支持
tldr: EEG数据存在长度不一、噪声高、跨被试差异大且高质量数据集稀缺等问题。本文借鉴NeRF思想提出神经脑场，将电极位置信息编码进神经网络以生成原本不存在的虚拟电极，从而支持对记录的灵活渲染和编辑。该方法有望缓解缺失电极和数据集不足对深度学习EEG建模的制约。此类生成式建模可作为EEG表征学习和下游任务的数据预补充手段。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: EEG数据跨时长、跨被试差异大、信噪比低且数据集稀缺，深度学习建模困难。
method: 借鉴NeRF将场景记忆与渲染分离的思想，用神经网络参数化脑电场的连续表示，生成并编辑缺失电极信号。
result: 能生成虚拟电极并支持从任意电极位置渲染，为缓解EEG数据不完整提供了新工具。
conclusion: 神经脑场为不规则、高噪声EEG数据的深度建模与数据增强提供了新的生成式方向。
---

## Abstract
Electroencephalography (EEG) data present unique modeling challenges because recordings vary in length, exhibit very low signal to noise ratios, differ significantly across participants, drift over time within sessions, and are rarely available in large and clean datasets. Consequently, developing deep learning methods that can effectively process EEG signals remains an open and important research problem. To tackle this problem, this work presents a new method inspired by Neural Radiance Fields (NeRF). In computer vision, NeRF techniques train a neural network to memorize the appearance of a 3D scene and then uses its learned parameters to render and edit the scene from any viewpoint. We draw an analogy between the discrete images captured from different viewpoints used to learn a continuous 3D scene in NeRF, and EEG electrodes positioned at different locations on the scalp, which are used to infer the underlying representation of continuous neural activity. Building on this connection, we show that a neural network can be trained on a single EEG sample in a NeRF style manner to produce a fixed size and informative weight vector that encodes the entire signal. Moreover, via this representation we can render the EEG signal at previously unseen time steps and spatial electrode positions. We demonstrate that this approach enables continuous visualization of brain activity at any desired resolution, including ultra high resolution, and reconstruction of raw EEG signals. Finally, our empirical analysis shows that this method can effectively simulate nonexistent electrodes data in EEG recordings, allowing the reconstructed signal to be fed into standard EEG processing networks to improve performance.

---

## 论文详细总结（自动生成）

> **说明**：以下总结完全基于您提供的论文摘录和元数据。由于可用的原始内容十分有限（仅含摘要与元数据，不包含正文的完整实验章节），因此不便对未被摘要言明的细节作过度推断，但会尽可能提取和整理可确认的信息，并指出信息缺失之处。

## 一、论文的核心问题与整体含义（研究动机与背景）

- **研究背景**：脑电图（EEG）数据是一类在深度学习建模中极具挑战性的神经信号数据。摘要明确指出 EEG 记录存在几个固有困难：
  - 记录长度不一致（跨样本、跨被试）；
  - 信号信噪比（SNR）极低，噪声干扰严重；
  - 不同被试之间的神经活动差异显著；
  - 单次记录过程中信号存在随时间漂移；
  - 大规模、干净、标注良好的 EEG 数据集很少。
- **核心问题**：由于上述数据特性，如何设计能够有效处理 EEG 信号的深度学习方法，仍然是一个开放且重要的研究问题。具体来说，EEG 电极位置分布有限、某些电极缺失、以及高质量标注数据匮乏，都会制约深度学习模型在下游任务上的表现。
- **整体含义**：作者提出一个全新的思路——借鉴计算机视觉中神经辐射场（NeRF）的连续场景表示方法，将 EEG 信号视为一个连续的“神经场”。这样一来，模型不仅可以在单个 EEG 样本上学习到一个固定长度的稠密向量表示，还能借此**合成原本不存在的、位于任意头皮位置的虚拟电极信号**，从而缓解 EEG 数据不完整、电极缺失以及数据量不足等问题，为下游 EEG 深度模型提供一种可验证的数据增强手段。

## 二、方法论：核心思想、关键技术细节与算法流程

- **核心思想——EEG 与 NeRF 之间的类比**：
  - 在 NeRF 中：从不同视角拍摄的离散 2D 图像被用于学习一个连续的 3D 场景表示，训练完成后可以基于该隐式表示从**任意视角**渲染/编辑场景。
  - 在本文中：位于头皮不同位置的 EEG 电极（每个电极相当于一个“观察视角”），其采集的信号扮演了类似于“离散图像”的角色；而电极所记录信号的底层来源——连续的神经活动——则类比于 NeRF 中的 3D 场景。
  - 由此，作者主张可以用一个神经网络对 EEG 信号背后的连续场进行参数化，将电极的空间位置（以及时间信息）作为输入。

- **网络训练与表征形成（按摘要的算法流程描述）**：
  1. 对**单个 EEG 样本**，采用类似 NeRF 的训练范式训练一个神经网络；
  2. 在网络训练完成后，其学到的权重被凝练为**一个固定长度且信息丰富的向量**，该向量被用来编码整段 EEG 信号（相当于把信号的全局信息压缩进网络参数中）；
  3. 借助这一隐式表征，模型能够从**任意（此前未见过的）时间步**以及**任意的电极空间位置**进行信号渲染和重建。

- **技术关键词**：
  - 连续神经表示（continuous neural representation）；
  - 固定大小权重向量（fixed-size informative weight vector）；
  - 任意空间电极插值 / 渲染（spatial electrode rendering）；
  - 任意时间分辨率重建（temporal rendering at any resolution）。

- **注**：由于未提供正文，本文无法给出具体的网络结构、损失函数、坐标编码方式、训练迭代算法等数学或工程细节。

## 三、实验设计

- 根据摘要与元数据，可以确认的实验内容主要包括：
  - 验证模型能够**在未见过的时间步**上重建原始 EEG 信号；
  - 验证模型能够生成/模拟 EEG 记录中**原本不存在的电极**的数据；
  - 展示模型支持**连续可视化**：可在任意期望的时间分辨率（包括超高清分辨率）下对脑活动进行可视化渲染；
  - 在下游实验中，将模型重建/模拟出的电极信号输入**标准 EEG 处理网络**，检验该方法能否提升下游任务的性能。
- **数据集与基准（Benchmark）**：摘要文本中**没有提及**任何具体的数据集名称、被试数量、电极数目、下游任务类型（如睡眠分期、癫痫检测、运动想象分类等），也**没有列出对比的基线模型**。
- 因此，基于现有文本，无法客观判断其实验基准的权威性与广度。

## 四、资源与算力

- **已说明的信息**：在所提供的全部文本（摘要与元数据）中，**没有披露任何算力信息**（没有 GPU 型号、GPU 数量、训练时长、显存开销等）。
- 结论：资源与算力方面的信息完全缺失。

## 五、实验数量与充分性

- **实验数量**：从摘要来看，主要实验方向有三项：(1) 任意时间重建；(2) 虚拟电极生成/模拟；(3) 与下游标准 EEG 网络的配合以提升性能。但从论文元数据中无法获知各方向具体的实验组数、消融设置、对比方法的数量。
- **充分性评估**：
  - 由于我们只能看到摘要级别的信息：
    - 无法确认是否做了**跨数据集泛化**实验；
    - 无法确认是否有**消融研究**（如网络结构选择、位置编码设计、训练策略）来验证关键设计决策；
    - 无法确认是否与**现有电极插值方法**（如样条插值、球面插值）做过公平对比；
    - 也无法得知下游任务是否在多被试、多种 EEG 范式上得到统一验证。
  - 因此，从摘要所能获取的信息来看，**实验的完整性和公平性目前无法得到充分评估**。

## 六、论文的主要结论与发现

- **主要结论（综合摘录与元数据）**：
  1. 神经网络可以像 NeRF 记忆 3D 场景那样，以**单个 EEG 样本**为对象，训练出一个固定大小且信息丰富的权重向量，从而编码整段 EEG 信号——这一做法对不规则、高噪声、跨被试差异大的 EEG 数据具有表征学习上的潜力。
  2. 借助该连续场表示，可以实现对 EEG 信号在**时间维**和**空间维（电极位置）**两个方向上的任意分辨率渲染，包括超高清连续可视化与原始信号的还原。
  3. 该模型能有效地**模拟 EEG 记录中原本不存在的电极信号**；这些重构/虚拟电极数据可以被输入到标准的 EEG 深度处理网络，从而**提升其输出性能**。
  4. 总地来说，该论文为不完整 EEG 数据集的深度学习建模提供了一种以生成式方法为基础的预处理/增强新方向。

## 七、优点

- **问题切入点有价值**：EEG 的高噪声、电极缺失、被试间差异大是实际场景中的真实痛点；从“数据增强”与“电极补齐”角度切入具有临床与应用价值。
- **类比富有启发性**：将 3D 场景渲染中坐标到颜色的映射映射到电极位置/时刻到电位的映射，两套坐标空间之间有着较好的结构平行性。这个新颖的跨领域迁移是本文最突出的亮点。
- **粒度上的突破**：支持任意时间时刻 + 任意电极空间位置的渲染，意味着可以从离散、稀疏的原始记录中生成“连续脑场”，可能有利于下游可视化、伪电极生成和数据补齐。
- **兼容现有架构的可操作性**：生成的电极信号可以输入标准 EEG 处理网络，说明所提方法作为前置增强模块，在现实工作流中具有一定的即插即用潜力。

## 八、不足与局限

- **信息不全导致的评估局限**：论文正文缺失，无法核实技术细节、实验设计和统计显著性，因此以下局限是基于摘要和元数据的合理推断。
- **跨被试泛化问题**：文中提到“在单个 EEG 样本上训练网络”，若是以单样本最优化的方式训练，则每个新样本都需要重新训练一个网络，如何做到高效推理并跨被试迁移，是一个潜在的实效瓶颈。
- **真实电生理约束可能被忽略**：头皮表面电极间的信号具有容积传导效应，即真正脑电源在皮层，电极信号是源在头皮上的非线性混合。若只用“电极坐标”和“时间”作为条件，不考虑头模型、源位置与传导物理过程，所生成的虚拟电极信号可能只具备表面统计上的合理性，而缺乏真实的神经生理学意义。
- **对高频噪声和伪迹的捕捉**：EEG 噪声构成复杂（眼电、肌电、工频干扰等），NeRF 式连续场方法倾向于学习平滑的连续映射，是否会在“超高清渲染”时过度平滑或错误放大伪迹，有待验证。
- **下游性能提升的普适性存疑**：摘要只笼统表示“可以提升性能”，没有报告提升幅度、适用任务以及是否只在特定电极缺失率下成立。
- **方法名/方向新颖但拒稿记录值得留意**：该论文标注为 ICLR-2026-Rejected-Public（2025-09-16 提交，评分 6.0）。尽管评分不低，但最终未被接收，可能说明审稿人对实验广度、真实数据集验证、与现有插值/SOTA 方法的对比结果、或者应用价值存在疑虑。

（完）
