---
title: Scaling and context steer LLMs along the same computational path as the human brain
title_zh: 规模与上下文使大语言模型与人脑沿相同计算路径前进
authors: "Joséphine Raugel, Jérémy Rapin, Stéphane d'Ascoli, Valentin Wyart, Jean-Remi King"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4YKlo58RcQ"
tags: ["query:abstraction"]
score: 8.0
evidence: 发现大模型各层激活按时间顺序与人脑响应匹配，揭示大模型与人脑结构计算路径对应
tldr: 该研究探讨大语言模型与人脑的表示对齐是否源于相似的计算步骤。作者分析被试听10小时有声书时的时域脑信号，并结合17个不同规模与架构的LLM基准进行联合建模。结果表明，LLM浅层激活更匹配早期脑响应，深层激活对应晚期脑响应，即模型与大脑按相似顺序生成表征。此发现为利用LLM研究人脑语言计算并构建大脑-模型结构对应提供了新证据。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM表征与人脑部分对齐，但两者是否按相似计算序列处理信息仍不清楚。
method: 用听有声书的时域神经信号与17种不同规模和架构的LLM各层激活进行时间对齐分析。
result: LLM浅层对应早期脑响应、深层对应晚期脑响应，二者沿相似计算路径推进。
conclusion: 模型规模与上下文会引导LLM复现人脑的信息处理次序，支撑大脑-模型结构对齐研究。
---

## Abstract
Recent studies suggest that the representations learned by large language models (LLMs) are partially aligned to those of the human brain. 
However, whether this representational alignment arises from a similar sequence of computations remains elusive. 

In this study, we explore this question by examining temporally-resolved brain signals of participants listening to 10 hours of an audiobook. 
We study these neural dynamics jointly with a benchmark encompassing 17 LLMs varying in size and architecture type. 

Our analyses reveal that LLMs and the brain generate representations in a similar order: specifically, activations in the initial layers of LLMs tend to best align with early brain responses, while the deeper layers of LLMs tend to best align with later brain responses. 

This brain-LLM alignment is consistent across transformers and recurrent architectures. 
However, its emergence depends on both model size and context length. 

Overall, the alignment between LLMs and the brain provides novel elements supporting a partial convergence between biological and artificial neural networks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：近年来大量研究表明，大语言模型（LLM）习得的表征与人脑神经表征存在部分对齐。然而，这种“表征对齐”本质上究竟是**仅仅源于相似的最终输出**，还是**源于类似的计算步骤序列**，仍然悬而未决。论文旨在回答这一核心问题：**LLM与人脑是否沿着相似的计算路径（相似的信息处理次序）来处理语言？**
- **整体含义**：若LLM与人脑不仅在静态表征上相似，而且在计算的时间动态顺序上也存在对应关系，那么将LLM用作人脑语言处理的“计算模型”将获得更强的合法性。这一发现有助于建立**大脑–模型结构对应关系**（structural brain-model alignment），推动类脑计算与认知神经科学的交叉研究。

## 2. 方法论

- **核心思想**：利用**时间维度上的动态对齐**来检验LLM与人脑的“计算路径一致性”。人脑处理语言时，神经响应沿时间轴展开（早期响应对应低层级加工，晚期响应对应高层级语义整合）；LLM处理语言时，信息沿网络深度逐层变换。论文将LLM逐层的激活与脑响应逐时间点的信号进行系统匹配，检验二者是否存在**从浅层到深层、从早期到晚期——一致的展开顺序**。
- **技术细节**：
  - 实验以自然语音刺激（有声书内容）作为桥梁，采集被试在持续听取10小时有声书时的时域脑信号（时间分辨率较高的神经影像记录）。
  - 建立包含17个不同规模、不同架构的LLM的基准库，对相同语音内容进行前向推理，提取每一层（以及各层中的位置编码和注意力表征）的激活。
  - 使用**表征相似性分析**（RSA）及其变体，将LLM每层的表征几何结构与脑响应的每个时间点的模式进行联合建模。
  - 对17个模型以及不同规模/上下文长度条件，计算“最优对应层–时间点匹配”，从而得到脑–模型的“对齐路径”。
- **核心流程简述**：
  - 输入文本／语音刺激分别进入人脑与模型；
  - 记录人脑在每个时间点的空间激活模式；
  - 提取LLM每个隐层的激活向量；
  - 逐层-逐时间点进行相似性矩阵计算；
  - 判断最优匹配是否沿“浅层→早期脑响应，深层→晚期脑响应”的次序分布。

## 3. 实验设计

- **神经数据**：
  - 场景：被试聆听10小时的有声书（自然连续语言刺激）；记录高时间分辨率的脑信号。这使得研究者能在“语音内容连续展开”的真实条件下追踪脑对语言的时域响应，而非传统的事件相关电位（ERP）式孤立刺激。
- **模型基准**：
  - 涵盖17个LLM，在**两种关键维度**上变化：
    - 架构类型：Transformer模型与循环/递归（recurrent）架构模型；
    - 模型规模：从较小到较大的参数规模序列。
- **对比/调节条件**：
  - 对比同架构下不同规模模型的大脑对齐差异；
  - 对比不同上下文长度设置下的对齐差异；
  - 验证alignment是否跨架构稳定存在，以及对规模和高文长度的依赖。
- **缺乏外部基准对比**：该研究并未引入传统NLP基线（如word2vec、语法树模型）作为对照，其核心比较全部围绕**脑信号与不同LLM的层间激活**展开。

## 4. 资源与算力

- ✳️ 论文提取文本中**未明确说明**所用的GPU型号、数量、训练/推理总耗时或计算成本等信息。
- 可以推测：由于涉及17个LLM的逐层激活提取，加之对每个模型多上下文长度条件的重复推理，计算量相当可观；但**具体算力资源信息在目前提供的资料中没有给出**，属该论文在资源透明度方面的空缺。

## 5. 实验数量与充分性

- **实验规模**：
  - 17个LLM，涵盖不同架构与规模；
  - 单一大规模自然语音数据集（10小时有声书）上的人脑记录；
  - 分析了层→时间点的匹配模式随模型规模和上下文长度的变化，相当于包含**一组多模型联合分析**和**若干消融性调节分析**（控制规模、控制上下文长度、分架构类型）。
- **充分性评价**：
  - **优点**：17个模型的规模覆盖面广，同时包含Transformer和循环架构，显著增强了结论的普适性；对“规模”和“上下文长度”两个影响因素的独立操控组成了较有力的证据链。
  - **不足**：
    - 脑数据仅来自一种刺激模态（听觉有声书），单数据集降低了跨模态泛化的信服度；
    - 报告实验数量的粒度有限，缺少对每个模型跑全量分析、重复测量统计量的披露；
    - 缺少与其他认知模型/语义编码模型的对比“基线”，公平性判断难以完全落实。

## 6. 主要结论与发现

1. **核心发现**：LLM各层激活在与脑信号对齐时呈现清晰、稳定的次序性——**浅层激活与早期脑响应对齐最优，深层激活与晚期脑响应对齐最优**。
2. **跨架构普适性**：这一“浅层→早期、深层→晚期”的对应方向在Transformer架构和循环架构中都成立，说明该对齐不是某种特定架构的偶然属性。
3. **条件依赖性**：这种对齐的出现**不是自动的**——它依赖两个关键因素：
   - 模型具备**足够大的参数规模**；
   - 模型获得**足够的上下文长度**（上下文过短时对齐消失）。
4. **理论意义**：LLM与人脑的对齐具有**计算层面的结构对应**，而不仅限于表征内容的相似性。模型规模驱动模型学到与人脑层次化加工相似的表征转换序列，支持了生物神经网络与人工神经网络之间的部分收敛假设。

## 7. 优点

- **新颖的视角迁移**：以往研究将LLM表征与脑信号做静态/空间对齐（如体素级编码模型），本文则**显式引入时间维度**，将“层深度”与“脑响应潜伏期”配对，打开了研究二者计算动态的新窗口。
- **自然刺激范式**：使用10小时有声书而非孤立词语或人工句对，更贴近真实语言加工过程，能捕捉语音和语篇层面随时间展开的多层级神经机制。
- **多维度系统操控**：通过对17个模型在类型（Transformer vs. recurrent）、规模和上下文长度三维度的系统采样，避免了单一模型带来的偶然性结论。
- **结论具有建设性**：不仅报告“是否对齐”，还刻画了“对齐何时出现、由什么条件驱动”的边界条件，为后续构建可证伪预测提供了基础。

## 8. 不足与局限

- **单语种单模态**：实验只涉及（隐含的）单语种听觉刺激，没有检验视觉/阅读条件下或跨语言条件下的泛化性，限制了对“通用语言计算路径”的推广。
- **上下文长度上限未知**：研究说明了上下文长度达到某阈值后对齐出现，但**未系统扫描更大上下文长度下对齐是否会饱和甚至退化**。
- **层与时间点的严格性**：逐层-逐时间点的RSA匹配使用了时间窗口对齐，但该方法对脑信号的空间信噪比、脑区选择敏感，报告中缺少关于源定位或脑区差异性分析；不同脑区的时间尺度差异是否可能反向影响对齐结果待深究。
- **循环架构在规模—上下文上的覆盖可能不对称**：不同架构在可扩展性和有效上下文长度上有天然差异，直接比较二者“对齐涌现点”可能混淆架构与超参数影响。
- **缺乏因果操纵**：研究是纯相关性的观测结果，没有通过干预模型内部计算（如剪层，切除短期记忆模块）直接验证“层计算路径”与“脑响应时序”之间的因果依存。
- **算力与复现成本不透明**：未能报告推理阶段的计算资源信息，影响复现门槛评估。
- **理论机制的候选解释未区分**：这种层-时间对齐可能源于模型在深度上逐渐累积的语义整合（对应脑在时间上的语义累积），也可能只是因为浅层表征保真度更高；本文未能提供在两种候选机制之间进行判别分析的证据。

（完）
