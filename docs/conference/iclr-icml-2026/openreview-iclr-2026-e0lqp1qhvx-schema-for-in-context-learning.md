---
title: Schema for In-Context Learning
title_zh: 上下文学习中的图式
authors: "Pan Chen, Shaohong Chen, Mark Wang, Shi Xuan Leong, Priscilla Fung, Varinia Bernales, Alan Aspuru-Guzik"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=e0lqp1QhVX"
tags: ["query:abstraction"]
score: 7.0
evidence: 以认知图式理论从示例中抽取抽象推理模板，实现抽象级的认知迁移
tldr: 传统上下文学习只把示例用于任务层面的模式匹配，缺少抽象知识抽取与迁移。基于认知科学的图式理论，本工作提出图式激活的上下文学习(SA-ICL)，先将示例中关键推理步骤及其关系压缩为轻量结构化图式，再在推理时激活该图式引导新任务。该方法把记忆中的认知积木显式化为抽象模板，使模型不再依赖表面示例。这一认知启发范式有望增强大模型在复杂任务上的抽象归纳与迁移能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 上下文学习停留在示例匹配层面，缺少对已有经验的抽象结构化复用。
method: 从先验示例中抽取推理积木并组织成轻量图式模板，在推理时激活图式完成知识迁移。
result: 通过抽象图式激活为上下文学习提供显式的抽象级推理结构，提升了任务泛化潜力。
conclusion: 将图式理论引入ICL可为语言模型建立可复用的抽象认知机制。
---

## Abstract
In-Context Learning (ICL) enables transformer-based language models to adapt to new tasks by conditioning on demonstration examples. However, traditional example-driven in-context learning lacks explicit modules for knowledge retrieval and transfer at the abstraction level. Inspired by cognitive science, specifically schema theory, which holds that humans interpret new information by activating pre-existing mental frameworks (schemas) to structure understanding, we introduce SCHEMA-ACTIVATED IN-CONTEXT LEARNING (SA-ICL). This proposed framework extracts the representation of the Building Blocks of Cognition for the reasoning process instilled from prior examples, creating an abstracted schema — a lightweight, structured template of key inferential steps and their relationships — which is then used to augment a model’s reasoning process when presented with a novel question. We demonstrate that a broad range of large language models (LLMs) lack the capacity to form and utilize internal schema-based learning representations implicitly, but instead benefit significantly from explicit schema-based scaffolding. Across chemistry and physics questions from GPQA dataset, our empirical experiment results show that SA-ICL consistently boosts performance (up to 36.19%) when the single demonstration example is of high quality, which simultaneously reduces reliance on the number of demonstrations and enhances interpretability. SCHEMA-ACTIVATED IN-CONTEXT LEARNING not only bridges disparate ICL strategies ranging from pattern priming to Chain-of-Thought (CoT) prompting, but also paves a new path for enhancing human-like reasoning in LLMs.

---

## 论文详细总结（自动生成）

# 论文总结：Schema for In-Context Learning（SA-ICL）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：基于 Transformer 的大语言模型能够通过上下文学习（In-Context Learning, ICL）快速适应新任务，即直接在提示中给出若干演示示例，让模型从中"悟"出任务规律。
- **核心问题**：现有的示例驱动型 ICL 本质上仍停留在**任务层面的模式匹配**，缺乏显式的**抽象知识抽取与迁移机制**——模型把示例当作表面输入来模仿，而不是从中提炼出可复用的推理框架。
- **认知科学启发**：人类在面对新信息时，会激活大脑中已有的"图式"（Schema），也就是预先存在的结构化知识框架，用它来组织对新的、陌生信息的理解。
- **核心论点**：大模型无法在隐式层面自发形成并利用图式化的抽象表征；但若**显式地提供图式作为推理支架**，模型性能可获得显著提升。
- **整体意义**：将认知科学中的图式理论引入 ICL，为 LLM 建立了一种更接近人类认知机制的、可复用的抽象推理结构，连接了从模式启动（pattern priming）到思维链（Chain-of-Thought, CoT）等多种此前看似不相关的 ICL 策略。

## 2. 提出的方法论：SA-ICL（Schema-Activated In-Context Learning）

- **核心思想**：不再让模型仅仅"看到"示例，而是先从示例中提炼出一张**抽象的推理模板（图式）**，再在遇到新问题时激活该图式来引导推理。
- **关键技术细节**：
  - **图式抽取（前序阶段）**：对先验示例中蕴含的推理过程进行分析，提取出其中关键的"认知积木"（Building Blocks of Cognition）——即关键推理步骤及其相互之间的逻辑关系。
  - **图式表示**：将上述认知积木组织成一个**轻量级、结构化的模板**（abstracted schema），它描述的是"这类问题应该怎样一步步想"，而非某个具体题目的答案。
  - **图式激活（推理阶段）**：给定一个全新的问题时，将该图式作为显式的结构提示注入模型的推理过程，引导模型按图式给出的抽象步骤进行演绎和求解。
- **文中未提供明确的公式或伪代码**；整体流程可用文字概括为：
  `先验示例 → 抽取关键推理步骤及其关系（认知积木） → 压缩为轻量结构化图式 → 注入新问题提示 → 模型按图式引导完成推理与回答`
- **方法定位**：SA-ICL 不是简单地增加示例数量，而是提升每个示例的"抽象利用率"，用显式的结构化知识替代对表面示例的依赖。

## 3. 实验设计

- **数据集（Benchmark）**：采用 **GPQA 数据集**中的**化学（Chemistry）与物理（Physics）** 两类科学问答题目——这两类题目属于需要专业知识与多步推理的困难任务。
- **任务设置**：采用**单演示示例（single demonstration example）** 的上下文学习设置，考察在极少示例条件下模型能否借助图式获得增益。
- **对比方法**：未在摘要中列出完整的 baseline 清单，但根据上下文可推断其对比范围涵盖：
  - 标准 ICL（不加图式、直接给示例）；
  - 不同形式的提示策略（包括模式启动类方法与 CoT prompting 等）。
- **主要实验结果**：
  - 在"单个演示示例质量较高"的情况下，SA-ICL 相对基线**最高提升 36.19%**；
  - 性能提升在**多种不同的大语言模型**上均得到体现（说明该框架具有跨模型泛化性）。

## 4. 资源与算力

- **文中未明确说明**：GPU 型号、GPU 数量、训练/推理时长、显存消耗等算力信息，在所提供的摘要文本中**完全未提及**。
- 需要指出的是：该方法的图式抽取与推理阶段均基于现成 LLM 完成，未涉及大规模预训练，实际算力开销可能远低于全量微调；但这一推断尚无原文数据支撑。

## 5. 实验数量与充分性

- **实验组数**：从摘要透露的信息看，至少涉及：
  - 两个学科领域（化学与物理）；
  - 多个不同的大语言模型架构；
  - 不同演示示例质量（高质量 vs. 低质量）的对比。
- **充分性评估（客观而言）**：
  - **优势**：跨模型验证增强了结论的稳健性；选择 GPQA 这类高难度科学问答作为测试床，具有一定区分度。
  - **不足**：作为一篇方法学论文，摘要中未报告消融实验的细节（如不同图式表示形式、不同抽取方式、图式长度的影响等），也未比较该方法与其他结构化提示方法（如 CoT、least-to-most）的系统性差异。仅凭摘要信息，**实验的全面性难以充分确认**。
  - **公平性风险**：需要警惕图式注入是否变相增加了推理步数（token 开销），与基线是否在等价推理预算下比较，摘要中未予说明。

## 6. 主要结论与发现

- **核心结论一（能力缺口）**：大语言模型普遍**缺乏在隐式层面自发形成并利用内部图式化学习表征的能力**——仅仅给示例，模型不会自动完成抽象层面的知识组织。
- **核心结论二（显式支架有效）**：当通过 SA-ICL **显式提供图式化的推理脚手架**时，模型在 GPQA 化学与物理任务上的表现获得**一致性提升（最高达 36.19%）**。
- **核心结论三（降低示例依赖）**：SA-ICL 在**单示例、高质量演示**的条件下即可取得显著增益，减少了对演示示例数量的依赖。
- **核心结论四（可解释性增强）**：显式图式使模型的推理依据可被检查和理解，提升了推理过程的可解释性。
- **理论意义**：SA-ICL 为从"模式启动"到"思维链提示"等不同 ICL 策略提供了一个统一的认知解释框架——这些策略都可视为在不同抽象层级上激活或构建图式。

## 7. 优点

1. **理论创新的跨学科性**：将认知科学中成熟的图式理论引入 LLM 上下文学习，理论根基扎实且视角新颖。
2. **轻量高效**：图式是"轻量级、结构化"模板，不需要重训练模型，符合大模型高效适配的趋势。
3. **认知机制显式化**：把记忆中的认知积木显式外化为可操作的推理结构，克服了传统 ICL 只模仿表面模式的根本缺陷。
4. **降低示例依赖**：在少示例（单示例）场景下就能获得大幅提升，具有显著的实际应用价值，尤其适用于标注数据稀缺的领域（如科学问答）。
5. **框架整合力强**：打通了不同 ICL 策略之间的联系，为后续研究提供统一的分析视角。
6. **提升可解释性**：抽象推理模板的注入让模型的决策路径更加透明。

## 8. 不足与局限

1. **示例抽象的信息损耗风险**：将示例压缩为图式模板的过程可能丢失关键细粒度信息；对复杂到无法简洁模板化的任务，该方法可能力不从心。
2. **实验覆盖范围有限**：仅在 GPQA 的科学问答（化学、物理）上验证，未覆盖代码、数学推理、开放域对话、多跳问答等更广泛的任务类型，通用性证据不足。
3. **对示例质量高度敏感**：文中明确指出提升出现在"演示示例质量较高"时；若示例质量不高，抽取出的图式可能具有误导性，甚至造成性能下降，这一风险未在摘要中充分讨论。
4. **缺少与现有强方法的系统对比细节**：未在摘要中说明与 CoT、Few-shot CoT 等方法的逐项对比，其增益是互补性还是替代性尚不清楚。
5. **推理开销问题未讨论**：图式的抽取与注入会增加额外的前置计算成本和提示长度，在长上下文或大规模任务中的性价比有待考察。
6. **尚无消融与鲁棒性分析披露**：文中未报告对图式设计不同选择（如抽取方式、图式粒度、注入位置）的敏感性分析，方法的稳健性有待更多实验支撑。
7. **认知图式与 LLM 机制的对应关系需要进一步验证**：虽然论文以图式理论为类比框架，但"图式激活"在 transformer 内部是否真的对应某种可复现的机制，仍需要更多机制层面的分析。

（完）
