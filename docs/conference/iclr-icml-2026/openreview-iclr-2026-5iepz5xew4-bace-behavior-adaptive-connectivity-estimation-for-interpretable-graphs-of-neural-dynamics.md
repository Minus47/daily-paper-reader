---
title: "BACE: Behavior-Adaptive Connectivity Estimation for Interpretable Graphs of Neural Dynamics"
title_zh: BACE：面向可解释神经动态图的行为自适应连接估计
authors: "Mehrnaz Asadi, Sina Javadzadeh, Rahil Soroushmojdehi, S. Alireza Seyyed Mousavi, Terence Sanger"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=5iepz5XeW4"
tags: ["query:eeg-align"]
score: 4.0
evidence: 建模行为条件下的脑区间连接以得到可解释网络图，未构造脑与行为的共享表征空间
tldr: 脑区间连接往往缺少行为上下文，难以同时做到可预测与可解释。BACE通过区域级时序编码器和行为自适应邻接矩阵，直接从多区域颅内场电位学习有向连接图并用于动态预测。在已知图结构的合成数据上，它能准确恢复真实连接，预测性能也与对比方法相当。这类行为条件化的连接模型为脑与行为关系的通用建模提供了可解释的图路径，但与脑电语义对齐任务关联较弱。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有脑连接建模缺乏行为上下文，难以在预测的同时得到可解释的脑区交互结构。
method: 利用区域级时序编码器聚合微接触信号，并为每个行为上下文学习可学习的有向邻接矩阵，以预测目标端到端训练。
result: 在合成多变量时间序列上准确恢复真实连接图，同时保持与对比方法可比的预测表现。
conclusion: 行为自适应连接估计能生成可解释的有向神经动态图，为脑与行为关系研究提供支持。
---

## Abstract
Understanding how distributed brain regions coordinate to produce behavior requires
models that are both predictive and interpretable. We introduce Behavior-
Adaptive Connectivity Estimation (BACE), an end-to-end framework that learns
context-specific, directed inter-regional connectivity directly from multi-region intracranial
local field potentials (LFP). BACE aggregates many micro-contacts
within each anatomical region via per-region temporal encoders, applies a learnable
adjacency specific to each behavioral context, and is trained on a forecasting
objective. On synthetic multivariate time series with known graphs, BACE accurately
recovers ground-truth directed interactions while achieving forecasting
performance comparable to state-of-the-art baselines. Applied to human subcortical
LFP recorded simultaneously from eight regions during a cued reaching task,
BACE yields an explicit 8×8 connectivity matrix for each within-trial behavioral
context. The resulting behavioral context-specific graphs reveal behavior-aligned reconfiguration
of inter-regional influence and provide compact, interpretable adjacency
matrices for comparing network organization across behavioral contexts. By
linking predictive success to explicit connectivity estimates, BACE offers a practical
tool for generating data-driven hypotheses about the dynamic coordination
of subcortical regions during behavior.

---

## 论文详细总结（自动生成）

# BACE：面向可解释神经动态图的行为自适应连接估计

## 1. 核心问题与整体含义

- 理解大脑分布式区域如何协同产生行为，需要同时具备**预测能力**和**可解释性**的模型。
- 传统脑连接建模往往**缺乏行为上下文**，难以在保持预测性能的同时揭示脑区间的交互结构。
- 论文指出需要一种能够生成“行为条件下的、有向的、可解释的脑区间连接图”的建模方法，以桥接神经动态预测与脑网络组织理解之间的鸿沟。
- 整体含义：将连接估计显式地构建为行为上下文的函数，可为脑—行为关系研究提供数据驱动的可解释图路径。

## 2. 方法论

- **核心思想**：提出行为自适应连接估计（Behavior-Adaptive Connectivity Estimation, BACE）框架，端到端地从多区域颅内局部场电位（LFP）中学习上下文特异的、有向的脑区间连接。
- **核心技术细节**：
  - 每个解剖脑区包含多个微接触（micro-contacts）信号，BACE 利用**区域级时序编码器**对各脑区内部的微接触信号进行聚合，得到区域级表征；
  - 针对每个行为上下文，模型学习一个**可学习的邻接矩阵**，表示该行为条件下的区域间有向连接强度；
  - 整个系统以**预测任务**（如时序预测 / forecasting）为目标进行端到端训练，即预测目标脑区的未来动态，从而约束连接矩阵既保证预测效果又具备可解释性。
- **算法流程**（文字说明）：
  1. 输入：多区域颅内 LFP（每个区域含多个微接触）；
  2. 区域级编码：每个区域独立经时序编码器将多通道微接触压缩为该区域的统一动态表征；
  3. 行为自适应连接：根据当前行为上下文选择或生成对应的有向邻接矩阵；
  4. 图传播与预测：利用学习到的连接结构对各区域表征进行信息聚合/传递，输出目标序列的未来预测；
  5. 以预测损失反向传播，驱动连接矩阵与编码器同步更新。

## 3. 实验设计

- **数据集 / 场景**：
  1. **合成多变量时间序列**：具有已知真实图结构，用于验证连接恢复的正确性（ground-truth directed interactions）；
  2. **人类皮层下 LFP 数据**：在一次线索引导的伸手任务（cued reaching task）中，从 **8 个脑区**同时记录得到。
- **Benchmark**：以预测性能为基准，对比方法为 state-of-the-art baseline；（文本中未具体列出 baseline 名称）
- **评估指标**：
  - 在合成数据上：连接结构恢复的准确性（是否能准确恢复真实有向连接）+ 预测性能；
  - 在真实数据上：得到显式的 8×8 行为上下文特异连接矩阵，观察其是否展示出行为对齐的脑区间影响重构模式，并评估其可解释性与跨情境可比性。

## 4. 资源与算力

- 文本信息中**未明确说明**使用的 GPU 型号、数量、训练时长或任何算力资源细节。
- 需要指出：原文没有对计算资源做任何披露，无法从文中评估其训练成本或可扩展性。

## 5. 实验数量与充分性

- 从文本可确认的实验场景有**两类**：一类是合成数据上的结构恢复与预测验证；另一类是真实人脑 LFP 数据上的行为相关图构建。
- **未提及**消融实验、不同 backbone 对比、超参数敏感性分析、多被试/多任务泛化实验等。
- 论文被标记为会议投稿被拒（ICLR 2026 Rejected），评分为 4.0，综述（tl;dr）指出其“与脑电语义对齐任务关联较弱”。
- **客观评价**：实验设计能够支撑“连接恢复 + 预测性能可共存”的核心论点，但覆盖范围较窄：
  - 真实数据仅涉及一个任务（伸手任务）与 8 个区域；
  - 缺少扰动分析、基线对比细节和统计显著性等验证，
  - 因此**实验充分性一般**，更接近于概念验证（proof-of-concept）而非系统性方法评估。

## 6. 主要结论与发现

- 在已知图结构的合成数据上，BACE 能**高准确率恢复真实有向交互**，同时预测性能与当前最优基线相当。
- 在人类皮层下 LFP 数据上，BACE 能为每个 trial 内行为上下文生成显式的 **8×8 连接矩阵**；
- 行为上下文特异的图揭示了**行为对齐的脑区间影响重构**（behavior-aligned reconfiguration of inter-regional influence）；
- 连接矩阵紧凑、可解释，可用于跨行为上下文的网络组织比较；
- 方法为生成关于行为过程中皮层下区域动态协调的**数据驱动假说**提供了实用工具。

## 7. 优点

- **端到端可训练**：连接估计直接由预测目标驱动，无需中间标注；
- **行为自适应**：能够刻画脑连接在不同行为条件下的动态变化，而不是给出固定连接图；
- **可解释性**：显式输出邻接矩阵，结果直观、可检查，有助于形成机制性假说；
- **多尺度聚合**：合理解决了一个脑区包含多个微接触的层级问题；
- **跨场景验证**：既在合成数据上验证了“能恢复真实连接”，又在真实神经数据上验证了“能产生有意义的行为相关图”。

## 8. 不足与局限

- **未建立共享语义表征**：模型是为预测而设计的，并没有构建脑信号与行为之间的共享表征空间，因此与“脑电-行为语义对齐”类任务的直接关联较弱（这也是文中标注的不足）；
- **验证范围有限**：真实数据仅涉及 8 个区域和单一伸手任务，是否泛化到其他任务、其他脑区组合或被试群体尚不明确；
- **对比方法不明**：文本未具体列出所比较的 SOTA baseline、评估协议和统计检验方法，削弱了可复核性；
- **缺少消融研究**：区域编码器结构、邻接矩阵的上下文条件化方式、预测窗口长度等关键设计选择没有系统验证；
- **可解释性深度不足**：连接矩阵虽直观，但对其神经生理学意义缺少外在验证或行为指标的相关性分析；
- **资源披露缺失**：未记录计算开销，限制了对该方法在大规模记录中的实用性的判断；
- **被会议拒稿，评审评分较低（4.0）**，提示其贡献的新颖性与实验严谨性仍有可改进的空间。

（完）
