---
title: Do Large Language Models Think like the Brain? Sentence-Level Evidences from Layer-Wise Embeddings and fMRI
title_zh: 大语言模型是否像大脑一样思考？基于分层嵌入和fMRI的句子级证据
authors: "Yu Lei, Xingyang Ge, Yi Zhang, Yiming Yang, Bolei Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37022/40984"
tags: ["query:abstraction"]
score: 8.0
evidence: 句子级LLM嵌入与fMRI信号对齐，揭示与大脑语义表征的对应关系
tldr: 论文关注大语言模型是否真正像大脑一样理解句子。作者将14个模型的逐层嵌入与人类阅读自然语句时的fMRI信号对齐，检验脑样模式是源于规模还是架构一致性。结果显示模型各层与大脑语言区动态响应存在系统的层级对应，指出部分模型层能较好地预测脑活动。该工作为评估和构建与人类语义理解一致的类脑语言模型提供了句子级比较框架。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 理解大语言模型与大脑是否共享相似的计算机制，以及类脑模式是否仅来自规模扩大。
method: 对14个公开LLM进行分层嵌入提取，并与被试在自然句子理解中的fMRI时间响应进行句子级逐层比对。
result: 揭示LLM各层与大脑语言区动态神经响应之间存在系统性层级对应，部分层可较好预测脑活动。
conclusion: 证明LLM与大脑的句子级语义对齐可能源于深层处理结构，为类脑AI表示与脑对齐语义空间研究提供新证据。
---

## Abstract
Understanding whether large language models (LLMs) and the human brain converge on similar computational principles remains a fundamental and important question in cognitive neuroscience and AI. Do the brain-like patterns observed in LLMs emerge simply from scaling, or do they reflect deeper alignment with the architecture of human language processing? This study focuses on the sentence-level neural mechanisms of language models, systematically investigating how layer-wise representations in LLMs align with the dynamic neural responses during human sentence comprehension. By comparing hierarchical embeddings from 14 publicly available LLMs with fMRI data collected from participants, who were exposed to a naturalistic narrative story, we constructed sentence-level neural prediction models to identify the model layers most significantly correlated with brain region activations. Results show that improvements in model performance drive the evolution of representational architectures toward brain-like hierarchies, particularly achieving stronger functional and anatomical correspondence at higher semantic abstraction levels. These findings advance our understanding of the computational parallels between LLMs and the human brain, highlighting the potential of LLMs as models for human language processing.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **核心问题**：大语言模型（LLM）与人类大脑在句子级语言加工中是否共享相似的计算原理？LLM 中出现的“类脑”表征模式，究竟是仅来自模型规模扩大，还是体现了与人类语言处理结构更深层的对齐？
- **研究背景**：已有研究表明 LLM 表征与脑神经反应存在线性映射关系，但这缺乏机制性解释；同时，许多公开脑解码数据集无法真实反映模型在具体任务中的语义理解能力。
- **整体含义**：作者以句子为单位，将 14 个公开 LLM 的逐层嵌入与人类在自然叙事刺激下的 fMRI 信号进行系统比较，希望从“表征架构是否趋同于脑”的角度回答上述问题，并探讨类脑对齐的来源。

## 2. 方法论：核心思想、关键技术细节

- **总体思路**：构建一条“fMRI 预处理 → GLM 句子级响应估计 → ROI 提取 → LLM 分层嵌入计算 → 岭回归预测 → 相关性分析”的多阶段流水线。
- **数据与刺激**：使用《小王子》多语言自然主义 fMRI 语料库（Li et al. 2022），被试听 99 分钟中文有声书，文本被切分为 1577 个句子；被试 35 人，排除 1 人后为 34 人（右利手、母语中文）。
- **fMRI 获取与预处理**：3T GE Discovery MR750；AFNI 预处理，去前 4 个时间点、ME-ICA 去噪、配准到 MNI 标准空间；后续用 GLM 估计每个句子的 BOLD 响应，采用最小二乘分离（LS-S）方案减少共线性。
- **ROI 提取**：采用 Fedorenko 等提出的语言网络，每半球 6 个 ROI（IFGorb、IFG、MFG、AntTemp、PostTemp、AngG），共 12 个 ROI，作为脑-模型对齐的分析框架。
- **分层嵌入**：将句子输入 14 个不同深度的 LLM，提取每层所有 token 的隐状态并在句子上取平均，形成“层 × 维度”的张量。
- **回归与评价（公式/算法文字说明）**：
  - GLM：\(Y = X\beta + \epsilon\)，设计矩阵由事件起始时间与血氧响应函数（HRF）卷积构造。
  - 岭回归：对每个感兴趣区按层训练解码/编码模型，\(\hat\beta=(X^\top X+\alpha I)^{-1}X^\top y\)，并通过嵌套交叉验证网格搜索超参 \(\alpha\)。
  - 相关性指标：每层预测值与被试真实 fMRI 响应的皮尔逊相关系数，在 K 折上取平均。
  - 标准化：y 与嵌入矩阵均做零均值、单位方差标准化；计算并行化覆盖被试×ROI×层，训练 O(SRL) 个模型。
- **评估 LLM 语义理解能力的新指标**：
  - 提出“跨语言语义对齐准确率”（CSAA）：对每个中文句子生成 5 个英文候选（正确翻译、词序打乱、词性替换、句法改造、信息插入/删除），计算原句嵌入与各候选嵌入的余弦相似度，若最大相似度对应正确翻译则记为正确，最终统计正确率。公式为 \(CSAA=\frac{1}{N}\sum_i \delta_i\)。

## 3. 实验设计：数据集、基准与对比方法

- **数据集/场景**：《小王子》fMRI 自然主义叙事语料（中文听力＋英文翻译对照），以及同一批刺激句上的跨语言语义对齐任务。
- **模型对象**：14 个公开 LLM，包括 Llama-3.1-8B(-Instruct)、mistral-7B(-Instruct)、Qwen2.5-7B(-Instruct)、gemma-2-9b(-it)、glm-4-9b(-chat-hf)、Baichuan2-7B-Chat、DeepSeek-R1-Distill-Qwen-7B、opt-6.7b 以及 bert-base-uncased（作为基准）。
- **对比设计**：
  - 模型间比较：按 CSAA 性能、与 12 个 ROI 的平均脑相关比较。
  - 分层比较：每个模型不同层对脑活动的预测相关性曲线（含 95% 置信区间）。
  - 指令微调与基础版对比：选取 5 对“base/instruct”模型，比较脑相关与 CSAA 性能变化。
  - 性能—脑对齐关系：检验模型 CSAA 分数与神经相关之间的皮尔逊相关。
  - 左右半球不对称分析：比较不同 ROI 的左-右半球相关差异，并与模型性能做相关分析。
- **基准/指标**：CSAA 正确率；脑-模型相关性（皮尔逊 r）；左-右半球差异；指令微调提升幅度的置换检验 p 值。

## 4. 资源与算力

- **论文没有明确说明**训练/推理所用的 GPU 型号、数量或总时长。
- 只提到分析流程并行化地训练大量岭回归模型（“O(SRL) 个模型”），并开源代码（GitHub: Lucasuuu02/LLM4Brain），但未给出硬件资源清单或能耗统计。

## 5. 实验数量与充分性

- **主要实验组数**：
  - 三组核心实验：① LLM 跨语言语义对齐性能（CSAA）；② 各模型/层级与 fMRI 的相关，覆盖 12 个 ROI；③ 左右半球不对称及其与模型性能的关系。
  - 另有 5 对基础版-指令微调模型的直接对比、多个统计检验（置换检验 p=0.03125；t 检验和皮尔逊相关 p 值）。
- **充分性与客观性评价**：
  - 优势：模型种类多（14 个）、逐层分析（约 32–40 层）、ROI 覆盖双侧核心语言区、统计检验较多样。
  - 不足：
    - fMRI 语料仅一部叙事作品（《小王子》）和一种语言条件，跨域泛化结论有限。
    - CSAA 总体正确率很低（最高 31.4%，随机基线 20%），脑相关性绝对值也只有 0.05–0.10 左右，虽统计显著但效应量小。
    - 相关分析在多个 ROI、多个层上重复进行，文中未系统做多重比较校正。
    - 参数规模与架构混杂（bert-base 与 7B-9B 模型差异大），性能—脑相关的关系只在 6.7B–9B 的窄区间内检验。

## 6. 主要结论与发现

- **中间层最佳**：所有 LLM 表现出一致规律——中间层的表示与脑活动相关性最高，而非最后一层；说明后期任务导向层更抽象化，与神经表征偏离更大。
- **指令微调增强类脑对齐**：指令微调版本在 CSAA 和脑相关性上都优于基础版；性能提升的置换检验显著（p=0.031），脑相关提升也呈边缘显著。
- **性能驱动的脑样层级架构**：在 6.7B–9B 参数区间内，LLM 的语义理解能力（CSAA）与平均脑相关显著正相关（r=0.601，p=0.030），提示“能力提升”而非单纯“规模扩大”是驱动脑对齐的关键因素。
- **半球功能不对称**：在 IFG 和 PostTemp（核心语言区）呈现左半球优势（与句法和语义整合相关）；在 MFG 与 AntTemp（前额控制、多模态语义）呈现右半球优势。左-右半球差异更大的 IFG/MFG 与更高 CSAA 存在正相关趋势（IFG: r=0.54, p=0.055；MFG: r=0.50, p=0.084），提示前额叶的偏侧化可能促进更高效的语言表征。
- **总体推断**：LLM 与大脑的句子级语义对齐很可能源于深层处理结构上的相似性，而非简单的规模复制；LLM 可作为研究人类语言处理的计算模型。

## 7. 优点：方法与实验设计的亮点

- **系统化逐层粒度分析**：覆盖 14 个模型的多层嵌入，得到“层-脑相关”曲线，揭示中间层峰值的稳健规律。
- **跨语言与自然主义刺激结合**：以同一文本的中文听力 fMRI 与英文翻译作为语义对齐任务，兼顾神经信号与行为语义验证。
- **提出 CSAA 指标**：在嵌入空间中量化模型跨语言语义对齐能力，为脑对齐研究提供更直接的任务相关度量。
- **强调能力而非规模**：通过指令微调版本的严格配对比较，将讨论从“模型大小/参数”转向“语义理解能力”，并得到与脑对齐显著相关的证据。
- **神经科学理论关联强**：从半球偏侧化角度解释模型-大脑对应关系，与经典语言网络理论（左半球主导句法/语义、右半球参与认知控制）形成互证。
- **可复现性支持**：公开代码（GitHub），实验细节（如 ROI 定义、GLM/LS-S、岭回归正规化）描述清晰。

## 8. 不足与局限

- **语料与语言覆盖狭窄**：仅用《小王子》单一中文听故事语料及对应英文文本，对其他语言、文体、话语长度的泛化能力不强。
- **效应量较小**：CSAA 普遍较低（某些模型甚至接近 0 或等于 0），脑相关绝对值也处于 0.02–0.10 范围内；结论虽然统计显著，但实际解释力需谨慎。
- **因果机制不明确**：相关与线性编解码不能证明 LLM 真的“像大脑一样运作”，模型与大脑可能在共享统计表征的同时采用不同机制。
- **统计多重比较风险**：大量 ROI、层和模型的重复检验缺少全局多重比较校正（例如 FDR/置换域校正），部分 p 值可能为假阳性。
- **硬件资源与效率信息缺失**：未报告评估或训练使用的 GPU 数量/型号/时长，不利于经济成本评估。
- **模型选择偏置**：在比较模型平均相关时“选取其最优层作代表”，可能高估特定模型的脑匹配度，需在更大范围验证。
- **参数范围有限**：“能力而非规模”的结论只在 6.7B–9B 的窄范围内得到相关性证据，是否适用于更大或更小的模型仍未知；而 BERT 等小型基准的参与更增加了架构异质性混淆。

（完）
