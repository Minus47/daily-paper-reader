---
title: "PKR-QA: A Benchmark for Procedural Knowledge Reasoning with Knowledge Module Learning"
title_zh: PKR-QA：面向程序性知识推理的知识模块学习基准
authors: "Thanh-Son Nguyen, Hong Yang, Tzeh Yuan Neoh, Hao Zhang, Ee Yeo Keat, Basura Fernando"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39638/43599"
tags: ["query:abstraction"]
score: 4.0
evidence: 融合本体、ConceptNet与LLM输出构建程序知识图谱并提出知识模块学习
tldr: 现有大语言模型在表格等结构化知识上的处理能力评测多关注高层推理，缺少对大表格单元格的细粒度感知；类似地，程序性任务问答也缺乏带结构化知识的基准。本文提出PKR-QA基准，以程序知识图为中心，将教学视频、领域本体、ConceptNet常识及LLM结构化输出半自动链接并人工核验。进一步设计图遍历模板生成QA对，并提出知识模块学习以开展可解释推理，为程序性知识库建设与评测提供工具。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 程序性任务问答需要结构化知识库和可解释推理，但现有基准在覆盖度与评测方法上仍有不足且构建繁琐。
method: 使用程序知识图将COIN教学视频、本体、ConceptNet和LLM输出链接并人工验证，经图遍历模板自动生成问答对，再以知识模块学习训练可解释推理。
result: 基准与模型实验展示出程序性推理QA的可行性和评测稳定，最终形成可复用的知识库与推理模型资源。
conclusion: 该工作为程序性知识的组织与可解释推理提供了新基准与知识模块学习方法，可辅助大型概念或任务本体的构建。
---

## Abstract
We introduce PKR-QA (Procedural Knowledge Reasoning Question Answering), a new benchmark for question answering over procedural tasks that require structured reasoning. PKR-QA is constructed semi-automatically using a procedural knowledge graph (PKG), which encodes task-specific knowledge across diverse domains. The PKG is built by curating and linking information from the COIN instructional video dataset and the ontology, enriched with commonsense knowledge from ConceptNet and structured outputs from Large Language Models (LLMs), followed by manual verification. To generate question-answer pairs, we design graph traversal templates where each template is applied systematically over PKG. To enable interpretable reasoning, we propose a neurosymbolic approach called Knowledge Module Learning (KML), which learns procedural relations via neural modules and composes them for structured reasoning with LLMs. Experiments demonstrate that this paradigm improves reasoning performance on PKR-QA and enables step-by-step reasoning traces that facilitate interpretability.

---

## 论文详细总结（自动生成）

# PKR-QA：面向程序性知识推理的基准与知识模块学习（AAAI-26）中文总结

## 1. 论文的核心问题与整体含义

*   **研究背景**：理解程序性任务（如烹饪、机械维修、医疗流程）对于机器智能至关重要。人类通过终生学习，结合对物体用途的常识理解和多步骤过程中时序与因果依赖的推理，不仅知道“做什么”，还能推断“为什么”和“怎么做”。然而，现有视觉问答基准大多聚焦于视频或图像描述性推理，缺乏对模型在**程序性知识推理**能力上的针对性评估。
*   **核心问题**：当前大视觉语言模型（VLM）的推理过程是不透明的（黑盒），其内在推理机制难以验证；同时，在需要结合**外部程序性知识**的视频问答任务上，传统基准和模型的性能评测存在空白。
*   **核心痛点**：
    *   缺乏一个系统化的基准来评估多跳、演绎、概率、因果和反事实等程序性推理能力；
    *   现有神经符号方法（如ViperGPT）缺乏领域特定的知识学习和可定义的推理边界；
    *   端到端黑盒推理（即使借助思维链）缺乏可解释性、可控性和可靠性，尤其在医学、工业等特定领域。
*   **工作含义**：该论文提出一个全新的基准（PKR-QA）和一种名为知识模块学习（KML）的神经符号方法，旨在推动程序性知识AI的发展，以支持可验证、可调试和可解释的知识驱动推理。

## 2. 论文提出的方法论

### 2.1 PKG 程序知识图构建
- **核心组成与流程（半自动 + 人工验证）**：
  1. 定义知识图谱模式（PKGS），包含七种核心实体类型：**Domain, Task, Step, Action, Object, Tool, Purpose**；
  2. 从 **COIN 数据集**的训练集中提取步骤序列、任务边界和时序关系；
  3. 用 **GPT-4o** 从步骤描述中抽取动作-对象对，并推断潜在工具清单，再进行人工验证；
  4. 用 **ConceptNet** 作为外部常识来源接入“用途”（Purpose），并用GPT-4o进行上下文特化处理；
  5. 使用 **Sentence-BERT** 对语义相近的“目的”进行合并（相似度阈值0.8）；
  6. 图谱最终存储在 **Neo4j** 图数据库，持续人工核实。
- **图谱规模与结构**：总计包含2954个实体、12484条关系，并配有 START/END 节点用于建模任务边界与经验的步骤转换频率。

### 2.2 PKR-QA 问答数据集的半自动化生成
- 设计**图遍历模板**（Traversal Templates，共17种），每个模板表达特定推理模式，如 Step→HAS TOOL→Tool→HAS PURPOSE→Purpose（问“该步骤使用到的工具有何用途？”）
- 使用 Cypher 查询在 PKG 中检索答案，生成五选一题目；
- 使用采样策略均衡正确答案与干扰项的分布；
- 每个问题都配有正式的 Cypher query 形式，便于后续用 KG 评估、模型监督和推理轨迹分析。

### 2.3 KML（知识模块学习）方法
- **核心思想**：为 KG 中的每种*二元关系类型* Rk(Ei,Ej) 训练一个小型但独立的神经网络模块（Neural Knowledge Module，KM），其本质是一个可学习的神经关系映射（图神经网络式操作）。输入是头实体嵌入，输出是一个能代表“一组正确尾实体”的向量，并在训练时使用**对比损失函数**：
  - Loss = −log(exp(ej·x(ej)/τ) / Σ_{ep∈B} exp(ej·x(ep)/τ))
- **具体特征**：
  - 初始时使用**冻结CLIP文本编码器**嵌入实体名（称为KML-F-CLIP）；
  - 也探讨了从零学习的方案（KML-Rand），以及微调CLIP编码器的方式（KML-CLIP）；
  - 每个关系都可训练其逆关系。
- **可选的嵌入配置**：使用 CLIP 文本编码器（冻结/微调）、随机初始化的Embedding层；
- **推理流程**：
  1. 将视频片段交给 VLM（如 ProceduralVRL）得到高层次实体类别分布（top-K类，如步骤分类）
  2. LLM生成一个程序（多个模块调用组合），多个替代程序 = “多思路/多痕迹推理”
  3. 程序执行：将加权后的实体嵌入输入首个 KM，接着将输出嵌入送入下一个KM（如链式过程 zi→zj→zk→zf）
  4. 最后一层输出与选项嵌入做余弦相似度 → softmax → 预测答案；
  5. 使用 GPT/deepseek/Mistral/Qwen 等不同 LLM 来生成路径并验证泛化能力。

## 3. 实验设计

### 3.1 数据集（Baseline/Benchmark）
- **PKR-QA** 测试集：46,921题，来自COIN测试分区的全部视频片段；
- 训练集1700题/验证集850题（100/50/模板）；
- 数据合理性人工测评：92.4%问题合理（三名参与者中至少一名答对），随机基线≈20%（机会水平）。

### 3.2 对比方法
- **VLM系列**：DeepSeek-VL2, MiniCPM-V, mPLUG-Owl3, Qwen2.5-VL, VideoChat2-HD
  *   测试设置：零样本 / 给P.VRL输出 / KG训练 / QA训练 / KG+QA联合微调
- **神经符号及传统方法**：
  *   概率逻辑遍历推理：IGP（不确定性传播）
  *   知识图谱嵌入方法：TransE、TransH、RotatE（含CLIP变体）
  *   现代NS模型：ViperGPT、MAC（自底向上记忆网络）
  *   大语言模型直接调用：GPT-4o+P.VRL

### 3.3 附加跨Benchmark泛化测试
- 从STAR benchmark抽取10条关系进行训练评估
- 使用WikiHow 7,687个任务、57,027个步骤、870万三元组在GPT-4o生成的KG上进行训练并做零样本/微调迁移评估

## 4. 资源与算力

- 文档明确提及了显卡型号和对应任务：
  *   用于 VLM 的 NVIDIA A100 GPU（80GB VRAM）
  *   KML 训练使用 NVIDIA GeForce RTX 2080 Ti 和 A5000 GPU
  *   知识图谱嵌入模型使用 RTX 3090 GPU（24GB VRAM）
- **说明**：论文未具体提供各个实验的GPU数量、训练总耗时、GPU卡时数或大致预估整体能耗的详细数据。

## 5. 实验数量与充分性

| 实验类别 | 数量/对象 | 说明与评价 |
|---|---|---|
| VLM评估设置 | 5种设置 × 5个大模型 | 较系统评估了“知识注入”方式的差异，很好地展示了KG+/QA微调的价值，但设置之间存在不完全公平的参数开销问题未深入比较（例如更多的LoRA）。 |
| NS基线对比 | 3类共约9~10种对比方法 | 覆盖面较全面，覆盖了图嵌入、记忆网络、直觉程序合成等代表方法。 |
| 消融研究 | 是否用PKG训练、程序并行数量、专家干预程序、不同LLM生成器（GPT/Deepseek/Llama/Mistral/Qwen） | 揭示了模型对某类LLM的鲁棒性并测试多程序协调。 |
| 可控性研究 | top-1至top-5的实体分类输入 | 输出显示了鲁棒性不高依赖于步骤分类。 |
| 跨基准泛化 | STAR（4类问题）与WikiHow领域迁移 | 加强了方法泛化声称的说服力。 |
| 人类合理性实验 | 170题，3人/题 | 确认了题材可行与回答标签无明显偏差。 |

**总体评价**：实验数量和覆盖比较充分，**但细节（如哪些KV组合、计算/推理消耗）没有深究**；对不同方法评估的有效性与统计显著性（例如多重随机种子、置信区间）未报告，由此应在“确保实验公平性”上持保留考虑。

## 6. 主要结论与发现

- 向VLMs提供步骤级/任务级预测类别（即P.VRL 信息）比仅提供原始视频片段能带来更稳定的性能增益（例如Qwen2.5-VL零样本59.6→69.4）；
- 单独的KG预训练对VLM增益有限，而基于有限QAs数据集（1700条）微调带来的增益明显；
- KG与QA联合微调（KG+QA）是所有VLM中的最优设置（Qwen2.5-VL达到74.2%超过其他同体量模型）；
- 提出的 **KML-CLIP（KG+QA训练）** 在所有NS方法中展现最优：78.1精度/77.1平均精度；KML-Rand 已超过 KML-F-CLIP，说明端到端学习文本编码器也能达到竞争力性能；
- 对比传统KG嵌入（TransE/TransH/RotatE），KML更稳健；
- **解释性强**：每个KM计算的结果可与人类可读概念相对性联系（实例表显示逐步的“工具”和“步骤”），有利于审计；
- **对LLM不敏感**：生成的程序在多个LLM不同能力层级下性能浮动有限（GPT-4o最好，Qwen-2.5最差，但仍可达63.7%）；
- 在领域迁移（WikiHow）下性能有所下降为零样本状态，但可在小规模重训下补足（KG+QA→74.9%）。

## 7. 优点

- **新问题的提出**：不同于单纯知识问答视频QA，PKR-QA要求融合视频内部状态（步骤识别）与外部程序和共同规则知识，天然支持组合推理；
- **半自动+人工验证的拓扑路径**：较大程度控制了知识图谱质量和风险；
- **KML及其轻量化模块设计**：利用对比学习单独为“每个关系”设定可组合神经网络模块——好处是可显式验证推理路径下每一步的语义，解决了知识图谱嵌入中语义不可辨认的问题；
- **直接在Video QA任务上训练低参数量模块**（KMs），而非依赖对推理路径的大量监督，有利于小样本适应；
- **鲁棒评估**：不仅关注端到端精度，还考察KML对输入归类噪声容忍度、多路径一致性、跨LLMs泛化与不同领域知识等，能较好证明框架具有实际延展性；
- **支持可控性**：程序可被专家后台进行重写优化与解释，对于特定专业领域（医疗/工业）有安全上的积极作用。

## 8. 不足与局限

- **嵌入层语义信息限制**：实体属于CLIP浅层开集，在极端专业术语（如医疗器械专名）上对知识图谱短语之间的紧密映射可能存在语义间隙；
- **知识只深度依赖COIN源结构**：数据集同构性高（同一领域重复出现），而对罕见/极端领域的覆盖面仍有限；
- **大量LLM辅助合成：标签标记准确性受制于GPT-4o抽取质量，尽管有人工校验，但缺陷模式仍可能传播；
- **分类类别的粒度局限**：问答集仅允许五种候选答案和固定实体类型，限制了开放式解释（如深度多模态解释或需要数值/序列原因的问题）；
- **没有对全部视频QA做多模型平均精度的统计显著性检验，差异显著性仍是有限性的；
- **基线评估不足保证的对比公平性**：虽然和ViperGPT等有对比，但KML的路径生成相对基线受益于访问测试集的语义模式（知识图谱关系的数量一致），而MAC/ViperGPT在没有预训练逻辑演化情况下直接使用，可能低估这类方法的真实上限；
- **程序简略程度的可控性（程序顺序的连贯性）也未做全面鲁棒分析**，若系统扩展为更复杂的程序空间可能会显著增加解答不稳定性风险。

---

（完）
