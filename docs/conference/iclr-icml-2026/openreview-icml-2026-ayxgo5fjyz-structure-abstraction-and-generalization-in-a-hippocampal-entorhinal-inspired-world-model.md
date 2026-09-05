---
title: Structure Abstraction and Generalization in a Hippocampal-Entorhinal Inspired World Model
title_zh: 海马-内嗅启发的世界模型中的结构抽象与泛化
authors: "Tianqiu Zhang, Muyang Lyu, Xiao Liu, Si Wu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9f1f805b9f6c66b338094e85433e74e0d47c4987.pdf"
tags: ["query:abstraction"]
score: 9.0
evidence: 海马-内嗅启发的分层模型同时抽取抽象结构并构建预测世界模型，是构建类脑概念空间的直接方法示例
tldr: 针对连续高维动态中同时提取抽象结构并迁移推理机制不清的问题，提出受海马-内嗅环路启发的分层预测世界模型。反演模型抽取结构信息，HPC-MEC耦合模型分离关系结构（内嗅皮层）与整合的情节场景（海马）。以旋转动力学为基准的实验证明模型能学习潜在转移并用于泛化。该研究提供了从动态经验构建类脑抽象概念空间的可计算方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 海马-内嗅环路虽能表征空间和概念空间，但如何并发地从高维连续动态中抽象结构仍不明确。
method: 通过反演模型抽取潜在结构，并结合海马-内嗅耦合模型分离关系结构与情节式场景。
result: 在旋转动力学基准上验证了模型能学习抽象结构并将结构泛化到新动态。
conclusion: 为体现场景具体细节到抽象关系结构的类脑概念建模提供了一条具体路径。
---

## Abstract
Humans abstract experiences into structured representations to facilitate pattern inference and knowledge transfer. While the hippocampal-entorhinal (HPC-MEC) circuit is known to represent both spatial and conceptual spaces, the mechanisms for concurrently extracting abstract structures from continuous, high-dimensional dynamics remain poorly understood. We propose a brain-inspired hierarchical model that simultaneously infers latent transitions and constructs a predictive visual world model. Our architecture employs an inverse model for structural extraction alongside an HPC-MEC coupling model that dissociates relational structures (MEC) from integrated episodic scenes (HPC). Using rotation dynamics as a benchmark, we demonstrate the model's capacity for structural abstraction. By leveraging velocity-driven path integration, the framework enables robust prediction and structural reuse across diverse contexts, thereby achieving structural generalization. This work provides a novel computational framework for understanding how brain-inspired, self-supervised learning of world models facilitates the acquisition of reusable abstract knowledge.

---

## 论文详细总结（自动生成）

基于论文元数据与摘要，以下为该论文的详细中文总结：

## 1. 核心问题与研究背景

尽管海马-内嗅（Hippocampal-Entorhinal, HPC-MEC）环路已被广泛证实既能表征空间环境，又能表征抽象的概念空间，但机制上仍存在一个重要空缺——**生物体如何从连续、高维的动态感官经验中，同时抽取可复用的抽象结构，并将其迁移到新情境？** 这一问题涉及认知科学中"结构抽象与泛化"的根本机制。传统世界模型通常停留在预测像素级动态的层面，缺乏将经验凝练为可迁移的关系结构的能力；而类脑计算方法虽然借鉴了海马的结构特性，却较少直接回答"如何从经验流中并发地学习结构"这一计算问题。

## 2. 方法论

论文提出了一种受大脑启发的分层预测世界模型（hierarchical predictive world model），其核心设计包含两个相互耦合的部分：

- **反演模型（Inverse Model）用于结构抽取**：通过反演（inference）机制从高维连续观测中提取潜在的离散或低维结构——具体来说，是从视觉动态中推断出隐含的转移关系（latent transitions）。这一步骤实现了"从表象到结构"的抽象。
- **HPC-MEC 耦合模型用于结构分离与整合**：借鉴海马-内嗅环路的解剖分工——内嗅皮层（MEC）侧编码**关系结构**（relational structures），海马（HPC）侧整合**情节式场景**（episodic scenes）。这种解耦设计使得模型既能维持对具体经验的记忆，又能抽取可泛化的关系规则。
- **速度驱动的路径积分（velocity-driven path integration）**：模型利用潜在状态中的速度信息进行路径积分，从而实现跨不同情境的鲁棒预测与结构复用。

整体上，该框架不依赖显式标签，以**自监督学习**方式训练，将世界模型的学习与抽象结构的学习统一在同一个类脑架构中。

## 3. 实验设计

论文以**旋转动力学（rotation dynamics）**作为基准任务，在该环境中验证模型的能力。值得注意的是：

- 摘要与元数据中仅报告了单一基准场景（旋转动力学），未提及多数据集或与其他 baseline 的系统比较。
- 核心验证目标是模型能否从旋转动态中学习到潜在转移规则，并获得**结构泛化**能力——即在新的多样性情境中复用已学到的结构。
- 目前给出的材料中没有列出具体对比方法（如是否与 LSTM、Transformer 或标准 VAE-based world model 进行对比），也没有详细说明评估指标。

## 4. 资源与算力

论文的可获取内容（元数据与摘要）**没有提供任何算力相关信息**——包括 GPU 型号与数量、训练轮次、训练时长、参数量等均未披露。这可能是由于当前可获取的材料仅为论文前置摘要而非完整正文，若需获取详细算力配置，需查阅论文正文的实验设置部分。

## 5. 实验数量与充分性

从可获取的信息来看，实验的整体数量有限：

- 只在旋转动力学一个 benchmark 上做了验证。
- 未在元数据和摘要中反映消融实验的必要性（例如去掉 HPC-MEC 解耦结构后泛化是否下降，或替换反演模型后的效果差异）。
- 未提及跨领域验证（如自然视频、导航任务、符号推理任务等），也未报告与既有方法在定量指标上的对比。因此，实验在**覆盖广度与对比公平性**上难以从现有材料中做出充分评估。该验证更接近一种"概念证明"（proof-of-concept），展示了该架构具有结构抽象与迁移的潜力，但尚不足以证明其在大规模、多样化任务上的普适优势。

## 6. 主要结论与发现

论文的核心结论可概括为三点：

1. 受海马-内嗅环路启发的分层预测模型能够从连续的、非平稳的动态经验中**同时进行结构抽取与视觉预测**。
2. 将内嗅皮层式的关系结构表征与海马式的情景整合表征解耦，是使模型获得**跨情境迁移能力**的关键设计。
3. 该工作为"从动态经验中构建类脑抽象概念空间"提供了第一条可计算的路径——即从感知细节逐步归纳为可复用的结构规则，这一路径为将来构建具有更强泛化能力的认知智能体提供了参考框架。

## 7. 优点

- 与神经科学的结合紧密而具体：不是笼统地套用"海马=记忆"的类比，而是将内嗅的**关系性表征**和海马的**情节性表征**的功能分工落实为架构中的模块化分离，具备计算上的可解释性。
- 同时解决两个问题：一方面构建可预测的视觉世界模型，另一方面从模型内部提取可迁移的抽象结构，将感知与认知统一在同一个学习框架内。
- 具备自监督属性，不需要额外标注即可进行结构学习，具有较好的生物合理性。
- 提出了"速度驱动的路径积分"这一机制，与内嗅皮层中网格细胞所表征的速度积分机制高度契合，给结构泛化提供了神经层面的依据。

## 8. 不足与局限

- **实验场景单一**：当前只在旋转动力学中做验证，结构的复杂度有限，尚不足以证明方法可泛化到更复杂的高维环境（如自然动态场景、多物体交互或非欧几里得结构）。
- **缺乏与基线方法的量化比较**：从现有材料看，没有报告与标准深度学习世界模型（如 Dreamer、RSSM 等）的对比数据，对泛化能力的优势缺乏相对证据支撑。
- **缺少消融与机制分析**：HPC-MEC 解耦中每一部分的贡献、反演模型的鲁棒性等尚未被系统地分析。
- **可扩展性未知**：未报告模型在更高分辨率输入下的计算代价、收敛速度以及在大规模训练上的表现。
- **当前评审信息的局限性**：由于提取到的内容为 OpenReview 验证页而非完整论文正文，上述不足的确认还需结合全文实验细节作最终判断。

---

（完）
