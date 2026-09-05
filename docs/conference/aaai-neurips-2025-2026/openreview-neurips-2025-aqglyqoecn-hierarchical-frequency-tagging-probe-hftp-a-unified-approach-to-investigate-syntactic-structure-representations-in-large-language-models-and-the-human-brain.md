---
title: "Hierarchical Frequency Tagging Probe (HFTP): A Unified Approach to Investigate Syntactic Structure Representations in Large Language Models and the Human Brain"
title_zh: 层级频率标签探测（HFTP）：一种统一研究大语言模型与人脑句法结构表征的方法
authors: "Jingmin An, Yilong Song, Ruolin Yang, Nai Ding, Lingxi Lu, Yuxuan Wang, Wei Wang, Chu Zhuang, Qian Wang, Fang Fang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=aqgLyQOECN"
tags: ["query:abstraction"]
score: 4.0
evidence: 统一探测LLM与人脑皮层中句法结构编码对应关系的频谱方法
tldr: 大语言模型具备类人甚至更优的语言能力并能建模句法结构，但具体的计算单元及其与人脑机制是否一致仍不清楚。本文提出层级频率标签探针（HFTP），利用频域分析定位LLM内单个MLP神经元以及人脑皮层颅内记录中编码句法结构的组分。对该方法在GPT-2、Gemma等模型与真实脑数据上实施后，能辨识出对应句法信号，进而揭示LLM与大脑在结构表征上的相似性，为神经-语言对齐研究提供了统一探针。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 虽然LLM可建模句法，但究竟哪些计算单元参与及是否与人脑机制同源尚不明确。
method: 提出HFTP，通过频域标签化技术对LLM单神经元及颅内皮层数据做频率分析，找出共同编码句法结构的成分。
result: 实验在多种LLM与人脑皮层信号中识别出句法编码神经元和区域，证明该探针能跨系统比较结构表征。
conclusion: 该工作提供直接比较大模型与人脑结构表征的统一频域探针，为探索人工与生物系统中的句法编码桥梁做出贡献。
---

## Abstract
Large Language Models (LLMs) demonstrate human-level or even superior language abilities, effectively modeling syntactic structures, yet the specific computational units responsible remain unclear. A key question is whether LLM behavioral capabilities stem from mechanisms akin to those in the human brain. To address these questions, we introduce the Hierarchical Frequency Tagging Probe (HFTP), a tool that utilizes frequency-domain analysis to identify neuron-wise components of LLMs (e.g., individual Multilayer Perceptron (MLP) neurons) and cortical regions (via intracranial recordings) encoding syntactic structures. Our results show that models such as GPT-2, Gemma, Gemma 2, Llama 2, Llama 3.1, and GLM-4 process syntax in analogous layers, while the human brain relies on distinct cortical regions for different syntactic levels. Representational similarity analysis reveals a stronger alignment between LLM representations and the left hemisphere of the brain (dominant in language processing). Notably, upgraded models exhibit divergent trends: Gemma 2 shows greater brain similarity than Gemma, while Llama 3.1 shows less alignment with the brain compared to Llama 2. These findings offer new insights into the interpretability of LLM behavioral improvements, raising questions about whether these advancements are driven by human-like or non-human-like mechanisms, and establish HFTP as a valuable tool bridging computational linguistics and cognitive neuroscience. This project is available at https://github.com/LilTiger/HFTP.

---

## 论文详细总结（自动生成）

# 1. 论文核心问题与整体含义

- 论文围绕一个核心神经-语言对齐问题展开：**大语言模型（LLM）虽已展现出人类级别甚至更优的语言能力，并能有效建模句法结构，但其具体参与的计算单元（如单个 MLP 神经元）仍然不明**。
- 另一个关键问题是：**LLM 的这些行为学能力，是否源自与人脑相似的机制？** 即 LLM 内部是否真正对应人脑的句法编码方式。
- 整体研究意义在于：提出一种统一的探针工具，将计算语言学与认知神经科学连接起来，在**单个神经元层级（LLM）与大尺度皮层区域层级（人脑）**上比较句法结构的表征，为理解“智能系统如何表示句法”提供跨物种、跨系统的分析框架。

# 2. 方法论

- 论文提出 **层级频率标签探测（Hierarchical Frequency Tagging Probe, HFTP）**，核心思路是**利用频域分析**对神经/模型信号进行标签化，从而定位编码句法结构的成分。
- 其技术操作可分为两个对应层面：
  - **LLM 侧**：对模型内部的单个 MLP 神经元进行频域分析，找出与句法结构相关的神经元响应成分；
  - **人脑侧**：对人脑颅内脑电/记录信号进行同样的频域标记，识别负责不同句法层级的皮层区域。
- 频率标签化的本质是通过不同频率的刺激或输入特征，使目标句法结构在频谱上产生可分离的响应模式，从而将抽象的句法计算分配到具体的计算单元。
- 由于提供的文本来自摘要和元数据，**论文正文中详细的公式、算法流程和超参数并未给出**，只能从方法名称和摘要推断其基本原理。

# 3. 实验设计

- 使用的模型：**GPT-2、Gemma、Gemma 2、Llama 2、Llama 3.1、GLM-4**，覆盖多个规模和代际的开源 LLM。
- 人脑数据：采用**颅内记录（intracranial recordings）** 的皮层信号。
- 基准/对比：未明确提及传统探针方法或基线对比，主要比较了：
  - 不同 LLM 之间句法加工分布层（是否在相似层编码句法）；
  - 人脑不同皮层区域对不同句法水平的响应；
  - LLM 表征与人脑表征的**表征相似性分析（RSA）**。
- 具体使用的语料、刺激句集及脑电采集范式在提供的文本中**没有详细列出**。

# 4. 资源与算力

- 提供的文本中**没有明确说明**使用的 GPU 型号、GPU 数量、训练或推理时长。
- 由于论文中涉及多个开源 LLM 的探针分析，推测需要一定的 GPU 推理资源，但该部分信息缺失。

# 5. 实验数量与充分性

- 在模型侧进行了**6 个模型**的跨模型层间比较，在人脑侧分析了真实颅内记录。
- 进行了**跨模型对比**（Gemma vs Gemma 2, Llama 2 vs Llama 3.1）以分析“升级模型是否更类脑”。
- 进行了**表征相似性分析**（RSA）比较模型与左右半脑的相似程度。
- 但从仅有的摘要来看，**没有提到消融实验、控制实验、不同句法结构类型（如短语结构、依存关系、层级深度等）的系统性变化，也没有说明数据规模与统计检验方法**。因此，目前只能认为该实验在概念上是多模型、多条件的，但**证据充分性无法从摘要层面完全判断**。

# 6. 主要结论与发现

- **多个 LLM（GPT-2、Gemma、Llama 2 等）在不同深度的层级上编码句法结构，且这些层的位置在模型间具有一定相似性**。
- 人脑并不是单一区域处理所有句法层面，而是**不同皮层区域负责不同层级的句法结构**。
- **表征相似性分析显示，LLM 的表征与人脑语言优势半球（左半球）的对齐程度更高**，这与语言处理偏侧化现象一致。
- **升级模型的类脑趋势并不一致**：
  - Gemma 2 比 Gemma 更接近人脑表征；
  - Llama 3.1 与大脑的相似度反而低于 Llama 2。
- 该结果提示：LLM 的能力提升并不总是通过“更像人”的机制实现，也可能采用了非人脑独有的计算策略，这对理解模型行为可解释性提出了新问题。

# 7. 优点

- **跨尺度统一探针**：首次在同一频域框架下定位“单个神经元”和“皮层区域”两级表征，使得模型与人脑的比较在同一分析逻辑下进行。
- **直接建立比较桥梁**：将大模型的可解释性研究与人脑记录数据相结合，提供一条从人工系统到生物系统的可操作分析路径。
- **方法论具有通用性**：HFTP 不限于特定模型或特定脑区，可扩展到更高层模型及更多语言结构。
- **对模型演化的分析视角新颖**：通过追踪同一系列模型升级前后的类脑程度，能够揭示模型设计（或训练数据/方法）与神经对齐之间的复杂关系。

# 8. 不足与局限

- **信息不足**：由于现有文本仅为摘要和元数据，无法评估方法细节、复现难度和统计分析力度。
- **频率标签范式局限**：频域响应可能只捕捉周期性/层级性较强的句法信号，对于更复杂、非周期性的语言结构（如语义、语用）可能无能为力。
- **脑数据稀疏性**：颅内记录来自患者而非健康人群，覆盖区域有限，左右半球对齐的分析可能受采样区域影响。
- **比较的公平性存疑**：LLM 训练目标与人类语言习得的生物机制完全不同，即使表征在几何上对齐，也不能直接等同为“相同机制”。
- **仅观察相关系数而非因果途径**：RSA 只能说明表征相似，无法证明 LLM 的训练过程或推理过程使用了与人脑相同的因果计算步骤。
- **升级趋势分歧的未解释性**：为什么 Gemma 系列更类脑而 Llama 系列相反，论文没有在摘要层面给出机制性解释，可能难以概括为一般规律。

（完）
