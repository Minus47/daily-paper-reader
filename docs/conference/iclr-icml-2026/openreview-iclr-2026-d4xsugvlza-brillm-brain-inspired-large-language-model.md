---
title: "BriLLM: Brain-inspired Large Language Model"
title_zh: BriLLM：类脑大语言模型
authors: "hai zhao, Hongqiu Wu, Dongjie Yang, Anni Zou, Jiale Hong"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=D4xSuGvLZA"
tags: ["query:abstraction"]
score: 9.0
evidence: 提出基于神经科学事实的类脑大语言模型，直接对应类脑AI模型方向
tldr: 现有类脑AI大多模拟局部神经特征，缺少对大脑宏观信息处理原则的系统复现。作者提出BriLLM，以信号全连接流动学习首次在规模上复现大脑的静态语义映射与动态电生理传播这两个神经认知事实。模型获得多模态兼容、节点级可解释及上下文长度无关扩展等能力。该工作为脑启发语言模型及后续类脑概念空间研究奠定基础。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 已有类脑模型多只模仿局部神经特征，无法真正复现大脑宏观信息处理机制，需要新的学习范式。
method: 提出信号全连接流动(SiFu)学习，以静态语义映射和动态电生理传播为验证基础构建类脑大语言模型。
result: 模型实现多模态兼容、全节点可解释和上下文长度无关的扩展等能力。
conclusion: 证明了真实神经认知事实可赋能大型语言模型，为类脑认知架构提供可行路径。
---

## Abstract
We introduce BriLLM, the first brain-inspired large language model that establishes a genuinely biology- and neuroscience-grounded machine learning paradigm. Unlike previous approaches that primarily mimic local neural features, BriLLM implements Signal Fully-connected flowing (SiFu) learning—the first framework to authentically replicate the brain's macroscopic information processing principles at scale. Our approach is uniquely validated by two core neurocognitive facts: (1) _static semantic mapping_ to dedicated cortical regions, and (2) _dynamic signal propagation_ through electrophysiological activity. This foundation enables transformative capabilities: inherent multi-modal compatibility, full node-level interpretability, context-length independent scaling, and global-scale simulation of brain-like language processing. Our 1–2B parameter models demonstrate stable learning dynamics while replicating GPT-1-level generative performance. Scalability analysis confirms feasibility of 100–200B parameter variants. BriLLM represents a paradigm shift from representation learning toward biologically-validated AGI foundations, offering a principled solution to current AI's fundamental limitations.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：现有类脑AI研究大多仅模仿局部神经特征（如神经元连接或局部可塑性规则），未能在机器学习规模上系统复现大脑的宏观信息处理原则。这导致类脑模型缺乏真正的神经科学一致性，也无法解决当前AI在可解释性、多模态融合、扩展性与生物学有效性方面的根本局限。
- **研究意义**：该论文提出一项类脑大语言模型的奠基性尝试，试图将神经科学中关于大脑工作的两个核心事实——静态语义映射与动态电生理信号传播——作为机器学习建模的第一性原则，从而推动AI从纯表征学习转向“经生物学验证的AGI基础”。

### 2. 方法论：核心思想与技术路线
- **模型名称**：BriLLM（Brain-inspired Large Language Model）。
- **核心思想**：以**信号全连接流动（Signal Fully-connected flowing, SiFu）学习**为范式，在规模化层面真实复现大脑宏观信息处理原则。
- **两大神经认知事实作为建模基础**：
  - **静态语义映射（static semantic mapping）**：模拟大脑中语义信息被映射到特定皮层区域的静态结构特性。
  - **动态信号传播（dynamic signal propagation）**：模拟大脑神经电生理活动的动态传导过程。
- **技术要点（文字性说明）**：
  - 不同于传统Transformer的固定前向与注意力计算，SiFu设计了一种使信号在模型各节点之间“全连接流动”的信息处理方式，以接近大脑活动模式。
  - 这种类脑架构使模型能力不是通过堆叠上下文窗口或增大表示维度获得的，而是源于生物学合理的信息逻辑。
- **架构与规模**：论文提出1–2B参数规模模型，并给出100–200B参数规模可扩展性分析。

### 3. 实验设计
- 由于该论文当前获取到的文本仅包含标题页和摘要（原始PDF入口受到OpenReview验证页限制，未能获得完整正文），**无法获得具体的实验细节**。
- 根据摘要及元数据信息，可确认以下实验方面的粗略信息：
  - **生成能力评估**：复现了GPT-1级别生成性能，说明会与GPT-1或同级别生成模型进行对照评估。
  - **能力维度测试**：探索了多模态兼容性、节点级可解释性、上下文长度无关扩展、大规模脑样语言处理模拟等指标。
  - **扩展性验证**：提供了从小规模（1–2B模型）到100–200B模型的扩展可行性分析。
  - **训练稳定性**：提到1–2B模型呈现稳定学习动态。
- 论文正文中使用的具体数据集、benchmark、对比基线（除GPT-1风格生成对比外）、消融实验设计均无法在此版本中得到确认。

### 4. 资源与算力
- 在可提取信息范围内，论文**未明确说明使用的GPU型号、数量、训练时长**，也没有给出具体的算力成本与能耗信息。
- 若需要了解资源占用，需查阅完整论文正文或补充材料；目前这份材料无法支持该部分的精确回答。

### 5. 实验数量与充分性
- 从现有材料看，实验覆盖的维度较广：包括生成性能、多模态兼容性、可解释性、扩展性和训练稳定性等。
- 但是否做过多组数据集实验、是否进行充分的消融实验、与SOTA基线对比是否公平等问题，**无法从当前截取内容中评判**。
- 可能存在的不足是：单凭摘要提到“复现GPT-1级生成性能”，不能充分证明该方法优于当前主流大模型——作为类脑模型，需要在更多基准上展示价值。

### 6. 主要结论与发现
- BriLLM是首个同时复现大脑静态语义映射和动态电生理传播的大规模类脑大语言模型。
- SiFu学习范式能够带来传统模型不具备的四个关键能力：
  1. **固有（inherent）多模态兼容性**；
  2. **全节点级别可解释性**；
  3. **上下文长度无关的扩展能力**（对比传统模型受限于上下文长度的问题）；
  4. **对脑样语言处理过程的全局模拟**。
- 可扩展性分析证明参数量可推至100–200B级别，显示该方法的规模潜力。
- 整体上证明“真实神经认知事实可以赋能LLM”，为未来类脑认知架构提供了可行路线。

### 7. 优点（亮点）
- **神经科学依据强**：不是单纯仿脑形，而是基于两个具体的、可验证的宏观脑认知事实来构建学习规则，科学动机清晰且具有独特性。
- **范式创新度高**：提出了 SiFu 这种与现有Transformer前向计算完全不同的信息流动范式，跳出了“表象学脑”的研究路径。
- **系统性能力突破**：一个架构同时覆盖多模态、可解释性和上下文长度不敏感扩展三大当前AI痛点，价值覆盖面较广。
- **边界意识良好**：用现有小规模模型验证可行性的同时给出了大规模扩展预测，在一定程度上考虑了类脑模型能否scale up这一现实问题。

### 8. 不足与局限（基于可获取材料的判断）
- **内容获取受限**：本次仅能基于摘要与元数据分析，无法核实方法细节、公式、数据集、消融方法与基准对比，故不能对其实验做全面、客观评估。
- **生成能力基准偏低**：所给出的验证上限为GPT-1级生成性能，远低于现代主流语言模型水平；即使该模型并非以生成性能为唯一目标，仍需证明它在更实际的下游任务中的有效性。
- **生物学合理性与计算可行性的平衡尚需说明**：模拟大规模电生理传播的计算复杂度需要具体分析，论文未显示此类成本与收益比较。
- **多模态兼容性缺少实证说明**：“固有兼容”只是一种设计主张，缺乏实际多模态实验支撑则难以断言。
- **可解释性验证不够具体**：“节点级可解释”如何度量？与哪些类脑模型进行比较？需要更多信息。
- **若类脑模型训练数据效率与损失收敛行为与标准LLM不同，则需更多讨论其在不同任务上的泛化风险；当前材料未覆盖这些方面。**

> 注：以上分析基于OpenReview拦截页面所捕获的标题、元数据和摘要。若需要更细致的实验客观性评价，建议获取经过CAPTCHA后的完整论文正文。

（完）
