---
title: "NeuroBasket: Interpreting Neuron Responses with Semantic Baskets"
title_zh: NeuroBasket：用语义篮子解释神经元响应
authors: "Chi Young Song, Hyeon Bae Kim, Yong Hyun Ahn, Seong Tae Kim"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3grV2CE4N0"
tags: ["query:abstraction"]
score: 7.0
evidence: 通过层次聚类与语言锚定构造语义一致的神经元组，用集合运算揭示共享抽象，可辅助概念抽象层级的自动构建
tldr: 深度神经网络的分布式概念编码使单个神经元解释不稳定。NeuroBasket通过层次聚类与自然语言锚定构造语义连贯的多神经元组，称为语义篮子；集合运算中的并集揭示共享抽象，差集凸显判别线索。在多种卷积与Transformer模型上，语义篮子获得了稳定且语义对齐的分组，并保留与预测相关的通路。该方法为神经网络中抽象概念的结构化解释提供了新工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 单个神经元或非层级分组难以刻画深层网络中的分布式概念编码。
method: 将神经元按语义分层聚类并用自然语言标签锚定，构成可集合运算的语义篮子。
result: 在多种模型与数据集上产生稳定且语义对齐的多神经元分组，捕捉与预测相关通路。
conclusion: 语义篮子为揭示网络中的共享抽象与判别概念提供了层级化解释方式。
---

## Abstract
Deep neural networks excel across domains, yet their internal representations remain opaque. Prior approaches based on single neurons or non-hierarchical groups are limited by the distributed nature of concept encoding. We introduce Neurobasket, a framework that constructs semantically coherent multi-neuron groups through hierarchical clustering and natural language grounding. Neurobaskets enable set-theoretic reasoning, with unions revealing shared abstractions and differences highlighting discriminative cues. Experiments across convolutional and transformer models, trained on diverse datasets, show that neurobaskets yield stable and semantically aligned sets, while capturing prediction-relevant pathways. Qualitative visualizations further showed that grouped neurons correspond to coherent and localized concepts. Overall, Neurobasket provides a structured and compositional view of neural representations, extending beyond unit-centric or non-hierarchical explanations.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义

- 深度神经网络（DNN）具有强大的表示能力，但其内部表示不透明。为了可解释性，已有方法多基于*单个神经元*或*非层级分组*来定位概念，但深层网络中的概念往往是**分布式编码**，一个概念分散在多个神经元中，单个神经元的响应并不稳定，导致解释粒度不足。
- 论文提出一个核心问题：如何将分布式、多神经元激活转化为与人类语义对齐的结构化表示？即跳出单神经元中心主义和扁平分组，构建**层级化、可组合**的概念单位，从而更好地反映网络内部的抽象编码机制。

## 方法论文本

### 核心思想

- 通过对神经元的激活向量进行**层次聚类（hierarchical clustering）**，将语义上相近的神经元归拢为不同粒度的群组——这些群组称为**“语义篮子”（neurobaskets）**。
- 用**自然语言**为每个篮子锚定语义标签，使群体具备可解释的语言意义。
- 篮子之间定义了**集合运算**：
  - **并集（union）** 用于联合若干篮子以揭示它们共享的更高层抽象；
  - **差集（difference）** 用于提取某个概念独有的判别性特征或线索；
- 论文强调该框架的目的是提供一种**结构化、可组合**的视图，把底层神经元聚为不同抽象层级的候选“概念单元”，而非只做线性或聚类式的神经元分组。

### 技术细节与流程（按文字描述归纳）

1. 神经元集合获取：对给定模型（卷积或Transformer）的内部神经元，在不同输入样本下获取其激活值；
2. 层次聚类：按激活模式计算相似度，递归合并神经元，形成从细粒度到粗粒度自底向上的层级树；
3. 语言锚定：为聚出的节点（篮子）自动匹配/拟合语义描述（如借助**多模态对齐模型**或标注语料）；
4. 集合推理：对篮子实施并集、差集等操作，导出“共享于多个解释场景的抽象”和“仅特定概念具有的判别特征”；
5. 解释输出：综合各层级篮子与语义标签，形成对模型预测通路的多粒度自然语言描述。

## 实验设计

- **数据集/模型类型：** 在*卷积模型*与*Transformer模型*两个大类上、多样化的数据集之上开展实验。摘录中只说明“diverse datasets”，未具体点名 ImageNet、CIFAR 等数据集（摘要未列出说明）。
- **Benchmark：** 针对解释方法的评价主要考查三类指标或现象：
  1. 分组后语义标签与组内神经元实际行为的匹配程度（语义对齐性）；
  2. “篮子”在不同种子、初始化或随机实验条件下分组的稳定性；
  3. 篮子与模型实际预测路径的相关性，即分组是否落在影响分类结果的神经元通路中。
- **对比方法：** 对比*单神经元解释法*与*扁平（非层级）分组解释法*，以体现提出的层级式语义分组带来的增益。其他基线名未在摘要中出现。

## 资源与算力

- 提供的论文摘录（PDF 元数据与摘要）中**未明确提及** GPU 型号、卡数、训练时长等计算资源使用情况；原始 OpenReview 页面也不包含可提取的运行资源表。
- 论文审查与元数据中也未见 estimate FLOPs 之类信息，只能归纳为“未披露实验成本”。

## 实验数量与充分性

- 从原始摘要来看，在多个模型架构（卷积+Transformer）、多个数据集上对“神经篮子”质量做了经验验证，同时采用“群组稳定性+语义对齐+通路保留”三种角度来评价，基本覆盖了方法的目标主张。
- 但需注意：
  - 页面收录为 **ICLR-2026-Rejected-Public**——说明论文在审稿中被拒，实验中可能存在说服力不足；
  - 摘录并未展示消融设计的数量，也无有关超参数（聚类距离度量、树切分位置等）敏感性分析的证据；是否充分检验了**并集/差集**各种组合的稳健性仍需看全文，无法仅凭摘要确认；
  - 对比对象比较粗略（“单神经元”“非层级分组”），该声明可能因缺乏强基线而削弱公平性。总而言之：在缺乏全文的情况下，实验充分性不能做出高置信判定。

## 论文主要结论与发现

- 语义篮子（neurobaskets）比单神经元和简单分组更符合分布式概念编码的特点；
- 在不同模型（卷积和Transformer）与异构数据集上，该方法都能**保持分组稳定且语义对齐**；
- 通过集合并/差运算可以展示高级共享抽象和判别细节，说明此类分组在语义层面具有*可组合性*；
- 可视化结果显示：被打包的神经元群对应图像中的*外观一致且位置上局部化的概念*，从而在使用价值上优于碎片化的神经元单元。

## 优点

- **新颖的表达单位**：提出“神经元语义篮子”这样一个介于单独单元与全局网络之间的中间单位，让解释粒度与真实编码过程更匹配；
- **层级化组成性**：将神经元的相似度按层级组织，并通过自然语言锚定搭建“从细概念到抽象概念”的阶梯；
- **集合逻辑可操作**：并/差等集合运算相比通常仅选用 Top-k 神经元的解释法更有推理味，适合再现“共享抽象”和“区别线索”；
- **与深度学习表征结合自然**：可用现有多模态模型实现“语言锚定”，无需重度人工局部标注，具备一定扩展性。

## 不足与局限

- （页面级别局限）本摘要只呈现高层框架，**缺少详实的方法细节和实验曲线**，其严谨程度难以独立复核，仍属于被拒工作，落地实力存疑；
- 对于尺度较大、神经元成千上万层级复杂的模型，聚类的稳定性和语义对齐效果需要更多算力和大规模的消融；不同数据集间的差异可能让“语言标签匹配”形成偏置；
- 该方法有效性依赖用以锚定的**外部视觉语言模型的强弱**，如果世界知识不充分就可能得到明显偏差或滞后的语义描述；
- 实验未明确列出典型视觉数据集上的量化收益（如分类精度无关的各种系数数值），与解释性 baselines 的对比公平性尚未夯实；
- 场景仍主要指向视觉/图像任务，对序列、图或动作类型网络是否适用不明确，且“群组与预测有相关性”不等于因果必要性，解释过程中可能存在验证偏差。

（完）
