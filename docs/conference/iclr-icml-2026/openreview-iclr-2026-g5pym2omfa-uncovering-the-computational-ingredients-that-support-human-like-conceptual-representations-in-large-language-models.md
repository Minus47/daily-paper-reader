---
title: Uncovering the computational ingredients that support human-like conceptual representations in large language models
title_zh: 揭示支撑大语言模型类人概念表示的计算成分
authors: "Zach Studdiford, Timothy T. Rogers, Kushin Mukherjee, Siddharth Suresh"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=g5pYm2OmfA"
tags: ["query:abstraction"]
score: 8.0
evidence: 探索大语言模型类人概念表示的关键计算因素，强调模型与人类表征对齐，与类脑概念空间目标一致
tldr: 大语言模型产生类人概念表示的机制不明确，而且现有基准没有衡量模型与人类表征对齐。论文系统比较不同架构、微调及数据条件下的概念表征，提出以表示对齐为核心的评估方向。实验定位出若干支持类人概念的关键成分。该工作对建立大模型与人脑在抽象概念上的结构化对应具有直接价值。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 大模型类人概念表示依赖哪些计算成分未知，且现有基准无法衡量与人类表征的对齐。
method: 变换架构、微调方式和训练语料，比较模型与人脑或人类行为的概念表示对齐程度。
result: 识别出对类人概念表示最关键的计算成分，并证明传统基准分数会扭曲表征优劣判断。
conclusion: 表示对齐应成为评估类人概念模型的核心标准，可指导类脑概念空间构建。
---

## Abstract
The ability to translate diverse patterns of inputs into structured patterns of behavior has been thought to rest on both humans’ and machines’ ability to learn robust representations of relevant concepts. The rapid advancement of transformer-based large language models (LLMs) has led to a diversity of computational ingredients — architectures, fine tuning methods, and training datasets among others — but it remains unclear which of these ingredients are most crucial for building models that develop human-like representations. Further, most current LLM benchmarks are not suited to measuring representational alignment between humans and models, making existing benchmark scores unreliable for assessing if current LLMs are making progress towards becoming useful cognitive models. Here, we address these limitations by first evaluating a set of over 70 models that widely vary in their computational ingredients on a triplet similarity task, a method well established in the cognitive sciences for measuring human conceptual representations,  using concepts from the THINGS database. Comparing human and model representations, we find that models that undergo instruction-finetuning and which have larger dimensionality of attention heads are among the most human aligned. We also find that factors such as choice of activation function, multimodal pretraining, and parameter size have limited bearing on alignment. Correlations between alignment scores and scores on existing benchmarks reveal that while some benchmarks (e.g., MMLU) are better suited than others (e.g., MUSR) for capturing representational alignment, no existing benchmark is capable of fully accounting for the variance of alignment scores, demonstrating their insufficiency in capturing human-AI alignment. Taken together, our findings help highlight the computational ingredients most essential for advancing LLMs towards models of human conceptual representation and address a key benchmarking gap in LLM evaluation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机与背景）

- 论文关注认知科学与人工智能的交叉问题：人类能够将多样的输入模式转化为结构化的行为模式，背后依赖的是对相关概念形成的稳健表征能力。
- 当前 Transformer 架构的大语言模型（LLM）在架构、微调方式和训练数据等“计算成分”（computational ingredients）上存在很大差异，但学界并不清楚哪种成分对模型形成**类人的概念表征**最为关键。
- 现有 LLM 基准大多衡量任务正确率，并不适合测量“人类与模型之间的表征是否对齐”，因此高分并不说明模型在概念层面与人类具有相似的内在结构。
- 论文的整体含义是：需要以“表征对齐”为核心，重新审视大模型能否成为有用的认知模型，并找出推动类人概念表示的关键计算因素。

### 2. 论文提出的方法论

- **核心思想**：利用认知科学中成熟的行为范式——三元相似性任务（triplet similarity task）——来度量人类和大模型对概念的内部表征结构，从而计算二者的对齐程度。
- **概念材料**：采用 THINGS 数据库中的概念，该数据库在人类概念认知研究中被广泛使用，适合获得可靠的人类相似性判断。
- **比较逻辑**：让模型对相同概念三元组产生相似性判断，进而将模型的表征空间与人类行为数据所反映的表征结构进行比较。
- **考察变量**：论文系统关注多个“计算成分”，包括：
  - 模型架构（architectures）；
  - 是否经过指令微调（instruction-finetuning）；
  - 注意力头的维度（dimensionality of attention heads）；
  - 激活函数的选择；
  - 是否经历多模态预训练；
  - 模型参数量；
  - 训练数据集构成。
- 方法论的技术细节并不只是看任务分数，而是通过表征空间内部结构的相似度来判断“类人性”；摘要中未给出具体的相似度指标或损失函数公式，但整体流程属于“模型表征 — 人类表征”的嵌入空间比较。

### 3. 实验设计

- 评估对象：超过 70 个模型（over 70 models），模型的架构、微调方式和训练数据差异很大，构成大范围的模型矩阵。
- 人类基准：以 THINGS 数据库中的概念为材料，采用三元相似性任务获取人类概念表征结构。
- 模型任务：让模型完成同样的三元组判断或产生相应的表征相似性，再与人类数据求对齐。
- 对照基准：论文进一步检查了模型的对齐分数与现有 LLM 基准得分之间的关系，涉及 MMLU 与 MUSR 等公开基准。
  - 结果显示 MMLU 这类知识与推理基准与表征对齐的相关性相对较好；
  - MUSR 等任务则较差；
  - 但没有任何现有基准能解释对齐分数的全部方差。
- 对比方式：本质上是“多模型、多训练条件、多架构”下的模型间比较，不是与特定的另一类方法做对抗式对比。

### 4. 资源与算力

- **论文提供的 PDF 正文实际上无法获取**（OpenReview 页面被验证码拦截），因此只能基于摘要与元数据总结。
- 在现有可见内容中，论文**没有说明**所使用的 GPU 型号、数量、训练时长、推理成本或硬件平台。
- 由于本研究以“已有模型的评估”为主而非以大规模训练为主，除非原文在实验部分公开了具体硬件信息，否则我们无法了解其实际算力开销。这个问题需要查看论文全文才可回答。

### 5. 实验数量与充分性

- 实验规模较大：超过 70 个模型，覆盖面包括了较广的架构与训练条件，这比通常在 3–5 个模型上比较的工作更有优势。
- 实验任务：只有一个核心评估任务（triplet similarity），但该任务本身来自认知科学，具有较好的效度基础。
- 对照实验：论文同时比较了多种计算成分，等效于一种自然条件下的“横断面消融”：例如比较是否指令微调、不同激活函数、是否多模态预训练等。
- 公平性评价：
  - 由于使用多来源已发布的模型，不同模型之间的训练数据规模、公司/社区训练策略、评测时的解码设置等难以完全控制，会引入混淆因素；
  - 论文若未做扰动分析或重复运行，则无法完全排除这些变量之间的相关性；
  - 70+ 个模型已经为该类研究提供了较高的统计效力，但从认知建模角度看，仅 THINGS 英文概念可能无法覆盖抽象概念、跨语言、跨文化差异。
- 总体认为：可见摘要足以表明实验设计具有较好的广度和客观性；但若要判定“完全充分”，还需要阅读全文中的数据集构建细节、模型清单和统计检验设计。

### 6. 论文的主要结论与发现

- **最有利于类人表征的成分**：经过指令微调的模型，以及注意力头维度更大的模型，与人类概念表征的对齐程度最高。
- **影响较小的成分**：激活函数类型、是否多模态预训练、参数量大小与类人表征对齐的关系有限。
- **现有基准无法替代对齐评估**：传统基准（尤其 MUSR）无法解释表征对齐分数的主要差异；MMLU 能力稍好，但仍然不足。
- **方向性判断**：文本智能并不自动带来概念层面的类人抽象表征；必须要有对齐导向的训练或架构因素，模型才更像人类的概念系统。
- **建议意义**：未来 LLM 若要作为人类认知模型，应把“表征对齐”作为核心评估维度之一，而不应仅把任务准确率当作模型智能的代理指标。

### 7. 优点

- 方法论扎实：使用认知科学中成熟的三元相似性任务，避免单纯借鉴 NLP 基准，从人类行为层面度量概念结构。
- 评估视角创新：将“表征对齐”而不是“任务正确率”作为主要因变量，直接呼应类脑与类人概念空间的研究目标。
- 模型覆盖面广：对 70+ 个模型做系统比较，使其结论比针对少数模型的案例分析更有概括力。
- 因素筛选明确：论文区分了“有关”（指令微调、注意力维度）和“无关”（激活函数、参数量等）因素，具有较好的解释力。
- 直接回应基准缺口：通过对齐分数与传统基准分数的关联分析，展示出已有 LLM 研究中评测盲区，并提出可用的替代评估方向。

### 8. 不足与局限

- 信息源的局限：此处能获得的仅为摘要与元数据，OpenReview 的全文 PDF 被验证页阻挡，因此缺少实验完整细节，如具体模型清单、人类受试者人数、相似度指标与统计检验方式等。
- 概念域限制：THINGS 数据库侧重日常物体概念，未必能代表数学、抽象社会概念或专业领域概念，所得“类人对齐”可能需要更广泛的概念材料才能外推。
- 跨语言与多模态问题：论文提到多模态预训练与人类对齐关系不大，但没有说明模型评测是基于文本还是图像输入；跨语言条件下的观念结构是否与英语一致亦不清楚。
- 混淆变量控制：超过 70 个已发布模型在训练集、训练目标、预训练规模等方面并不统一，指令微调的效果可能与数据质量或品牌家族相关联。
- 对齐方向问题：人类概念相似性判断本身存在个体差异、任务背景依赖，单一 THINGS 三元任务得到的“人类基准”也只是一种统计平均，不能代表所有人群。
- 因果性说明受限：论文更多是发现相关关系，比如“指令微调与更高对齐相关”，但若要说明“为什么”有效，还缺少干预性实验或更严格的消融。
- 应用限制：即使模型在概念表征上与人类更接近，也不等同于模型语言生成一定更好、更稳健或更可解释；对齐分数需要与其他评测维度结合使用。

（完）
