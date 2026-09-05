---
title: "Concepts from Representations: Post-hoc Concept Bottleneck Models via Sparse Decomposition of Visual Representations"
title_zh: 表征生成概念：基于视觉表征稀疏分解的事后概念瓶颈模型
authors: "Shizhan Gong, Xiaofan Zhang, Qi Dou"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42424/46385"
tags: ["query:abstraction"]
score: 4.0
evidence: 稀疏分解视觉表征得到概念瓶颈，实现从表征到概念的可解释映射，但非针对语义空间本身。
tldr: 概念瓶颈模型通常需要在模型中预先嵌入人工概念标签，昂贵且易受先验影响。本文提出PCBM-ReD，通过稀疏分解预训练模型的视觉表征来获得可解释概念瓶颈，无需人工视觉定义或模型重训。该方法面向任意已训练的分类器，自动提取可视化概念并给出概念相关性解释。结果说明稀疏分解可在不干扰原模型的情况下重建人类可理解的概念表示，有助于事后概念分析。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 已有概念瓶颈模型依赖昂贵且不通用的人工概念定义，且不易适配预训练模型。
method: 对预训练视觉表征做稀疏分解以识别概念，通过概念瓶颈实现事后可解释预测。
result: 在图像识别模型上能够自动提取可视化概念并提高概念相关性可靠性。
conclusion: 证明稀疏分解可以从预训练视觉模型中恢复概念级解释，增强深度学习透明度。
---

## Abstract
Deep learning has achieved remarkable success in image recognition, yet their inherent opacity poses challenges for deployment in critical domains. Concept-based interpretations aim to address this by explaining model reasoning through human-understandable concepts. However, existing post-hoc methods and ante-hoc concept bottleneck models (CBMs), suffer from limitations such as unreliable concept relevance, non-visual or labor-intensive concept definitions, and model/data-agnostic assumptions. This paper introduces  Post-hoc Concept Bottleneck Model via Representation Decomposition (PCBM-ReD), a novel pipeline that retrofits interpretability onto pretrained opaque models. PCBM-ReD automatically extracts visual concepts from a pre-trained encoder, employs multimodal large language models (MLLMs) to label and filter concepts based on visual identifiability and task relevance, and selects an independent subset via reconstruction-guided optimization. Leveraging CLIP’s visual-text alignment, it decomposes image representations into linear combination of concept embeddings to fit into the CBMs abstraction.  Extensive experiments across 11 image classification tasks show PCBM-ReD achieves state-of-the-art accuracy, narrows the performance gap with end-to-end models, and exhibits better interpretability.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：深度学习模型在图像识别中性能优异，但由于其“黑箱”本质，难以在医疗、自动驾驶等高风险领域可靠部署。概念级解释（concept-based interpretation）通过人类可理解的概念解释模型推理过程，是解决该问题的关键思路。
- **已有方法的缺陷**：
  - **事后方法（post-hoc）**：提取的概念往往不能忠实反映网络真实推理过程，概念与类别间缺乏直观因果联系，且自动挖掘可能产生大量低语义价值的概念，难以人工标注。
  - **事前方法（ante-hoc，即 CBM）**：严重依赖人工定义概念（昂贵、覆盖不全），或 LLM 生成的概念包含大量不可视化特征（如味道、行为）；同时这些预定义概念是数据无关、模型无关的，忽视了特定数据分布与骨干网络编码能力的约束；已有 CBM 一般不保证概念间相互独立，妨碍有效干预。
- **论文定位**：提出 **PCBM-ReD（Post-hoc Concept Bottleneck Model via Representation Decomposition）**——一种在不改变预训练黑箱模型参数的前提下回溯式注入可解释性的新管线，将训练好的视觉编码器转化为符合 CBM 抽象的可解释模型。

## 2. 论文提出的方法论

- **总体思想**：从预训练图像编码器的表征中“挖掘”概念 → 用多模态大模型（MLLM）标注与筛选概念 → 选出能重建原始表征的最小独立概念子集 → 借 CLIP 的视觉-文本对齐特性，将图像表征稀疏分解为一组概念文本嵌入的线性组合 → 在拟合表征上训练线性分类层，从而满足 CBM 的可解释性约束。

- **三个阶段（对应论文第 3 节）**：

  1. **数据驱动的概念发现（Data-driven Concept Discovery）**：
     - 以 CLIP 的图像编码器 I 和文本编码器 T 为基础；对图像 xᵢ 得到表征 Iᵢ = I(xᵢ) ∈ ℝᵈ。
     - 使用**稀疏自编码器（SAE）**将 Iᵢ 稀疏编码为概念字典原子的线性组合（Iᵢ ≈ V uᵢ，uᵢ = ψ(Iᵢ)），以缓解多义性（polysemanticity），字典 V 的每一列对应一个候选概念，uᵢ 相应维度表示该概念的重要性。
     - 对于每个自动提取的概念，选取其得分最高的 Top-K 图像作为“探针图”，用 MLLM（Llama-3.2-11B-Vision-Instruct）做 chain-of-thought 图像描述，再用 LLM（DeepSeek-V3）汇总描述并生成候选概念、按“可视觉识别、有区分度、无捷径/背景伪相关”等标准打分，过滤低质概念。

  2. **重建引导的概念选择（Reconstruction-guided Concept Selection）**：
     - 目标函数为最小化概念文本表征（R(C)）重建图像表征的 Frobenius 范数误差：  
       min_C Σᵢ min_{βᵢ(C)} ‖ Iᵢ − R(C)ᵀβᵢ(C) ‖²_F ；
     - 这是一个无封闭解的离散优化问题，论文给出**贪心选择算法**（Algorithm 1）：每次挑选使重建误差下降最多的新概念加入集合，过程中利用投影与残差更新的技巧避免穷举式求解，并利用线性相关性检查（z = 0）剔除与已有概念线性相关的候选；当达到预设瓶颈大小或无可选新概念时停止。
     - 该选择过程完全无监督，因而可用于零样本/少样本场景。

  3. **事后类-概念关联（Post-hoc Class-concept Association）**：
     - 利用 CLIP 的联合嵌入空间，将图像表征写成概念文本嵌入的线性组合加残差：  
       Iᵢ ≈ Îᵢ = Σⱼ wᵢⱼ cⱼ；  
       使用**正交匹配追踪（OMP）**做稀疏编码，强制每个图像只用少量（n 个）非零系数解释，残差被丢弃。
     - 在拟合表征 Îᵢ（而非原始 Iᵢ）上训练线性分类层 ŷᵢ = argmax(WᵀÎᵢ + b)，其数学形式可重写为基于概念得分的 CBM 形式，故天然满足 CBM 抽象。
     - 权重矩阵 W 初始化为文本嵌入“This is a photo of [cls]”以继承 CLIP 的零样本能力。

## 3. 实验设计

- **评测基准**：沿用 Yang et al. (LaBo) 提出的统一 benchmark，覆盖 **11 个图像分类数据集**，跨领域包括：通用物体（ImageNet、CIFAR-10/100）、细粒度分类（Food-101、FGVC-Aircraft、Flower-102、CUB-200-2011）、动作识别（UCF-101）、纹理（DTD）、医学皮肤病变（HAM10000）、卫星图像场景识别（RESISC45），使用一致的 train/dev/test 划分。
- **主实验设置**：
  - 全监督设置：默认使用 CLIP ViT-L/14（OpenCLIP），对比线性探针、LaBo、Res-CBM、V2C-CBM；另设一组以 CLIP RN50 为骨干，与更多 CBM 基线（原版 CBM、CompDL、PCBM、Label-free CBM、CDM、DN-CBM、VLG-CBM）直接对比。
  - 零样本设置：在无需训练标签的设定下，使用 vanilla CLIP 提示和 CuPL 两种策略。
  - 少样本设置：按 CLIP 的评估协议，每类随机取 1、2、4、8、16 张图，对比 LaBo。
- **评测维度**：主实验为测试集分类准确率；另有人类评估（39 名志愿者对 5 个数据集的解释质量从 3 个维度打分：概念是否可视觉辨识、是否忠实描述图像、解释与预测是否存在因果关联）；以及面向解释的定性可视化示例。

## 4. 资源与算力

- 论文只在“Implementation Details”提到使用 **NVIDIA GeForce RTX 4090 GPU**，采用 Adam（batch size 64，学习率 5×10⁻⁵）训练线性头，并公开了代码仓库（https://github.com/peterant330/PCBM-ReD）。
- **论文未明确报告** GPU 数量、总训练时长、显存占用等具体算力信息，也未给出各步骤（SAE 训练、MLLM 标注、概念选择、线性层训练）分别的计算开销，这是资源复现与成本评估方面的信息缺口。

## 5. 实验数量与充分性

- **实验量较为充足**，主要包括：
  1. 11 个数据集上的全监督 + 零样本主实验；
  2. 与 10 余种 CBM 基线（含 LaBo、Res-CBM、V2C-CBM、Label-free CBM 等）的横向对比；
  3. 少样本学习实验（5 个尺度的 shot 数）；
  4. 4 组消融/分析实验：概念来源（SAE 提取 vs LLM 生成 vs WordNet）、概念选择算法（贪心重建引导 vs 随机 vs K-Means）、概念-图像得分关联方式（稀疏分解 vs CLIP 相似度）、概念提取来源骨干与分类骨干是否匹配（mismatch 实验）、以及 bottleneck 大小敏感性；
  5. 39 人的人类可解释性评估与定性可视化。

- **充分性评价**：
  - 优点：覆盖数据集类型广、基线数量多且时代较新、消融设计能较好地分离每一步的贡献、跨两类骨干（ViT-L/14 与 RN50）验证。
  - 不足：少样本与消融实验主要报告平均精度，部分完整结果放到附录；零样本与少样本仅与 LaBo 做了少数对比，未覆盖更多零样本 CBM 方法；人为评估样本随机性、标注者专业背景等细节需依赖附录说明。整体而言，实验在广度上是充分的，但部分深层对比（如领域特异数据集、更细粒度误差分析）仍可加强。

## 6. 论文的主要结论与发现

- 在 11 个数据集的平均精度上，PCBM-ReD 达到 **86.97%**（ViT-L/14 全监督），比线性探针仅低 **0.41%**，且部分数据集上甚至超过端到端线性探针；
- 相对其他 CBM：比 LaBo 平均高 1.25%，比 Label-free CBM 高 5.57%，在 RN50 骨干上也优于 CompDL、CDM、DN-CBM、VLG-CBM 等基线，是目前性能最好的解释性概念瓶颈方案之一；
- 由于拟合表征保留了零样本性质，PCBM-ReD 的零样本精度与 CLIP 几乎一致（同时具备可解释性），这是大多数既有 CBMs 不具备的；少样本场景平均稳定高于 LaBo（平均领先 5.01%）；
- 人类评估显示：从图像描述中提取的概念比纯 LLM 先验生成的概念更视觉化、更忠实；概念打分筛选环节能剔除因果性弱的概念，显著提高解释质量；
- 消融表明：概念来源与视觉编码器匹配很重要；重建引导选择优于 K-Means 与随机；基于稀疏分解的概念得分优于 CLIP 相似度得分；Bottleneck 为 300 左右时可接近性能饱和，仅 50 个概念也有合理精度。

## 7. 优点

- **概念质量高**：概念直接从预训练编码器的表征中通过 SAE 提取，而不是脱离数据凭语言先验生成——更贴合数据分布和编码器表达能力；MLLM 标注/筛选进一步剔除不可视化、含捷径伪相关的概念。
- **性能-可解释权衡出色**：利用 CLIP 同构空间中图像/文本嵌入可线性分解的性质做稀疏重建，把残差从一开始就压缩到很低，显著缩小了 CBM 与端到端模型的性能差距（不同于 PCBM 靠额外残差连接补性能）。
- **概念独立性有保障**：重建引导的贪心选择结合线性相关性判据，有效控制概念之间的冗余，有利于 CBM 的事后干预，是此前 CBM 工作中常被忽略的关键点。
- **无需人工标注、无监督概念选择**：整个训练过程不依赖人工概念标注，且选择/分解过程可用于零样本与少样本，继承 CLIP 的多模态迁移能力。
- **广泛验证**：在 11 个任务、多种数据类型上系统评估，并以人类研究证明了实际决策者可感知的解释性提升；消融实验比较周全，能清楚看出各模块贡献。

## 8. 不足与局限

- **依赖通用 MLLM/LLM 描述能力**：在 HAM10000 这类专业医学图像上，通用 MLLM 缺乏领域词汇会导致概念描述不精确，作者也承认需要领域专用 MLLM 或更好提示方能改进；
- **受限于原始编码器**：作为事后方法，其性能上界是原预训练表征的质量（如 CLIP 对空间关系、纹理等特征的短板）；论文中 backbone mismatch 实验也表明概念与编码器不匹配时性能明显下降；
- **多种超参数需预设**：SAE 字典大小、Top-K 探针图数、瓶颈大小 m、稀疏编码的非零系数个数 n、MLLM 评分阈值等均需人工调节，论文未系统讨论其对最终性能的敏感性；
- **概念边界依赖 OMP 线性稀疏分解的完备性**：残差被直接丢弃，若

……残差中蕴含与任务高度相关的判别性信息，则稀疏线性近似会丢失这部分信号，从而在概念空间完备性不足的数据集上造成精度损失；同时，图像表征与概念文本嵌入之间“线性可分解”的假设虽在 CLIP 空间中得到部分验证，但仍属于一种近似，可能无法捕捉概念间复杂的非线性交互（如空间共现、遮挡、层级组合），这类交互恰恰是部分细粒度识别任务的关键线索。

- **概念语义颗粒度与数量不够灵活**：贪心选择出的 top-m 概念以“能重建表征”为目标，而非以“类间可分”为目标，因此挑选出的概念可能在语义上偏向全局统计特征，忽略某些稀有但判别力强的局部概念；固定的 bottleneck 大小也无法随类别数或领域复杂度自适应调整。
- **多阶段管线存在误差累积**：SAE 稀疏编码的近似误差 → MLLM 描述与 LLM 归纳的概念化误差 → 贪心重建选择的子集误差 → OMP 稀疏拟合误差 → 线性头分类误差，每个环节都可能引入有偏信息，而论文未对各阶段误差做端到端的联合传播分析，也未提供不确定度估计。
- **人类评估的标准化程度有限**：尽管有 39 名志愿者参与，但标注者是否具备机器学习背景、回答是否存在锚定效应、评分量表是否经过信度检验（如 Cohen's κ 或 ICC）等问题在正文中未充分交代，削弱了“解释质量更好”这一结论的证据强度。
- **对 CLIP 骨干的依赖较强**：整套方法建立在 CLIP 联合嵌入空间之上（既要用 CLIP 视觉骨干提取表征，又要用文本编码器投影概念），若将其迁移到不具备强图文对齐能力的普通 CNN/Transformer 骨干，概念文本表征与图像表征将不再可比，方法的核心机制——线性稀疏重建——将失去数学基础；RN50 实验本质上仍是基于 CLIP 的 RN50，并未脱离该框架。

## 9. 与已有研究的深层关系

- **对 PCBM 的继承与修正**：传统 PCBM 用稀疏编码（semi-discrete matrix factorization 等）从预训练特征中提取概念，但概念需人工标注，且概念权重与分类之间存在残差连接，导致概念瓶颈约束被旁路。PCBM-ReD 用 MLLM 完全取代人工标注，并在拟合表征上训练分类头，从而在不牺牲可解释约束的前提下规避了残差旁路，是对“事后概念瓶颈”范式的实质性推进。
- **对 Label-free CBM 的路径差异**：Label-free CBM 用 GPT-3 生成概念再以 CLIP 匹配视觉特征，本质是“语言先验 → 视觉验证”；PCBM-ReD 则相反，是“视觉表征 → 语言命名 → 视觉重建验证”。前者受制于语言模型对未知视觉模式的盲区，后者受制于 MLLM 对隐式表征的命名覆盖率，两者可视为互补。
- **对 SAE 可解释性研究的承接**：近年来关于 SAE 的文献认为，SAE 能从视觉 Transformer 中解离出可解释的、近乎单义的稀疏特征。PCBM-ReD 是将这一“特征字典即概念”的思想推向端到端可解释分类器的关键一步，其重建引导选择机制实际扮演了“字典原子 → 人类语义概念”的桥接角色。
- **对 CLIP 空间线性结构的挖掘**：论文隐含地再次验证了 CLIP 对齐空间中文本嵌入可张成图像表征主要信息子空间这一经验事实，这为未来利用联合嵌入做受控干预、反事实解释提供了有力支撑。

## 10. 总体评述与启示

- **一句话概括**：PCBM-ReD 通过“SAE 提取候选概念 + MLLM 语义化标注与筛选 + 重建引导的贪心选择 + OMP 稀疏分解 + CBM 化线性头”这一模块化管线，构建了首个不依赖人工标注、不修改预训练模型参数且逼近端到端精度的纯事后概念瓶颈模型。
- **最大的方法论启示**：可解释性不必以牺牲性能为代价，也不必在“事后分析”与“事前可解释模型”之间二选一——通过表征分解可以把事后提取的概念以约束形式重新注入模型下游，从而同时获得两种范式的优点。该思路在 CLIP 类多模态模型日益成为视觉骨干的当下尤其具有推广价值。
- **对高风险场景的潜在意义**：若配合领域专用视觉-语言模型对医学、遥感数据校准概念词汇，PCBM-ReD 所提供的“输入图像 → 稀疏概念权重 → 线性决策”链条，可使临床医生或领域审核员以极低认知成本审查模型依据，并可通过修改概念权重进行人为干预，为可审计 AI 提供了一条现实的落地路径。
- **值得进一步探索的方向**：①用概率模型代替确定性 OMP，以表征残差的置信区间；②引入概念之间的语义层级约束，使选择出的概念非但线性无关，而且结构化；③将概念选择从“重建导向”升级为“重建与可判别性的多目标导向”，以拾回被遗漏的低频判别概念；④设计端到端的可微稀疏编码以融合各阶段误差；⑤在更大规模（如 ImageNet 全类别、多标签医疗数据集）与真实使用场景（用户实际干预操作）中检验方法的鲁棒性与有用性。

（完）
