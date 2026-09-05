---
title: "FACE: Faithful Automatic Concept Extraction"
title_zh: FACE：忠实的自动概念提取
authors: "Dipkamal Bhusal, Michael Clifford, Sara Rampazzi, Nidhi Rastogi"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ssocZIfk94"
tags: ["query:abstraction"]
score: 5.0
evidence: 用NMF与分类器监督自动提取概念，提供可信的概念发现方法，可服务于概念表构建。
tldr: 针对自动概念发现常与模型实际决策脱节的问题，FACE提出把非负矩阵分解与KL散度正则结合，并在学习概念时加入分类器监督，使基于概念的解释重建出与原模型一致的预测。与只使用编码器激活的方法不同，FACE明确以分类预测一致性作为约束，因而生成的概念更能反映模型真实决策。该方法为深度学习模型提供了一套更可信的自动概念提取框架，也能为概念列表构建积累候选。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 已有概念自动发现方法没有对齐模型真实决策过程，解释不够忠实可信。
method: 结合非负矩阵分解和KL散度正则，引入分类器监督以对齐原模型与概念预测。
result: 相比仅基于编码器激活的旧方法，FACE提取的概念与模型预测一致性更强。
conclusion: 为可解释AI提供可靠的概念提取方法，可用于支撑概念库的自动化建设。
---

## Abstract
Interpreting deep neural networks through concept-based explanations offers a bridge between low-level features and high-level human-understandable semantics. However, existing automatic concept discovery methods often fail to align these extracted concepts with the model’s true decision-making process, thereby compromising explanation faithfulness. In this work, we propose FACE (Faithful Automatic Concept Extraction), a novel framework that combines Non-negative Matrix Factorization (NMF) with a Kullback-Leibler (KL) divergence regularization term to ensure alignment between the model’s original and concept-based predictions. Unlike prior methods that operate solely on encoder activations, FACE incorporates classifier supervision during concept learning, enforcing predictive consistency and enabling faithful explanations. We provide theoretical guarantees showing that minimizing the KL divergence bounds the deviation in predictive distributions, thereby promoting faithful local linearity in the learned concept space. Systematic evaluations on ImageNet, COCO, and CelebA datasets demonstrate that FACE outperforms existing methods across faithfulness and sparsity metrics.

---

## 论文详细总结（自动生成）

### 论文总结：FACE——忠实的自动概念提取

#### 1. 核心问题与整体含义
- **研究动机**：深度学习模型的可解释性是可解释人工智能的关键挑战之一。基于概念的解释方法试图在低层特征与人类可理解的高层语义之间架设桥梁，以提高模型决策的透明度和可信度。
- **现有缺陷**：现有自动概念发现方法大多只优化编码器（encoder）层面的表征，如利用激活值进行概念聚类或分解，却忽略了提取出的概念是否真正与模型最终分类决策相一致。因此，这类解释可能**与模型的真实决策过程脱节**，导致解释缺乏“忠实性”（faithfulness），甚至产生误导性结论。
- **核心研究问题**：如何自动提取既能被人类理解、又能忠实反映模型内部真实决策依据的概念。

#### 2. 方法论
- **总体目标**：提出一个名为 **FACE（Faithful Automatic Concept Extraction）** 的新框架，保证依据提取的概念重建出的预测结果能够与原模型的预测分布保持一致。
- **核心技术思想**：将**非负矩阵分解（NMF）**与**KL 散度正则化项**结合，用于学习概念表示。
  - **非负矩阵分解（NMF）**：解码深层网络的特征激活，得到一组基向量（即候选概念）及相应的非负系数。NMF 因其天然的“部分-整体”语义可解释性，被广泛用于概念发现。
  - **KL 散度正则项**：在学习概念表示时，同时引入一个分类器监督信号，促使 **原模型的预测分布** 与 **基于提取概念重建的预测分布** 之间尽可能地一致（即减小 KL 散度）。
- **与既有方法的本质区别**：以往工作（如基于编码器激活的 NMF 或聚类）只关注表示层面的结构，FACE 则将**预测一致性（predictive consistency）**作为显式学习约束引入概念学习过程，使概念空间表现出**忠诚的局部线性（faithful local linearity）**特性。
- **理论保证**：论文提供理论分析，证明最小化 KL 散度可以将原模型与概念重建模型之间的预测分布差异控制在一个可证明的边界内——这为解释在数学意义上具备忠实性提供了支撑。

#### 3. 实验设计
- **数据集**：实验在三个广泛使用的基准数据集上进行：
  - **ImageNet**（大规模自然图像分类）
  - **COCO**（目标检测与上下文分割）
  - **CelebA**（大规模人脸属性分类）
- **基准与对比方法**：与现有自动概念提取方法（如仅使用编码器激活的分解方法）比较，评测指标重点对齐 **忠实性（faithfulness）** 与 **稀疏性（sparsity）** 两方面。
- **实验目标和口径**：验证 FACE 是否在概念解释质量的核心维度上超越传统方法，且理论上保证的预测一致性是否在实证中得到验证。

#### 4. 资源与算力
- **文中说明情况**：论文提供的摘要与公开信息中**未明确报告**所使用的 GPU 型号、数量或具体训练时长。
- **评价**：缺少算力配置说明不利于读者评估方法的实际计算负担和可复现性，这是本文在技术细节透明度上值得完善的地方。

#### 5. 实验数量与充分性
- **覆盖面**：在**三个不同领域的数据集**上进行实验，涵盖类别分类（ImageNet）、多目标场景（COCO）、细粒度语义（人脸属性）等不同任务复杂度，具有较强的代表性。
- **对比完整性**：论文展示了 FACE 在关键指标（忠实性和稀疏性）上的优势，说明方法相对于旧范式具有显著增量。
- **充分性与客观性**：从摘要所提供的信息看，缺少关于**具体基线方法类型、消融实验设置、统计显著性测试（如多次随机种子实验结果以及方差）** 的详细说明。因此，暂不能完全判断实验是否做到全方位公平，本论文的对比实验需要增强的方面在于补充更多从不同维度设计的对照组。

#### 6. 主要结论与发现
- FACE 提取的概念解释在**忠实性**与**稀疏性**指标上优于已有方法。
- 加入分类器监督作为学习概念的结构性约束，能够显著拉近概念预测与原模型的预测分布，使得解释更好地反映模型决策的真实逻辑。
- 理论与实证共同提示：**概念解释的质量应首先对齐模型结果，而非仅关注特征空间内的某种形式的低秩一致性**。

#### 7. 优点
- **架构设计新颖**：将分解与监督预测一致性约束关联起来，解决了解释“语法上可信但语义不匹配”的痛点。
- **理论支撑完善**：给出了 KL 散度最小化的概率分布偏差界，使方法优越性不仅停留在经验层面，也有较强的理论基础。
- **对不同视觉任务的表现证明了该框架一定的泛化能力**。
- 工作可操作性强，不需要复杂额外的模型设定，只需在原有编码器-分类器结构上加入分解层和监督损失。

#### 8. 不足与局限
- **实验数据量有待加深**：摘要中仅提及三个数据集的结果，未深入展示诸如类别梯度差异性分析、不同模型结构下的可迁移性验证（如 transformer vs CNN、域迁移/跨任务场景）。
- **稳定性信息缺失**：没有关于多次训练重复实验的标准差或置信区间信息，尚难判定该优势是否具有统计学上的广泛稳定性。
- **可扩展性与应用限制**：概念提取需要将激活层投影到可学习空间，大规模数据（如 10 亿级参数模型，流式数据场景）时的扩展性没有被讨论。
- **系统验证目前以视觉任务为主**，对于时序数据和自然语言任务缺少横跨性论证。
- 对解释效果更广泛的评估除了忠实性与稀疏性外，还缺乏用户侧或下游行为学的实际验证。

---

（完）
