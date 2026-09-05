---
title: Continuous Thought Machines
title_zh: 连续思维机
authors: "Luke Nicholas Darlow, Ciaran Regan, Sebastian Risi, Jeffrey Seely, Llion Jones"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=y0wDflmpLk"
tags: ["query:abstraction"]
score: 8.0
evidence: 将神经元时序处理与同步作为核心表征的类脑连续思维机器模型
tldr: 多数人工神经网络忽略单个神经元的动态复杂性和时间参数，仅将其视为静态激活单元。该文提出连续思维机器，在每层每个神经元上用独特权重处理其输入历史，并把神经同步作为潜在表征使用，把神经时序重新引入计算基础。这种模型在神经元抽象和生物真实度之间折中，实验显示加入神经同步与时间处理使网络具备更丰富的动态表示，可适用于需要精细时间信息的任务。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 人工神经网络普遍忽略神经元时序及神经同步动态，缺少神经动力学支撑导致计算表达局限性。
method: 将每个神经元设计为具有独立时间权重以处理输入历史，同时利用神经元同步作为潜表征，把动力学引入整个网络结构。
result: 在该模型上开展的实验表明，神经元级时序处理和同步表示可在保持抽象水平的同时获得更强的动态信息建模能力。
conclusion: 这项工作表明，关注神经元时序与同步可以构建更接近生物真实性的类脑计算模型。
---

## Abstract
Biological brains demonstrate complex neural activity, where neural dynamics are critical to how brains process information. Most artificial neural networks ignore the complexity of individual neurons. We challenge that paradigm. By incorporating neuron-level processing and synchronization, we reintroduce neural timing as a foundational element. We present the Continuous Thought Machine (CTM), a model designed to leverage neural dynamics as its core representation. The CTM has two innovations: (1) neuron-level temporal processing}, where each neuron uses unique weight parameters to process incoming histories; and (2) neural synchronization as a latent representation. The CTM aims to strike a balance between neuron abstractions and biological realism. It operates at a level of abstraction that effectively captures essential temporal dynamics while remaining computationally tractable. We demonstrate the CTM's performance and versatility across a range of tasks, including solving 2D mazes, ImageNet-1K classification, parity computation, and more. Beyond displaying rich internal representations and offering a natural avenue for interpretation owing to its internal process, the CTM is able to perform tasks that require complex sequential reasoning. The CTM can also leverage adaptive compute, where it can stop earlier for simpler tasks, or keep computing when faced with more challenging instances. The goal of this work is to share the CTM and its associated innovations, rather than pushing for new state-of-the-art results. To that end, we believe the CTM represents a significant step toward developing more biologically plausible and powerful artificial intelligence systems. We provide an accompanying [interactive online demonstration](https://pub.sakana.ai/ctm/) and an [extended technical report](https://pub.sakana.ai/ctm/paper).

---

## 论文详细总结（自动生成）

# 连续思维机（Continuous Thought Machines）论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 生物大脑处理信息时表现出复杂的神经活动，其中**神经动力学**（包括单个神经元的放电时序、跨神经元的同步等）被认为是信息处理的关键机制。
- 主流人工神经网络（ANN）普遍将单个神经元简化为静态激活单元，忽略了单个神经元的复杂时序处理特性和神经元之间同步动态，导致模型缺乏神经动力学的支撑，认知与推理能力受限。
- 论文挑战这种简化范式，主张**将神经时序重新引入计算基础**，构建一种能够在合理抽象层次上利用神经动力学的类脑计算模型。
- 核心意义在于：人工神经网络不应只追求生物学启发的表层结构（如卷积、注意力），还应吸收生物神经系统更底层的时间动态特性，从而发展更接近生物真实性的、具备更强动态信息建模能力的 AI 系统。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- 论文提出 **连续思维机（Continuous Thought Machine, CTM）**，该模型以神经动力学作为核心表征，主要有两个创新点：
  1. **神经元级时间处理（Neuron-level temporal processing）**：每个神经元使用**独特的权重参数**处理其输入的历史信息，即神经元的输出不仅依赖于当前输入，还依赖于一段时序上的输入动态。这意味着网络中的每个单元本质上是一个小型“记忆/时序处理器”。
  2. **神经同步作为潜在表征（Neural synchronization as a latent representation）**：不同神经元的激活时序关系（如同步激活或相位关系）不仅被视为“副产品”，而是作为模型内部的潜在表示被显式利用，模型可通过同步模式编码信息与进行决策。
- 从抽象层次看，CTM 并没有完全模拟生物神经元（如真实膜电位、离子通道等），而是**在神经元抽象与生物真实度之间折中**：保持了足够的时序分辨率，使模型能够捕捉关键动态特征，同时仍具有可计算性和可训练性。
- 由于模型内部天然涉及时序与同步过程，CTM 具有**自适应计算能力**：对简单任务可以提前终止计算，对复杂任务可以继续计算更多时间步，从而动态调整计算预算。
- 技术架构层面（根据摘要）：模型对所有层和所有神经元引入内部时间维度和权重，输入序列经过带权重的历史处理，最终以网络神经元的“思考时间”实现推理。训练方式及具体损失函数在提供的内容中未展开说明。

## 3. 实验设计：数据集 / 场景、benchmark、对比方法

论文报告了多个不同特性的任务场景：

- **2D 迷宫求解（2D mazes）**：需要空间推理、探索和路径规划，测试模型处理复杂顺序决策的能力。
- **ImageNet-1K 图像分类**：大规模视觉识别基准，用于验证模型在标准静态识别任务上的可扩展性和有效性。
- **奇偶校验计算（parity computation）**：经典顺序逻辑推理任务，需要沿序列进行长程状态保持与逻辑运算，用于测试模型对时间依赖和状态追踪的能力。
- 摘要还提到“更多任务”（and more），具体未展开。
- 实验目的不是刷新SOTA，而是展示 CTM 的**通用性、丰富的内部表征和可解释性**。
- 摘要没有明确说明对比了哪些基线方法；但从上下文推测，至少应与同规模/同架构的前馈神经网络、循环神经网络、带记忆机制的 Transformer 等进行对比。所有对比均无细节，仅提供了交互式在线演示和技术报告链接。

## 4. 资源与算力

- 提供的论文内容中**没有明确说明**使用了哪些 GPU（如型号、数量）以及训练总时长。
- 考虑到 ImageNet-1K 是一个大型 benchmark，训练一定使用了多卡并行（常见如多台 A100/H100），但**具体算力配置和数据不详**。
- 若需要完整算力报告，需查阅论文附带的技术报告（technical report）部分。

## 5. 实验数量与充分性

- 根据摘要文字，论文至少覆盖 4 类不同任务（迷宫、ImageNet-1K、奇偶校验、及其他）。
- 没有给出消融实验的具体信息（例如去掉时间处理或同步表征后性能的变化），但强调两个创新是“核心”，很可能论文正文包含对应分析。
- 实验的目的是验证“能否工作”和“表现出的动态表征能力”，而非“是否达到 SOTA”，因此实验设计在思路上是合理的：通过异构任务集和人工可读的时序状态来观察内部动态。
- **充分性判断**：从摘要来看，实验场景多样性较好（空间、视觉、逻辑），但缺少对比实验细节、消融细节以及超参数敏感性等描述，因此其**公开摘要层面的实验充分性还不足以全面佐证**模型的有效性；更完整证据需依赖后续技术报告和实际代码。
- 论文在 OpenReview 上的评分是 8.0，说明评审认为实验在 NeurIPS 层面具有说服力。但这里只能基于摘要概括，不能肯定具体所有实验的公平性。

## 6. 论文的主要结论与发现

- CTM 成功将**神经时序处理和同步成分**引入神经网络之后，网络能够在保持抽象建模水平的同时捕捉更丰富的动态信息，并执行需要复杂时序推理的任务，说明传统静态神经元范式不是唯一途径。
- 模型内部显示出丰富的表征结构，并且由于每种思维状态与神经“同步模式”对应，模型内部过程具有天然可解释性（比传统黑盒模型更适合用于分析）。
- CTM 具有**自适应计算能力**：可以在简单样本上提前终止，困难样本上继续计算，从而节约计算资源并提高推理灵活性。
- 作者的目标是“分享”而非“屠榜”：这一研究为构建生物合理且强大的 AI 提供了一个可行的重要方向。
- 总的来说，实验表明“关注神经元时序与同步”是未来类脑计算和动态深度学习值得拥抱的设计维度。

## 7. 优点与亮点

- **概念新颖、动机深远**：将神经动力学从“辅助正则/脉冲网络小圈子”重新拉回通用深度学习主渠道，提出宏观而可训练的实现方式。
- **统一处理时序与同步**：单个神经元的带权历史处理 + 网络级同步表征结合，使时序信息从底层就参与表示形成过程，而非仅作为输入特征或位置编码。
- **利于可解释性**：同步模式天然是一种结构化的、可用于可视化和分析的内部表示机制，比传统连续值激活更容易语义化理解。
- **自适应的计算控制**：模型可根据任务复杂度决定“思考步数”，具有类人的投入-产出权衡能力。
- **任务设置多样**：从像素分类到迷宫搜索和逻辑顺序问题，显示模型超出了一般时序模型的适用面。

## 8. 不足与局限

- **可获取信息有限**：这里仅能基于摘要总结，技术细节和实验细节——如公式、结构图、训练稳定性、锚点对齐方式、同步表示的编码方式——都未在所给文本中出现。
- **规模与效率未知**：CTM 涉及每个神经元的逐历史权重，参数量和计算复杂度可能显著高于普通前馈网络。论文对训练效率、Memory 消耗及规模化友好性未在摘要中给出说明。
- **没有提 SOTA 对比**：作者明确不做 SOTA 追求，这导致对 CTM 的“实际收益”缺少直接量化结论，需要接受其“演示性”定位。
- **缺乏消融实验证据**：不能判断“时间处理”和“同步表征”各自对性能的独立贡献，也无法判断某一项是否可去除。
- **适用场景存在局限**：同步和时间处理只对需要时间精细信息的任务有价值；对于静态任务（如普通图像分类），其可能并不比标准 ResNet/ViT 有优势，甚至效率更低。
- **公平性/基准风险**：ImageNet-1K和迷宫、奇偶校验的任务难度差别大，如果没有尺度匹配和训练策略公平对比，易受“用复杂模型可以做更复杂任务”的一般性质影响。
- **硬件开销与部署**：没有提供精简、量化和分布式训练策略，未来若要落地仍需更多工程探索。

（完）
