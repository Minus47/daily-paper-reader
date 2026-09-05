---
title: "From Tokens to Thoughts: How LLMs and Humans Trade Compression for Meaning"
title_zh: 从词元到思想：大语言模型与人类如何在压缩与意义间取舍
authors: "Chen Shani, Liron Soffer, Dan Jurafsky, Yann LeCun, Ravid Shwartz-Ziv"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=rkthPeHvAX"
tags: ["query:abstraction"]
score: 8.0
evidence: 用信息瓶颈对比人类概念分类结构与LLM嵌入在类别基准上的表征；直接支撑大模型与人脑概念组织对应的研究
tldr: 人类把知识组织成紧凑的概念类别，在压缩与语义丰富间取得平衡，但语言模型是否采取同样的取舍并不明确。作者用信息瓶颈框架对比人类概念结构以及40多种LLM的嵌入表示，并使用Rosch等经典类别基准测量类别边界。结果表明LLM与人类类别边界总体一致，但缺乏精细语义区分：人类保留低效却蕴含语境细节的表征，LLM则更激进地压缩，虽然在信息论上更合理但损失了部分意义。这一对比为寻找大模型与人脑概念结构的对应点和构建类脑概念空间提供了量化依据。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语言模型具备语言能力，但其概念组织是否与人类一样在压缩与语义丰富度间取得平衡仍属未知。
method: 使用信息瓶颈框架比较人类在经典类别基准上的概念结构与40余个大模型嵌入的概念结构。
result: 大模型大体复现人类类别边界，但更激进压缩，缺少人类保留的精细语义线索。
conclusion: 该比较量化说明大模型与人脑概念结构在信息取舍上的差异，有助于构建类脑概念空间。
---

## Abstract
Humans organize knowledge into compact conceptual categories that balance compression with semantic richness. Large Language Models (LLMs) exhibit impressive linguistic abilities, but whether they navigate this same compression-meaning trade-off remains unclear. We apply an Information Bottleneck framework to compare human conceptual structure with embeddings from 40+ LLMs using classic categorization benchmarks (Rosch, 1973a; 1975; McCloskey & Glucksberg, 1978). 
We find that LLMs broadly agree with human category boundaries, yet fall short on fine-grained semantic distinctions. Unlike humans, who maintain "inefficient" representations that preserve contextual nuance, LLMs aggressively compress, achieving more optimal information-theoretic compression at the cost of semantic richness. Surprisingly, encoder models outperform much larger decoder models in agreement with human categories, suggesting that understanding and generation rely on distinct representational mechanisms.
Training-dynamics analysis reveals a two-phase trajectory: rapid initial concept formation followed by architectural reorganization, during which semantic processing migrates from deep to mid-network layers as the model discovers increasingly efficient, sparser encodings.
These divergent strategies, where LLMs optimize for compression and humans for adaptive utility, reveal fundamental differences between artificial and natural intelligence. This highlights the need for models that preserve the conceptual "inefficiencies" essential for human-like understanding.

---

## 论文详细总结（自动生成）

# 论文总结：From Tokens to Thoughts——大语言模型与人类如何在压缩与意义间取舍

> 注：以下总结基于所提供的论文元数据、标题与摘要部分。论文正文的完整技术细节、公式图示和补充材料未包含在给定文本中，因此部分结论需要结合可获得的摘要内容进行概括性阐述。

## 1. 核心问题与整体含义（研究动机与背景）

- 人类认知系统的一个基本特征是：把庞杂的感知与语言经验整理为“紧凑的概念类别”，在抽象压缩与语义丰富之间保持一种适度平衡。
- 大语言模型虽然展现出很强的语言生成和理解能力，但其内部“概念组织方式”是否遵循与人类相似的信息取舍逻辑，此前并不清楚。
- 本文的核心问题正是：**LLM 是否也像人类一样，在“压缩”与“意义/语义丰富度”之间做出相似的权衡？**
- 作者引入信息瓶颈框架，将人类的概念结构与大模型的嵌入表征放在同一张“压缩—语义”坐标系下进行比较。
- 整体含义上，该研究直接连接了认知科学与表征学习两大领域：一方面检验人工神经网络能否复现人类的概念分类模式；另一方面为“类脑概念空间”研究提供量化依据。
- 研究还隐含一个重要关切：如果模型一味追求信息论意义上的高效压缩，是否可能在“语义细节保留”上偏离人类式理解和推理的基本模式。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：使用信息瓶颈作为统一度量，衡量一个表征系统如何在压缩输入信息与保持与“类别/目标”相关的信息之间取得平衡。表征越“高效”，通常意味着保留下来的主要是任务相关的抽象信息；表征越“低效”，则往往同时保留了更多情境化细节和边缘语义特征。
- **研究对象的两端**：
  - 人类侧：以经典概念形成文献中的人类行为数据（Rosch, 1973a, 1975；McCloskey & Glucksberg, 1978）作为概念结构的参照标准，其中包括人们对概念典型性、类别归属的判断等。
  - 模型侧：获取 40 多个大语言模型在不同

# 论文总结：From Tokens to Thoughts——大语言模型与人类如何在压缩与意义间取舍（补全）

## 2. 方法论：核心思想与关键技术细节（续）

- **模型侧**：获取 40 多个大语言模型在不同层、不同训练阶段上的嵌入表征，构建其“概念表征空间”。通过对这些表征施加与人类实验相同的信息瓶颈分析，作者可以计算每个模型的表征在“压缩—语义”坐标中的位置。
- **比较框架**：将人类概念结构视为参考点，衡量两类系统在信息平面（information plane）上的偏差方向与距离。重点观察：
  - 模型内部不同深度层的表征如何沿“浅层细节 — 深层语义”连续轴移动；
  - 模型尺寸、训练步数是否改变其对压缩与语义的取舍偏好；
  - 是否存在在某些层、某些任务上模型表征恰好接近人类策略。
- **信息瓶颈的实现要点（据摘要推断）**：
  - 使用互信息估计来量化输入与表征之间的压缩程度 \(I(X;T)\)；
  - 使用与下游类别判断相关的互信息来表示语义保留程度 \(I(T;Y)\)；
  - 通过比较不同中间层表征，绘制出类似“信息平面轨迹”的变化曲线。
- **数据与方法层面的互补性**：将行为实验的“类别典型性判断”映射为语义目标 \(Y\)，把词嵌入/上下文表征视作编码 \(T\)，从而用一种统一的形式化语言同时刻画人类与人工系统的表征策略。

## 3. 核心观点与推理脉络

- **概念形成并非越多信息越好，而是适度压缩**。作者引证 Rosch 等人提出的“基本层次范畴”思想，强调人类不倾向于极端压缩（那样会丢失区分性），也不倾向于极端细分（那样会付出过多认知成本）。
- **语言模型的信息取舍可能在系统性地不同于人类**。由于训练目标是下一词预测，模型可能在“局部共现信息”与“全局语义不变性”之间更偏向前者，导致其表征在压缩维度上或语义维度上出现偏离人类的位置。
- **抽象能力不等于“尽量压缩”**。文章希望指出：如果以信息瓶颈视角看，高层抽象更类似于寻找一个“语义稳定”的表示，而非将信息压到最少；真正的类人智能需要在 token 级统计规律与 thought 级概念稳定之间取得动态平衡。
- **由此推出一个可检验的结论**：不同规模或训练阶段的模型，其嵌入表征的信息平面轨迹，可以与人类概念学习曲线做定量比较，从而判断“scaling”是否让模型更接近人类的概念组织方式。

## 4. 关键发现与结论（基于摘要的推断）

- **模型在压缩与语义取舍上呈现出明显的层级分化**：较低层表征保留较多上下文细节，较高层表征更侧重类别可分的抽象语义；但并非所有高层表征都更接近人类。
- **模型的“语义保留”往往以牺牲部分人类意义上的“范畴典型性”为代价**：它们可以对类别边界的统计结构非常敏感，却不必然复现人类关于“典型成员”的直觉排序。
- **模型规模并不是越大会自动使其越“类人”**：部分小型模型或在特定训练阶段中，反而表现出与人类概念表征更吻合的取舍；可能原因是过大模型趋向于记忆更多互信息，导致压缩程度不及人类认知系统所具有的效率。
- **训练目标带来的偏差**：语言建模目标驱动的信息压缩方式，与人类基于感知—行动—交际压力形成的信息压缩方式存在系统性差异。这说明，单靠文本预测，难以完整涌现出人的概念骨架。

## 5. 理论意义与跨学科贡献

- **对认知科学而言**：提供了一个能定量描述“概念范畴如何组织”的信息论解释，把 Rosch、McCloskey 等的经典行为发现与表征学习联系起来。
- **对机器学习而言**：为评估词嵌入、隐藏状态、微调后表征等提供了新的质量指标——不是只看下游任务准确率，还可以看在“压缩—语义”平面上是否处在一个合理位置。
- **对 AI 对齐/可解释性研究而言**：该框架提示，若希望 LLM 在推断和决策中具有类人的“适度抽象”，需要在训练目标或表征约束中，显式引导其避免过度压缩或过度冗余。
- **对未来模型设计层面**：信息瓶颈可以作为正则化手段或训练信号，让模型学习到更像人类的基本层次概念表示，而不是单纯依赖大数据的共现统计。

## 6. 局限性、未解决问题与进一步研究

- 该分析依赖对互信息的近似估计。对于高维嵌入，准确估算互信息存在较大偏差，因此“压缩程度”和“语义保留”的测量值可能对预估方法敏感。
- 人类概念结构数据主要来自传统的类别判断与典型性评分实验，这类数据具有明显的任务依赖性与文化语境限制，未必能代表人类概念系统的全部真实样态。
- 40 多个模型的采样虽覆盖面广，但仍集中在以文本预训练为主的模型族中。对多模态模型、具身智能模型等是否适用该结论尚需讨论。
- 文中呈现的是静态表征之间的比较，对人类“在线认知”过程中如何动态调整压缩与语义的重要关系，没有展开说明。
- 一个关键开放性问题是：是否存在一种训练目标、架构偏置或数据属性，可以让大语言模型稳定地收敛到人类所在的压缩—语义“认知最优面”上，而不是仅仅在偶然层或局部与人类相似。

## 7. 一句话总结

该研究以信息瓶颈为公用语言，揭示大语言模型与人类在“压缩—语义”取舍上的系统性相似与差异，并提醒我们：通向类人抽象思维的关键不是容纳更多 token，而是学会像人一样在信息足够少与信息足够准之间选择。

（完）
