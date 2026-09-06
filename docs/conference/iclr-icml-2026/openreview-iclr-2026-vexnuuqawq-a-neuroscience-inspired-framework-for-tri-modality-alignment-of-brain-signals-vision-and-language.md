---
title: "A Neuroscience-inspired Framework for Tri-modality Alignment of Brain Signals, Vision, and Language"
title_zh: 脑信号、视觉与语言三模态对齐的神经科学启发框架
authors: "Yixing Ke, Binghao Ye, Dong Liang, Kun Shang"
date: 2025-09-09
pdf: "https://openreview.net/pdf?id=VExNUuQAWq"
tags: ["query:eeg-align"]
score: 9.0
evidence: 面向脑信号、视觉与语言的三模态对齐与检索
tldr: 针对现有脑信号视觉检索忽视神经加工机制的问题，作者提出一种神经科学启发的三模态对齐框架，将脑信号、视觉和语言表征统一到共享语义空间。方法考虑了特征生理学错配、同类内神经表征一致性以及动态图文语义依赖。实验表明该框架能有效提升脑到视觉和语言的跨模态检索性能。相关工作为脑机接口中的多模态对齐提供了新的实现思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有脑信号视觉检索忽略视觉神经机制，造成高层语义特征与低层神经反应错配、同类表征一致性差等问题。
method: 构建神经科学启发的三模态对齐框架，在脑信号、视觉和语言间建立统一语义空间并加入类内表征一致性约束。
result: 实验结果显示框架有效提高脑到视觉与语言的跨模态检索和语义对齐质量。
conclusion: 引入神经机制先验能解决脑信号跨模态对齐中的特征生理学错配并提升检索效果。
---

## Abstract
Visual retrieval from brain signals is a key challenge in Brain-Computer Interfaces (BCIs). Existing methods mainly rely on direct cross-modality mapping, yet they often overlook the neural mechanisms of visual processing, which leads to three major limitations. First, a feature physiology mismatch arises because high-level semantic features extracted by image encoders do not align with the low-level neural responses evoked by rapid visual stimulation. Second, most approaches emphasize cross-modality alignment while neglecting the similarity of neural representations within the same category, which results in poor intra-modality semantic consistency. Third, brain-image alignment typically depends on static image-text semantic spaces and therefore lacks dynamic semantic priors that interact with brain activity. We introduce NeuroAlign, the first neuroscience-inspired framework for brain-vision alignment. NeuroAlign mitigates the feature physiology mismatch by integrating bottom-up structural perception with top-down semantic modulation, enhances semantic consistency through intra-modality self-supervision and cross-modality intra-class constraints, and leverages large language models (LLMs) to provide dynamic semantic signals that interact dynamically with brain responses. Extensive experiments demonstrate that NeuroAlign achieves state-of-the-art performance on both intra-subject and inter-subject retrieval tasks, which validates the effectiveness of this neuroscience-guided alignment strategy.

---

## 论文详细总结（自动生成）

# 中文总结：脑信号、视觉与语言三模态对齐的神经科学启发框架

## 1. 核心问题与整体含义（研究动机和背景）

- **研究领域**：脑机接口（BCI）中的脑信号视觉检索任务，即根据脑电图（EEG）等神经信号来检索对应的视觉图像/语义内容。
- **现状缺陷**：现有方法主要依赖直接的跨模态映射，但普遍忽略了视觉加工的神经机制，由此引出三大核心局限：
  1. **特征生理学错配（Feature Physiology Mismatch）**：从图像编码器提取的高层语义特征本质上对应大脑后期、抽象的视觉加工阶段，而快速视觉刺激（rapid visual stimulation）诱发的神经响应更多处在早期、低层感知加工阶段。将这两者直接对齐，在生理学上是错配的。
  2. **类内神经表征一致性不足**：多数已有方法只关注跨模态对齐，却忽略了同一个语义类别中多个脑信号样本之间本身应具有的内在表征相似性，导致脑信号侧的类内语义一致性差。
  3. **静态图文语义空间缺乏动态先验**：传统脑—图对齐依赖于静态的图文语义空间，缺少能与实时脑活动进行动态交互的语义先验信息。
- **整体含义**：作者提出 NeuroAlign——首个受神经科学启发的脑—视觉对齐框架，系统性回应上述三个问题。

## 2. 方法论：核心思想、技术细节与算法流程

论文提出 **NeuroAlign** 框架，核心思想是从人类视觉系统的加工机制中汲取灵感，构建一个同时容纳脑信号、视觉和语言的统一语义空间。具体技术路线可概括为三条：

- **针对"特征生理学错配"——融合"自底向上结构感知 + 自顶向下语义调制"**：
  - 借鉴视觉皮层中"前馈（bottom-up）通路负责快速低层结构感知、反馈（top-down）通路承载高层语义调节"的神经机制。
  - 在模型设计上，不是简单地把深层图像特征直接和脑信号硬对齐，而是综合了局部结构特征与全局语义的调节作用，使对齐建立在更符合生理学实际的表征链路之上。
- **针对"类内表征一致性不足"——引入脑信号侧与跨模态侧的类内一致性约束**：
  - **模态内自监督**：在脑信号模态内部施加基于类别的自监督信号，推动同类别脑信号在嵌入空间中聚合。
  - **跨模态类内约束**：在跨模态对齐过程中进一步收紧同一语义类别的跨模态表征，既保证脑信号与视觉/语言跨模态可对齐，也保证同类样本在统一语义空间中形成一致的簇。
- **针对"静态语义空间缺乏动态性"——引入大语言模型（LLM）提供动态语义信号**：

  - 利用大语言模型在图文语义建模方面的强大能力，使其根据当前脑信号/图像提供**动态变化的语义表征**，与脑活动进行交互式调整，而非依赖固定不变的图文语义先验。
- **整体流程（文字描述）**：三个分支编码器分别处理脑信号（EEG编码器）、视觉图像（视觉编码器）和语言文本（语言编码器/LLM）；中间通过上述三个方法论模块进行联合训练和特征调制；最终将三类表征映射到统一语义空间中，支持脑到视觉、脑到语言的跨模态检索任务。

## 3. 实验设计与评估

- **任务场景**：文中的实验覆盖两类典型场景：
  - **被试内（intra-subject）检索任务**：同一被试的脑信号数据划分训练集与测试集，评估模型在个体内部泛化能力。
  - **被试间（inter-subject）检索任务**：在跨被试的零样本/迁移情境下评估模型，要求模型适应不同个体间的神经信号差异。
- **Benchmark**：作者声称 NeuroAlign 在上述两种检索任务上均达到**最先进水平（State-of-the-Art）**。
- **对比方法**：文中与"现有直接跨模态映射方法"进行了对比。由于当前提供的论文信息有限，具体对比方法名称和数量在现有材料中未完整列出。
- **数据集细节**：文中未明确指出使用了哪些具体的EEG数据集（如EEG-ImageNet、THINGS等），但从研究任务推断，应为视觉诱发脑电的公开基准。

## 4. 资源与算力

- **现有内容中未明确说明**：论文文本中未提及任何关于 GPU 型号、GPU 数量、分布式训练配置、训练时长、参数量、能耗等资源信息。
- 需要说明：本总结仅基于论文提供的摘要/元数据，无法获取完整的实验设置与算力细节。如果在完整论文中有对应描述，应以原文实验章节为准。

## 5. 实验数量与充分性评估

- **可识别的实验维度**：
  - 被试内检索与被试间检索两个主任务设置（各覆盖一定的评估指标）。
  - 从方法描述隐含的实验安排来看，至少应有对应三大创新点的消融实验（即：去掉自底向上/自顶向下融合、去掉类内约束、去掉LLM动态语义，分别观察性能变化），以验证每个模块的有效性。
- **充分性判断**：
  - **优点**：被试内与被试间的双场景评估覆盖了个体泛化和跨个体泛化两个关键维度，实验场景设计合理，能更好地体现脑信号检索任务的实际应用难点；三模态（脑、视觉、语言）框架同时检验了脑—视觉和脑—语言两条检索链路，覆盖面较好。
  - **不足**：由于现有材料仅为摘要级别，无法确认具体数据集数量、类别规模、被试人数、对比方法数量以及消融实验的完整矩阵；另外，要评估实验的"公平性"，需要考察预训练权重来源、特征提取器是否冻结、以及校准/调参等细节，这些在本材料中均不可见。总体判断为"实验方向设计充分，但细节证据不足"。

## 6. 主要结论与发现

- **有效性验证**：NeuroAlign 在被试内与被试间两类检索任务中均取得最优或最先进的性能，验证了"以神经科学机制为先导的跨模态对齐策略"的有效性。
- **问题解决印证**：实验结果间接验证了三大设计动机——特征生理学错配确实可以通过自底向上与自顶向下融合来缓解；加入类内一致性约束能改善同类表征的语义一致性；引入LLM动态语义先验能够带来比静态图文空间更好的对齐效果。
- **领域启发**：这项工作表明，在脑信号与外部模态的对齐中引入神经机制先验，是一种有前景的实现思路，为BCI多模态对齐研究提供了新方向。

## 7. 优点与亮点

- **科学动机新颖**：将"视觉神经加工的两大通路机制"引入多模态对齐设计，超越了单纯追求表征空间统一的工程视角，在跨模态对齐领域具有方法论创新意义。
- **问题诊断清晰**：精准指出已有方法的三项生理学/语义学层面的深层局限，且每一项局限都能对应到一个具体的设计模块，逻辑链条完整，论证结构紧凑。
- **三模态统一框架**：将脑信号、视觉、语言三者统一在共同语义空间，而不是仅做脑—图二元对齐，符合现代多模态大模型的发展方向，也拓展了脑信号检索的应用边界。
- **大语言模型的动态引入**：与常见的"用静态图文预训练空间做固定对齐"不同，让LLM提供动态的语义调制信号，更具灵活性。

## 8. 不足与局限

- **可复现性与细节透明度有限**：受本材料篇幅限制，文中未呈现损失函数具体形式、超参数设置、负样本采样策略、编码器结构等关键工程细节，读者难以直接复现。
- **资源与计算开销未披露**：多次提及使用大语言模型提供动态语义，但 LLM 的推理负载、训练阶段是否需要端到端后向传播更新 LLM 参数、整体显存开销等都未说明，对资源受限的研究小组不够友好。
- **实验泛化性的未知边界**：没有给出使用数据集的大小、被试数量和类别覆盖范围。如果仅在1-2个EEG数据集上验证，跨数据集的泛化能力仍待检验。
- **方法复杂度与收益的权衡**：引入了"三模态 + 动态LLM + 双路径调制 + 多类型约束"，系统复杂度较高，但摘要中没有展示与轻量级baseline的详细性能差距以及推理成本对比，存在"性能提升可能以小幅度换来大复杂度"的风险。
- **评估维度可进一步扩展**：检索任务之外，还可在图像解码重建、脑信号分类、跨模态生成等下游任务上验证框架的通用性；在零样本跨被试情境下的可靠性也有待更多实验支撑。
- **"神经科学启发"的验证深度有限**：当前实验属于行为级/性能级验证，并没有通过神经科学手段（如EEG时间序列上的神经解码分析）直接证明"自底向上与自顶向下融合"确实模拟了对应的皮层加工过程。一定程度上，该机制的生物学真实性仍是间接的。

（完）
