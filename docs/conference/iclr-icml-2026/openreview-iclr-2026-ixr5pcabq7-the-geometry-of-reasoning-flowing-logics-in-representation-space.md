---
title: "The Geometry of Reasoning: Flowing Logics in Representation Space"
title_zh: 推理的几何：逻辑在表征空间中的流动
authors: "Yufa Zhou, Yixiao Wang, Xunjian Yin, Shuyan Zhou, Anru Zhang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ixr5Pcabq7"
tags: ["query:abstraction"]
score: 6.0
evidence: 以几何流刻画表征/概念空间中的推理轨迹，研究逻辑与概念空间的几何组织
tldr: 大模型推理过程的内部结构难以直接观察。该文提出几何框架，将推理表示为表征空间中的光滑嵌入轨迹，并用位置、速度与曲率分析逻辑结构；实验用同一自然演绎命题配不同语义载体，验证模型内化了超越表面形式的逻辑。文中还把逻辑陈述视为控制流动速度的局部控制器，为理解表征与概念空间中的推理行为提供了形式化工具。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 大模型内部的推理与概念组织缺乏透明的几何刻画，逻辑难以与表面语义分离。
method: 将推理建模为表征空间中的光滑轨迹流，引入位置、速度与曲率描述逻辑变化。
result: 验证了推理流的存在以及逻辑陈述可看作速度的局部控制器。
conclusion: 几何流框架为表征与概念空间的推理分析打开了新途径。
---

## Abstract
We study how large language models (LLMs) "think" through their representation space. 
We propose a novel geometric framework that models an LLM's reasoning as flows---embedding trajectories evolving where logic goes. 
We disentangle logical structure from semantics by employing the same natural deduction propositions with varied semantic carriers, allowing us to test whether LLMs internalize logic beyond surface form. 
This perspective connects reasoning with geometric quantities such as position, velocity, and curvature, enabling formal analysis in representation and concept spaces. 
Our theory establishes: (1) LLM reasoning corresponds to smooth flows in representation space, and (2) logical statements act as local controllers of these flows' velocities. 
Using learned representation proxies, we design controlled experiments to visualize and quantify reasoning flows, providing empirical validation of our theoretical framework. 
Our findings indicate that training solely via next-token prediction can lead LLMs to internalize logical invariants as higher-order geometry in representation space, challenging the "stochastic parrot" argument.
Experiments across Qwen and LLaMA model families further suggest the presence of a general, possibly universal, representational law underlying machine understanding and human linguistic regularities, largely independent of specific training recipes or model architectures. 
Our work serves as both a conceptual foundation and practical tools for studying reasoning phenomena, offering a new lens for interpretability and formal analysis of LLMs' behavior.

---

## 论文详细总结（自动生成）

# 《推理的几何：逻辑在表征空间中的流动》中文总结

> 说明：由于原始 PDF 链接为 OpenReview 验证页面，未能获取完整论文正文；以下总结严格依据所给的论文元数据与摘要撰写，部分细节在原文中未明确说明，因此会标注为“未在提供信息中说明”。

## 1. 论文的核心问题与整体含义

- **研究动机**：大型语言模型（LLM）的内部推理过程难以直接观察，现有解释性工作往往停留在注意力权重或神经元激活层面，缺乏对“推理在表征空间中如何展开”的几何化理解。
- **核心问题**：LLM 的推理是否可以被建模为表征空间中的连续轨迹？逻辑结构能否与表面语义分离并被模型内化？
- **整体意义**：论文挑战了“随机鹦鹉”（stochastic parrot）观点——即认为 LLM 只是表面文本的概率预测器。作者提出，仅通过下一词预测训练的模型，也可能在表征空间中内化逻辑不变量，表现为某种高阶几何结构。

## 2. 方法论

- **核心思想**：将 LLM 的推理过程建模为表征空间中的“流”（flows），即嵌入向量随推理步骤演变形成的光滑轨迹。
- **关键概念**：借助几何量来描述推理：
  - **位置（position）**：推理当前状态在表征空间中的位置；
  - **速度（velocity）**：状态随推理步骤变化的快慢与方向；
  - **曲率（curvature）**：推理路径的弯曲程度，反映逻辑转折或结构变化。
- **逻辑与语义的分离**：采用同一套自然演绎命题，但替换其语义载体（即使用不同表面内容承载相同逻辑形式），以检验模型是否捕捉到超越表面形式的逻辑结构。
- **理论主张**：
  1. LLM 的推理对应表征空间中的光滑流；
  2. 逻辑陈述充当这些流的“局部速度控制器”（local controllers），即逻辑会调节推理轨迹前进的速度与方向。
- **实现方式**：通过学习的表征代理（learned representation proxies）设计受控实验，将推理流可视化并量化，从而对理论框架进行实证验证。

## 3. 实验设计

- **数据集 / 场景**：使用了自然演绎（natural deduction）命题，并构造不同语义载体以分离逻辑与内容。
- **Benchmark**：未在摘要中说明具体基准数据集名称；但实验基于“受控逻辑推理任务”而非传统问答基准。
- **对比方法**：未在摘要中提及与既有方法进行直接性能对比；对比更多体现在“不同模型家族”和“不同语义载体”之间。
- **模型范围**：实验涵盖 Qwen 和 LLaMA 两个模型家族，用于检验框架是否具有跨架构、跨训练方式的普遍性。

## 4. 资源与算力

- **未在提供信息中说明**。摘要没有提及 GPU 型号、数量、训练时长或推理开销等资源信息。这也意味着该工作可能在计算层面对可复现性资源需求不透明。

## 5. 实验数量与充分性

- **实验数量**：未给出具体组数；从摘要可见的主要维度包括：
  - 不同语义载体下的同一逻辑命题；
  - 两种模型家族（Qwen 与 LLaMA）；
  - 位置/速度/曲率的几何量分析。
- **充分性评估**：
  - **积极点**：跨模型家族实验有助于增强结论的普遍性；语义替换实验能较好排除表面拟合。
  - **不足**：缺少与传统可解释性方法或替代推理框架的对照实验；没有报告统计显著性或误差分析；受控逻辑任务较简单，是否反映复杂真实世界推理存疑；仅凭两个模型家族难以得出“普遍表征定律”。

## 6. 主要结论与发现

- LLM 的推理过程可以表示为表征空间中的光滑嵌入轨迹，即“推理流”存在。
- 逻辑陈述的作用类似于局部控制器，能够调节推理流的速度。
- 通过在相同逻辑不同语义载体上的实验，模型表现说明其内化了逻辑不变量，而不仅是表面语言规律。
- 这一结论支持：仅靠下一词预测训练的 LLM 也能形成内部的高阶几何逻辑表征，从而对“随机鹦鹉”论点构成挑战。
- 实验跨 Qwen 和 LLaMA 家族的现象表明存在一种可能普遍的表征规律，且该规律与具体训练配方和架构关系不大。

## 7. 优点

- **视角新颖**：用几何流（位置、速度、曲率）来描述 LLM 推理，为可解释性提供了一种直观、形式化的语言。
- **语义与逻辑解耦设计**：通过同一逻辑命题配不同语义载体，能够直接检验逻辑抽象能力，是一种干净的实验范式。
- **理论-实证结合**：既提出严格理论命题，又通过表征代理和受控实验进行验证。
- **跨架构验证**：在 Qwen 和 LLaMA 两种系列上测试，增强了结论的稳定性和普遍性。
- **反驳争论的意义**：其结果对“LLM 只是随机鹦鹉”的流派提供了系统性的反证证据，具有较大学术争议价值。

## 8. 不足与局限

- **实验覆盖有限**：仅使用自然演绎类逻辑命题，未涵盖真实世界复杂推理、多步常识推理或数学推理等复杂场景。
- **缺乏对比基线与消融**：未在提供信息中看到与先验几何方法或注意力可视化等方法的对比，也没有深入消融速度控制器机制的必要性和分量。
- **可复现性细节不足**：未报告数据规模、训练/微调方式、超参数以及评测标准，无法完全判断实验的公平性。
- **结果解释的风险**：将几何轨迹解释为“逻辑”可能带有循环论证色彩——如何证明某个几何曲率或速度模式在功能上确实对应逻辑规则，仍需要更因果化的干预实验。
- **普遍性的宣称过强**：“可能存在普遍表征定律”在仅两个模型家族上的证据尚不够充分。
- **算力与资源透明度低**：未公开 GPU 等资源信息，给后续研究者估计成本造成困难。

（完）
