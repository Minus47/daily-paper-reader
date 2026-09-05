---
title: Learning and Reusing Abstract Latent Actions in a Hippocampal-Entorhinal-Inspired World Model
title_zh: 在海马-内嗅启发的世界模型中学习并复用抽象潜在动作
authors: "Tianqiu Zhang, Muyang Lyu, Xiao Liu, Si Wu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=RSvfY6dRVN"
tags: ["query:abstraction"]
score: 9.0
evidence: 海马编码内容细节、内嗅编码抽象结构并可复用抽象潜在动作，对应具象到抽象的类脑概念组织
tldr: 基于海马-内嗅环路对空间与抽象概念空间的表征机制，该文研究从动态经验中抽取可复用的抽象潜在动作。模型区分海马对内容细节的编码与内嗅对抽象结构的编码，并利用相似转移动态推断共享模式，实现跨情境结构泛化。结果表明这一抽象机制可被学习并迁移到不同上下文。这为从具体经验到抽象概念空间的类脑组织提供了一个可实现的建模案例。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 人类能抽象动态经验并迁移共享结构，但海马-内嗅环路如何表示和复用抽象潜在动作仍需探索。
method: 分离海马内容细节与内嗅抽象结构，通过学习转移动态形成可跨情境复用的抽象潜在动作。
result: 验证模型可通过观察相似转换动态推断共享抽象模式并迁移到不同上下文。
conclusion: 为构建从具象到抽象的类脑概念空间提供了海马-内嗅机制启发的建模路径。
---

## Abstract
Humans are capable of abstracting dynamic experiences into structured representations, facilitating both the inference of shared patterns by observing similar transition dynamics and the transfer of these structures across varied contexts. The hippocampal-entorhinal circuit, widely known for its role in spatial navigation, also supports the representation of abstract conceptual spaces crucial for non-spatial cognitive processes. This function emerges from the distinct yet integrated encoding of content-specific details by the hippocampus and abstract structures by the entorhinal cortex, facilitating structural generalization across varied contexts. Although the hippocampal-entorhinal circuit has been previously explored as a predictive system for binding contents, the process for concurrently extracting abstract structures from continuous real-world dynamics remains largely understudied. In this work, we propose a computational model inspired by the hippocampal-entorhinal circuit, capable of simultaneously inferring latent actions to form abstract structures and constructing predictive world models from real-world video sequences. Our model combines an inverse model for extracting abstract latent actions with a hippocampal-entorhinal-inspired coupling model that separately encodes contents and structures, leveraging action-driven path integration for prediction. Experimental results demonstrate that our model effectively captures abstract latent actions, reuses them robustly across diverse contexts, and achieves reliable predictive performance in both familiar and novel environments. Additionally, our analysis of latent representations from 3D object rotation datasets highlights why latent actions extracted through entorhinal cortex representations demonstrate greater abstraction and reusability. This work provides novel insights into the brain-inspired mechanisms underlying the self-supervised learning of abstract latent actions and world models from real-world dynamics, illuminating cognitive processes essential for transfer learning and data-efficient learning.

---

## 论文详细总结（自动生成）

# 论文总结：在海马-内嗅启发的世界模型中学习并复用抽象潜在动作

## 1. 论文的核心问题与整体含义

- **核心问题**：人类能够将动态经验抽象为结构化表征，从而在不同情境间迁移共享的结构与规律。尽管海马-内嗅环路已被视为空间导航的神经基础，并被扩展解释为支持非空间认知的抽象概念空间表征，但**该环路如何从连续的现实世界动态中同时提取“抽象结构”并构建预测性世界模型**这个问题，仍缺少计算层面的研究。
- **研究动机**：已有研究将海马-内嗅环路作为“内容绑定”的预测系统，但对结构提取的动态过程理解不足。本文希望探索这一神经环路如何支撑自监督地从现实视频中学习**可复用的抽象潜在动作**，并以此实现跨情境的结构泛化与迁移。
- **整体含义**：工作不仅为海马一内嗅系统的计算功能提供了新的建模假设，也为类脑的、数据高效且可迁移的机器学习系统提供了启发。

## 2. 论文提出的方法

- **核心思想**：模仿海马与内嗅皮层在表征上的分工——海马编码与当前情境紧密绑定的内容细节，而内嗅皮层编码与特定内容解耦的抽象结构——构建一个能同时从视频序列中提取抽象潜在动作并预测未来状态的类脑世界模型。
- **模型组成**：
  - **逆向模型**：从连续观察到的转移动态中反推潜在的抽象动作；该动作不依赖具体内容，而是描述变化的本质。
  - **海马-内嗅启发的耦合模型**：将“内容”与“结构”分开编码，两者协同形成对当前状态和前向动态的预测。
  - **动作驱动的路径整合**：利用提取的抽象潜在动作进行预测，类似于内嗅中基于路径整合的神经计算。
- **学习范式**：采用自监督方式，直接从真实世界的视频序列中学习，无需额外的动作标注或人工标签。
- **实现要点（按文字说明）**：
  1. 观测连续帧；
  2. 由逆向模型推断当前情境下可解释的低维潜在动作；
  3. 将内容信息（海马路径）与结构信息（内嗅路径）分别映射到两类表征空间；
  4. 通过沿潜在动作方向的路径整合更新隐状态，实现对未来帧的预测或重建；
  5. 在训练中联合优化“对转移动态的逼真预测/重建”与“潜在动作代码的简洁/可复用性”。
- 论文元数据中未提供具体公式或损失函数的数学细节，仅能从摘要中重建上述整体算法流程。

## 3. 实验设计

- **实验场景与数据集**：
  - 使用**真实世界视频序列**作为主要学习来源。
  - 元数据显示其中一类数据为**3D 物体旋转数据集**，被专门用于分析潜在表征的性质（抽象性与可复用性）。
  - 元数据中未进一步说明是否还使用了其他公开基准数据集（如 Atari、MuJoCo、机器人操作视频等）。
- **评测任务与基准（Benchmark）**：
  - 检验模型是否能够提取抽象的潜在动作；
  - 检验这些动作是否能在**多种不同上下文中被稳健复用**；
  - 检验模型在**熟悉环境和新型环境**中的预测性能。
- **对比方法**：从摘要与元数据中**未看到**与特定基线方法（如 Dreamer、RSSM、forward/inverse dynamics 基线等）的明确对比列表，因此无法据此判断相对 SOTA 的位置。

## 4. 资源与算力

- 论文中**未明确提及**所使用的 GPU 型号、数量、训练时长、模型参数量或能耗等计算资源信息。
- 这一缺失是本论文在可复现性信息上的一项明显空白。

## 5. 实验数量与充分性

- **已知实验覆盖若干维度**：
  - 抽象潜在动作的“能否提取”验证——至少一组主实验；
  - 跨多个不同上下文的“复用”实验——元数据中提及多种上下文；
  - 预测性能评估——在熟悉与未知环境条件中均有考察；
  - 3D 物体旋转数据集上的**表征分析实验**，用于解释为何内嗅表征更具抽象性。
- **充分性与公平性**：
  - 如果以上实验均被严格执行并且有量化的对照指标，则基本能够支撑论文自述的目标，即“可提取、可泛化、可解释”；
  - 但当前元数据内容**不足以确认**存在系统性的消融实验（如去掉内嗅/海马分离、去掉逆向模型、替换为前向模型等进行的组件消融）；
  - 也**未展示**与同类世界模型或动作抽象方法的横向公平比较，因此“有效性是否显著优于现有方法”难以从现有信息中评判。
- 总体而言，实验结果在验证内部一致性上具备基本覆盖，但作为一篇投稿论文，其横向对比与消融分析的公开可查性不足。

## 6. 论文的主要结论

- 所提出的模型可以从真实视频动态中**成功习得抽象的潜在动作**，这类动作捕获的是“转变的本质”而非表面的像素或内容。
- 习得的抽象潜在动作可被**稳健地在多样上下文中复用**，说明其并非任务或场景特异的过拟合表征。
- 在**熟悉与新颖环境**中，该模型均能达到可靠的预测表现，体现了跨情境的结构泛化能力。
- 对 3D 物体旋转数据的表征分析显示：经由内嗅皮层式表征提取的潜在动作，相比由内容驱动表征得到的动作具有**更强的抽象性、更高的迁移复用能力**，与海马/内嗅分离编码的神经科学假设一致。

## 7. 优点

- **类脑动机清晰**：不是泛泛引用“记忆”或“预测编码”，而是将海马与内嗅两者的明确分工映射到内容编码与结构编码两个模块，有神经机制上的可解释性。
- **方法设计自洽**：逆向模型抽取潜在动作，配合动作驱动的路径积分式预测，形成一套可用于连续视频流的联合学习框架。
- **自监督潜力**：不依赖外部的动作标注，直接从真实世界动态中学习，适配更真实、更接近动物或人类学习条件的数据设定。
- **强调模型的可迁移性**：将“跨情境泛化—复用抽象知识”视为核心指标，这比在单一环境中做像素级重建更加具有认知相关性，也更接近人类的学习方式。
- **表征分析有说服力**：构建了“为什么结构表征比内容表征更容易迁移”的实证分析，而不仅停留在端到端性能的提升上。

## 8. 不足与局限

- **可用信息不足**：从当前提供的元数据与摘要来看，无法获得具体的模型结构图、维数、损失函数和训练配置细节，可复现性因此受限。
- **实验对比不够充分**：未报告与主流基于模型强化学习/世界模型（如 Dreamer 系列、Transfomer-based world models）或潜在动作发现方法的对比结果，缺乏在相同基准上的意义检验。
- **算力与训练成本缺失**：完全没有披露训练时使用的 GPU 数量、时长、数据量规模等信息，难以判断该方法在资源需求上的可负担性。
- **潜在评测偏差风险**：如果主要实验场景限于 3D 物体旋转类数据，其“跨上下文”可能更多是内容外观或颜色、纹理的变化，而未必是任务逻辑、动力学参数或 interactivity 层面的大幅变化；因此宣称的抽象泛化可能高估。
- **对神经科学解释的简化**：即便分离内容与结构的思路是合理的，海马与内嗅实际交互远比该模型复杂（如重激活、重放、θ 节律等时间动态均未建模），将实验结果直接映射为脑机制的支撑仍需谨慎。
- **应用限制**：当前模型所展示的能力在需要长时记忆、稀疏奖励或大型动作空间的任务上是否仍然成立并不明确，离实际工程部署或具身智能任务仍有距离。

（完）
