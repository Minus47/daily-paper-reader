---
title: Generating Computational Cognitive models using Large Language Models
title_zh: 利用大型语言模型生成计算认知模型
authors: "Milena Rmus, Akshay Kumar Jagadish, Marvin Mathony, Tobias Ludwig, Eric Schulz"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QKICx7eSMJ"
tags: ["query:abstraction"]
score: 5.0
evidence: 用大语言模型生成计算认知模型，可辅助认知架构的自动化构建
tldr: 传统计算认知模型高度依赖研究者手工编写代码，过程耗时且门槛较高。该研究提出以大语言模型为中心的管线，利用其代码生成能力自动产出可拟合行为数据的计算认知模型。凭借LLM的跨领域知识与模式识别，该方案能显著降低建模成本，并支持在多理论之间进行定量仲裁。这为认知架构的自动化构建和理论检验提供了新工具。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 计算认知模型通常需手工构建，成本高且需领域知识，亟需自动化方法降低门槛。
method: 提出基于大语言模型的管线，利用其模式识别与代码生成能力自动产生认知模型代码并拟合行为数据。
result: 展示该方法能生成可用的计算认知模型，从而减少手工建模时间并支持理论比较。
conclusion: 大语言模型可显著降低计算认知模型的构建成本，为认知架构与理论验证提供自动化工具。
---

## Abstract
Computational cognitive models, which formalize theories of cognition, enable researchers to quantify cognitive processes and arbitrate between competing theories by fitting models to behavioral data. Traditionally, these models are handcrafted, which requires significant domain knowledge, coding expertise, and time investment. However, recent advances in machine learning offer solutions to these challenges. In particular, Large Language Models (LLMs) have demonstrated remarkable capabilities for in-context pattern recognition, leveraging knowledge from diverse domains to solve complex problems, and generating executable code that can be used to facilitate the generation of cognitive models. 
Building on this potential, we introduce a pipeline for Guided generation of Computational Cognitive Models (GeCCo). Given task instructions, participant data, and a template function, GeCCo prompts an LLM to propose candidate models, fits proposals to held-out data, and iteratively refines them based on feedback constructed from their predictive performance. We benchmark this approach across four different cognitive domains -- decision making, learning, planning, and memory -- using three open-source LLMs, spanning different model sizes, capacities, and families. On four human behavioral data sets, the LLM generated models that consistently matched or outperformed the best domain-specific models from the cognitive science literature. 
To validate these findings, we performed control experiments that investigated (1) the contribution of the different LLM features (model size, model family, capacities); (2) the causal role of different prompt components; (3) the effect of data contamination; (4) the ability to recover ground truth models from simulated data; and (5) the total explainable variance in human behavior captured by LLM-generated models. 
Taken together, our results suggest that LLMs can rapidly generate cognitive models with conceptually plausible theories that rival -- or even surpass -- the best models from the literature across diverse task domains.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义

- **研究背景**：计算认知模型将认知理论形式化，能够通过将模型拟合至行为数据来量化认知加工过程，并在竞争性理论之间进行仲裁。然而传统上这类模型完全依赖研究者手工编写代码，门槛极高。
- **核心问题**：构建计算认知模型需要深厚的领域知识、编程技能与大量时间投入，制约了认知建模方法的普及与规模化应用。
- **研究动机**：近年来机器学习尤其是大型语言模型（LLM）取得巨大进展——LLM可以利用跨领域模式识别与知识迁移来解决复杂任务，并能生成可执行代码。这为将LLM引入认知模型自动生成提供了契机。
- **整体含义**：该研究提出了一种以大语言模型为中心的自动化管线，有望将计算认知建模从“手工写代码”转化为“研究者给出任务指令、模型自动生成候选模型并迭代优化”，显著降低建模成本并推动理论与数据的高效循环验证。

### 2. 论文提出的方法论

- **方法名称**：GeCCo（Guided generation of Computational Cognitive Models，引导式计算认知模型生成管线）。
- **核心思想**：以LLM为核心生成引擎，利用其基于跨领域知识的模式识别和代码生成能力，将行为数据拟合到可运行的认知模型之中。
- **算法流程**（文字说明）：
  1. **输入构建**：将任务说明（task instructions）、参与者行为数据（participant data）以及一个模板函数（template function）输入LLM。
  2. **候选模型生成**：LLM依据输入提示（prompt）生成候选的计算认知模型代码。
  3. **拟合与筛选**：将生成的候选模型拟合到留出数据（held-out data）上，评估预测性能。
  4. **迭代优化**：系统根据预测性能构造反馈信号，将反馈信息交还给LLM，由LLM对候选模型进行修改与精化。该过程反复迭代直至满足终止条件。
- **与手工建模的区别**：研究者只需提供任务层面的描述和模板函数，而无需亲自编写完整的模型代码；模型生成逻辑交由LLM完成，同时以数据驱动的方式在多个竞争的理论模型之间进行定量选择。

### 3. 实验设计

- **认知领域覆盖**：四个不同的认知领域——决策（decision making）、学习（learning）、计划（planning）、记忆（memory）。
- **数据集与benchmark**：
  - 使用四套人类行为数据集（对应上述四个领域）。
  - 基准对比对象为认知科学文献中针对各领域表现最优的领域特异性模型（the best domain-specific models from the cognitive science literature）。
- **LLM对比**：使用三个开源LLM，它们分属不同模型家族、规模和能力档位，以考察方法的普适性。
- **控制实验/消融设计**：
  1. 检验不同LLM特征（模型大小、模型家族、能力）的贡献；
  2. 检验不同提示组件（prompt components）的因果作用；
  3. 检验数据污染（data contamination）的影响；
  4. 检验从模拟数据中恢复真实模型（ground truth models）的能力；
  5. 量化LLM生成模型所捕获的人类行为总可解释方差。

### 4. 资源与算力

- 从所提供论文文本看，文中**没有明确说明**使用了何种GPU型号、GPU数量以及训练/推理时长。
- 该研究主要使用的LLM为开源的、不同规模的预训练模型，推理过程可能不需要大规模训练算力，但文本未提供具体计算资源细节，因此无法量化其算力开销。

### 5. 实验数量与充分性

- **实验总量**：文本中可见的实验至少包括——四套人类行为数据集上的主实验，加上五个方向的系统控制实验（LLM特征贡献、提示组件因果作用、数据污染效应、真实模型恢复能力、可解释方差分析）。整体实验设计较为立体。
- **充分性与客观性**：
  - **优点**：跨了四个认知领域、三种不同家族与规模的LLM，且以文献中最优领域模型为baseline，具备较好的泛化性检验；控制实验涵盖了从模型输入选择、提示设计到数据安全、模型可解释性的多个维度，体现了较严谨的验证思路。
  - **不足**：由于文本为摘要级信息，无法获知每个实验内部的样本量、重复次数、具体消融条件设置、统计检验方式等细节，因此难以对其统计学功效和严格公平性做出最终判断。此外，三个开源模型是否具有代表性、是否与商业闭源最强模型进行对比，也是尚未在文本中体现的问题。

### 6. 论文的主要结论与发现

- LLM生成的计算认知模型在四个不同认知领域的人类行为数据上，始终能够**匹配甚至超越**认知科学文献中最优的领域特异性模型。
- GeCCo管线能够在较少人工干预的情况下，快速生成在概念上合理（conceptually plausible）的理论模型。
- 不同LLM特征（模型大小、家族、能力）对生成模型质量有影响，但即使较小规模的模型也能产出有竞争力的模型。
- 数据污染与控制实验表明，模型不仅存在记忆已有知识的能力，还具备从仿真数据中发现并恢复真实认知机制的能力（能够从模拟数据中恢复ground truth模型）。
- 总体上，LLM可以作为一种高效的自动化工具，加速认知建模的理论比较与构建过程。

### 7. 优点

- **方法新颖有实际价值**：提出了利用LLM自动生成计算认知模型的完整闭环管线，突破了传统手工建模的瓶颈，为认知科学提供新的自动化工具。
- **跨领域验证设计**：在决策、学习、计划、记忆四个相对独立的认知领域进行验证，增强了结论的适用范围。
- **系统而多样的对照实验**：不仅对LLM的大小、家族、能力进行消融，还研究提示组件、数据污染、真实模型恢复能力等因素，显示出较好的实验严谨性。
- **以文献最佳模型为基准**：不只是验证模型能运行，还检验其是否超过了领域内的既有最佳成就，标准较高。
- **开源LLM+开源管线的可行性**：证明了即使不依赖专有闭源模型，也能产生高质量结果，有较好的可复现潜力。

### 8. 不足与局限

- **细节信息缺失**：论文摘要之外未提供具体的数据集描述（如被试数量、实验任务细节）、提示模板内容、迭代过程指标和模型代码示例，限制了对其技术实现细节的评估与复制。
- **领域覆盖边界**：虽然覆盖四个认知领域，但认知科学范围远不止于此（如社会认知、语言加工、知觉决策、元认知等），仍需扩展证明更高普适性。
- **算力资源说明不足**：未报告模型推理所需的实际计算资源，导致无法评估其效率提升的"净收益"。
- **LLM已知偏差风险**：与前人讨论一致，LLM存在数据污染（训练语料与测试数据重叠）的风险；作者虽做了控制实验，但在摘要中未能完全说明控制手段的效力与残余偏差。
- **理论的可解释性与可演进性更需深入探索**：LLM生成模型的"概念合理性"有限——模型在数值上拟合很好，但认知科学理论的核心在于机制解释力、可证伪性与适用范围，这些方面需要进一步分析。
- **评估指标限制**：主要基于预测性能（预测保留数据的准确性）来判定模型优劣，而一个好的认知模型还应具有参数可识别性、简洁性（如BIC/AIC比较）与跨任务迁移能力，文中并未展示对这些维度的综合比较。

（完）
