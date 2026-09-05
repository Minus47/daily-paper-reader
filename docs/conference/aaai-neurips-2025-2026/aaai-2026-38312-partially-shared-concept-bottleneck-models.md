---
title: Partially Shared Concept Bottleneck Models
title_zh: 部分共享概念瓶颈模型
authors: "Delong Zhao, Qiang Huang, Di Yan, Yiqun Sun, Jun Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38312/42274"
tags: ["query:abstraction"]
score: 4.0
evidence: 部分共享概念瓶颈模型自动生成与合并概念，可为构建可解释概念词库提供方法。
tldr: 概念瓶颈模型依赖清晰可解释的概念层，但现有自动生成方法存在视觉锚定弱、概念冗余等问题。PS-CBM提出多模态概念生成器，把LLM语义与图像示例特征结合，并采用部分共享策略按激活模式合并概念，兼顾特异性与紧凑性。该方法改善视觉语义锚定并降低冗余，同时引入新的平衡指标来实现概念表达与预测精度之间的优化。这项工作为可解释概念层的自动构建提供了改进机制，可作为概念库建设的组件。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 自动生成概念瓶颈模型存在视觉锚定差、概念冗余且缺少精度-紧凑性权衡指标。
method: 设计多模态概念生成器与部分共享概念策略，并加入指标平衡分类准确性和概念简洁度。
result: 缓解了概念视觉锚定不足与冗余问题，在不显著牺牲准确率时保持紧凑的概念层。
conclusion: 提供了一种可扩展的概念提取和精简框架，有助于构建结构化可解释的概念表征。
---

## Abstract
Concept Bottleneck Models (CBMs) enhance interpretability by introducing a layer of human-understandable concepts between inputs and predictions. While recent methods automate concept generation using Large Language Models (LLMs) and Vision-Language Models (VLMs), they still face three fundamental challenges: poor visual grounding, concept redundancy, and the absence of principled metrics to balance predictive accuracy and concept compactness. We introduce PS-CBM, a Partially Shared CBM framework that addresses these limitations through three core components: (1) a multimodal concept generator that integrates LLM-derived semantics with exemplar-based visual cues; (2) a Partially Shared Concept Strategy that merges concepts based on activation patterns to balance specificity and compactness; and (3) Concept-Efficient Accuracy (CEA), a post-hoc metric that jointly captures both predictive accuracy and concept compactness. Extensive experiments on eleven diverse datasets show that PS-CBM consistently outperforms state-of-the-art CBMs, improving classification accuracy by 1.0%–7.4% and CEA by 2.0%–9.5%, while requiring significantly fewer concepts. These results underscore PS-CBM’s effectiveness in achieving both high accuracy and strong interpretability.

---

## 论文详细总结（自动生成）

# 论文总结：部分共享概念瓶颈模型（Partially Shared Concept Bottleneck Models）

## 1. 论文的核心问题与整体含义

- **研究背景**：概念瓶颈模型（CBM）通过在输入与预测之间引入人类可理解的概念层来提升模型可解释性。但早期 CBM 依赖人工标注概念，成本高、难以规模化；后续方法虽借助 LLM 与 VLM 自动生成概念，仍存在三个关键挑战：
  1. **视觉锚定差（Poor Visual Grounding）**：LLM 生成的概念语义丰富，却常与真实视觉内容脱节；VLM 生成的概念视觉贴合较好，但类别语义一致性弱、计算成本高。
  2. **概念冗余（Concept Redundancy）**：独立为每个类生成概念会造成语义重复；全局共享概念池则会使不相关类被迫共享概念，削弱判别力。
  3. **缺乏统一度量（Inadequate Metrics）**：多数方法只评估分类精度，忽略概念集合过大或冗余带来的可解释性成本。
- **整体含义**：论文提出 PS-CBM 框架，旨在同时缓解上述三大问题，实现“高精度 + 强可解释性 + 低概念冗余”的可扩展 CBM。

## 2. 论文提出的方法论

### 2.1 整体框架
- PS-CBM 采用三阶段流程：**多模态概念生成 → 部分共享概念策略 → CBM 训练**。

### 2.2 多模态概念生成
- **Few-shot 图像选择**：利用 CLIP embedding 为每个类别挑选少量且多样的示例图像；对噪声数据集（如 Food101）采用随机采样。
- **概念生成**：将文本描述与示例图像组合成 prompt，调用 GPT-4o 两次取结果并去重，得到每个类的候选概念集合 S；每个概念关联一个类别集合 Cj。
- **目的**：融合 LLM 的语义丰富性与示例图像的视觉提示，弥合“语义—视觉”鸿沟。

### 2.3 部分共享概念策略（PSCS）
- **概念过滤**：用 CLIP 图像编码器 Φ 和文本编码器 Ψ 计算图像 xi 与概念 cj 的亲和度矩阵 A，其中 Ai,j = cos(Φ(xi), Ψ(cj))；保留每类 top-4 平均对齐分超过 τconf 的概念。
- **概念合并**：对过滤后的概念计算相关系数矩阵 Q（基于归一化的亲和度向量内积），用贪心算法合并相似度超过 τmerge 的概念；合并后的概念继承原类别集合的并集；每类仅保留最多 K 个“专属概念”，避免过度冗余。
- **概念标注**：为每个图像 xi 构造 one-hot 概念标签 si,j；若 yi ∈ Cj 且 Ai,j > τconf，则置为 1，否则为 0；最终得到带概念标签的数据集 D′。

### 2.4 CBM 训练
- **概念瓶颈层（CBL）**：用冻结的骨干编码器 ϕ 将图像映射为 embedding z，CBL 将 z 映射为概念 logits；训练目标为二值交叉熵（BCE）。
- **最终分类层（FCL）**：固定 CBL，用稀疏线性层将概念 logits 映射为类别预测；目标为交叉熵 + elastic-net 正则，用 GLM-SAGA 优化器求解。

### 2.5 CEA 指标
- 公式：CEA = ACC / (log_k m)^β，其中：
  - ACC 为分类准确率；
  - k = ⌈log₂ l⌉ 为区分 l 类所需理论最少二进制概念数（香农信息论下界）；
  - m 为实际使用概念数；
  - β 为控制概念复杂度的惩罚强度的温度参数。
- **性质**：当 ACC→1 且 m→k 时，CEA→1；base-k 对数缩放使得任务类别数越多，对概念数的宽容度越高；指标是无偏、有界、模型无关的，可事后计算。

## 3. 实验设计

### 3.1 数据集
- 11 个公开数据集，覆盖：
  - **通用图像分类**：CIFAR10、CIFAR100、ImageNet；
  - **细粒度分类**：Aircraft、CUB200、Flower102、Food101；
  - **领域特定任务**：DTD（纹理）、HAM10000（皮肤病变）、Resisc45（遥感）、UCF101（动作识别）。

### 3.2 对比方法与基准
- 与 **8 种代表性 CBM** 对比：LaBo、LF-CBM、LM4CV、DN-CBM、Res-CBM、VLG-CBM、V2C-CBM、DCBM；
- 另设 **Linear Probe** 作为无显式可解释性的强黑盒 baseline；
- 所有方法使用相同的 train/dev/test 划分，主实验 backbone 为 CLIP RN50，另在附录报告 CLIP ViT-L/14 结果。

### 3.3 评价指标
- 分类准确率（ACC）；
- 提出的 Concept-Efficient Accuracy（CEA）；
- CLIP Score（用于评估概念—图像对齐，尤其在 DTD、Resisc45、UCF101 上）。

### 3.4 超参设置
- 过滤阈值 τconf = 0.20；
- 合并阈值 τmerge 在 [0.9996, 0.9999] 内以 0.0001 步长搜索；
- 每类专属概念上限 K ∈ {0, 1, …, 8}；
- CEA 温度参数 β = 0.25；
- 因 ImageNet 规模大，仅采样 10% 训练图像用于概念合并。

## 4. 资源与算力

- 论文报告了实验环境：Ubuntu 22.04、PyTorch 2.3.0、CUDA 12.1、**单张 NVIDIA RTX 3080 Ti（12GB）**、Intel Xeon 4214R CPU（12核 2.40GHz）、90GB RAM。
- 但**未明确说明**每次实验的训练时长、总 GPU 小时数，也未报告 GPT-4o 调用的 API 成本或生成耗时。

## 5. 实验数量与充分性

- **实验数量较丰富**，主要包括：
  - 11 个数据集上与 8+ 种 CBM 的 ACC/CEA 对比；
  - 平均 ACC、概念数量、平均 CEA 的汇总对比；
  - 3 个领域特定数据集上的 CLIP Score 对比；
  - 对 K 的敏感性分析（图 3）；
  - 对独立/部分共享/全局共享三种概念策略的消融（图 4）；
  - 对 τconf 的敏感性消融（表 5）；
  - 可解释性案例研究（图 5）与概念-类别映射可视化（图 6）。
- **公平性**：实现细节中声明所有方法使用相同数据划分和 backbone，超参设定有明确范围；在不同数据集上均取最优设置的比较方式是同领域的常见做法。
- **充分性判断**：实验覆盖面较广，涵盖多模态概念质量、冗余控制、模型精度与可解释性平衡；但部分消融没有报告方差/多次运行稳定性结果，且只在两个 backbone 上验证，规模有限。

## 6. 论文的主要结论与发现

- PS-CBM 在 11 个数据集上相比 SOTA CBM：
  - 分类 ACC 平均提升 **1.0%–7.4%**；
  - CEA 平均提升 **2.0%–9.5%**；
  - 概念数显著更少（例如平均比 DN-CBM 少约 7647 个概念）。
- 多模态概念生成能同时获得语义丰富性和

同时获得语义丰富性与视觉锚定性，从而缓解了 LLM 与 VLM 各自单模态生成路线下的语义—视觉割裂问题。综合来看，论文的主要发现可以概括为以下几点：

- **精度与概念效率可兼得**：实验结果表明，PS-CBM 在 11 个数据集上的平均分类准确率均优于对比的 SOTA CBM，且使用的概念总数显著更少。这说明“概念数量”并非精度的简单代价；通过精心设计概念池的结构，可以在更精简的概念预算下取得更优的判别性能。
- **部分共享优于两个极端**：消融实验显示，独立概念策略（每类完全私有）与全局共享概念策略（所有类共用一个池）各有利弊——前者冗余度高、概念解释覆盖窄，后者强制类间共享、容易引入不相关概念。PS-CBM 的“部分共享”策略通过类别级概念池的重叠与边界控制，在判别性与可解释性之间实现了更优的折中。
- **CEA 指标具备实践区分力**：相比仅报告 ACC，CEA 将概念数纳入统一评价，能够揭示“精度高但概念冗余严重”的方法的真实现；论文用它作为主评价指标后，部分在 ACC 榜上看似接近的方法，在概念效率维度上被明显拉开差距。
- **概念过滤与合并是效率的主要来源**：通过亲和度过滤 + 贪心合并，大量视觉上等价或语义重叠的概念被剔除或归一，最终保留的概念具有更强的类别特异性与视觉一致性；案例研究也显示，滤波后概念与类别的对应关系呈现出合理的“非独占式”语义结构，即某概念可被多个相近类共享，但共享范围被自然约束在语义邻近的类别内。

**局限性与改进方向**（作者未完全展开，但从论文实验中可辨识出以下不足）：

- **概念质量仍依赖 GPT-4o 与 CLIP 的先验**：自动生成的概念即使经过阈值过滤与相似度合并，也缺乏人类层面的真值校验。在类别边界模糊或图像领域高度专门化的数据集上，生成的概念可能存在隐含偏差或语义幻觉，PS-CBM 无法从根本上消除这种来源性噪声。
- **对 fine-grained 类别规模较敏感**：论文中“每类最多 K 个专属概念”的约束，使总概念规模随类别数线性增长；面对类别数极大（如上千类）的开放域任务时，部分共享层的搜索空间与概念集的维护成本仍可能上升。
- **CEA 指标的 β 难以先验确定**：虽然论文在 β = 0.25 下给出了稳定结论，但该参数本质上是“概念复杂度-精度”权衡的主观温度系数，不同应用场景对概念冗余的敏感度不同，该指标的最优 β 可能需要随任务重新标定。
- **实验规模有限**：主实验仅围绕 CLIP RN50 与 CLIP ViT-L/14 两个视觉编码器展开，未使用微调骨干或与 label 微调后的 CBM 管线结合；因此 PS-CBM 对自研骨干和更复杂训练范式的鲁棒性仍缺乏证据。

总体而言，PS-CBM 的核心贡献并非某一组件的单项突破，而是将概念生成、概念选择与模型训练协同设计，提出了一个更具实用导向的可解释分类流程。它将“概念”从固定的人工注释产物转化为可由多模态模型自动构建、由类别先验引导筛选、并以信息论视角量化的动态中层表示，有效回应了 CBM 在规模化落地中的三大障碍。以此为基础，未来工作可进一步探索概念集合的可学习组合、跨任务的概念迁移，以及在人机协同审计场景中对概念正确性与因果忠实性的更深层验证。

（完）
