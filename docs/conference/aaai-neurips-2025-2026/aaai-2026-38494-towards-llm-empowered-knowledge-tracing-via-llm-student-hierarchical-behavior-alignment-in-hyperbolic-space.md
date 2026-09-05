---
title: Towards LLM-Empowered Knowledge Tracing via LLM-Student Hierarchical Behavior Alignment in Hyperbolic Space
title_zh: 基于双曲空间的大模型-学生层级行为对齐实现大模型赋能知识追踪
authors: "Xingcheng Fu, Shengpeng Wang, Yisen Gao, Xianxian Li, Chunpei Li, Qingyun Sun, Dongran Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38494/42456"
tags: ["query:abstraction"]
score: 4.0
evidence: 在双曲空间中构造知识点层级依赖并用于大模型知识追踪，对概念层级构建有方法参考，但不是脑启发抽象工作。
tldr: 知识追踪需要刻画学生概念掌握的变化，但现有方法缺少语义建模和知识点层级依赖的显式表达。L-HAKT使用教师大模型解析题目语义并构造知识点间的层级依赖，同时用学生智能体在双曲空间中模拟学习者行为，使两种行为实现层级化对齐，以便预测掌握状态。该方法把双曲空间的层级表达能力用于建模认知状态演化。尽管其目标是教育诊断而非构建脑类具象-抽象概念空间，但其层级语义建模方法可为大规模概念类目生成提供参考。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有知识追踪方法难以捕捉知识点层级依赖与个体化难度感知，语义建模不足。
method: 采用大模型解析试题语义，在双曲空间中对齐教师与学生层级行为，并显式构建知识点层级。
result: 该方法增强了知识追踪中的语义建模与层级认知状态刻画能力，有助于更准确地诊断概念掌握。
conclusion: 展示双曲空间可用于建模概念层级与认知演化，可作为概念层级学习参考，但并非类脑概念空间方法。
---

## Abstract
Knowledge Tracing (KT) diagnoses students’ concept mas- tery through continuous learning state monitoring in education. Existing methods primarily focus on studying behavioral sequences based on ID or textual information. While existing methods rely on ID-based sequences or shallow textual features, they often fail to capture (1) the hierarchical evolution of cognitive states and (2) individualized prob- lem difficulty perception due to limited semantic modeling. Therefore, this paper proposes a Large Language Model Hyperbolic Aligned Knowledge Tracing(L-HAKT). First, the teacher agent deeply parses question semantics and explicitly constructs hierarchical dependencies of knowledge points; the student agent simulates learning behaviors to generate synthetic data. Then, contrastive learning is performed between synthetic and real data in hyperbolic space to reduce distribution differences in key features such as question difficulty and forgetting patterns. Finally, by optimizing hyperbolic curvature, we explicitly model the tree-like hierarchical structure of knowledge points, precisely characterizing differences in learning curve morphology for knowledge points at different levels. Extensive experiments on four real-world educational datasets validate the effectiveness of our Large Language Model Hyperbolic Aligned Knowledge Tracing (L-HAKT) framework.

---

## 论文详细总结（自动生成）

# 《Towards LLM-Empowered Knowledge Tracing via LLM-Student Hierarchical Behavior Alignment in Hyperbolic Space》论文总结

## 1. 核心问题与整体含义（研究动机与背景）

知识追踪（Knowledge Tracing, KT）的目标是根据学生历史学习行为，持续诊断其对知识概念的掌握程度，进而支持个性化教学。现有 KT 方法主要分为两类：

- **序列建模方法**：如 DKT、DKVMN、SAKT 等，利用 RNN、记忆网络或 Transformer 建模交互序列的时间动态；
- **图建模方法**：如 GKT、GIKT 等，通过显式构造知识点之间的图结构来传播知识状态。

然而，论文指出这些方法存在三个关键问题：

1. **知识点的层级结构没有被显式建模**：知识系统天然具有“从基础概念到高阶推理”的树状或层级结构，但传统模型常在欧氏空间中学习，无法有效表达这种层级关系；
2. **题目语义信息利用不足**：很多方法仅依赖 ID 或浅层文本特征，没有从题目文本中有效抽取知识点之间的语义关联和依赖关系；
3. **难度感知存在个体偏差**：学生能力水平会造成“难题被低估/高估”的情况，传统方法难以区分题目客观难度与学生个体认知差异。

为此，论文提出 **L-HAKT（Large Language Model Hyperbolic Aligned Knowledge Tracing）** 框架，核心思想是：

- 利用 **教师智能体（Teacher Agent）** 解析题目语义，显式构造知识点的层级依赖图；
- 利用 **学生智能体（Student Agent）** 模拟个性化学习行为，生成合成交互数据；
- 在**双曲空间**中对齐真实数据与合成数据，并通过曲率优化显式建模知识点的树状层级结构，最终更精确地刻画学生认知状态的层级演化。

---

## 2. 论文提出的方法论

### 2.1 核心思想

L-HAKT 将“教师—学生”双智能体数据生成范式与“双曲几何”表示学习相结合，形成“层级知识点结构 + 层级认知行为 + 层级状态追踪”三条主线：

1. 构造显式知识点层级图谱；
2. 模拟并校准学生层级化学习行为；
3. 利用双曲空间的层次传播特性追踪高阶知识点的掌握情况。

### 2.2 教师智能体（Teacher Agent）

教师智能体处理题目（可选择图像输入 + VLM 转文本，例如 Qwen-2.5VL 用于 Eedi 数据集）并输出三类结果：

- **层级知识点识别**：将题目解析为语义文本后，抽取知识点并划分为 4 个层级 `L ∈ {1,2,3,4}`，从基本定义到综合推理；
- **结构化解构知识图谱**：依据知识点层级建立父子依赖边，形成树状教学结构；
- **题目难度量化**：根据题目相关知识点的组合层级计算客观难度分数。

### 2.3 学生智能体（Student Agent）

学生智能体用于模拟真实学生的做题过程。给定历史交互序列，它包含两个模块：

- **认知参与模块（Cognitive Engagement Module）**：根据当前题目难度和学生对相关知识的掌握程度，估计其学习专注程度 `Γ_j`；
- **层级遗忘模块（Hierarchical Forgetting Module）**：根据知识点层级差异设置不同的记忆衰减斜率，难度越高的知识遗忘越快，用 `F_j = exp(-λ · L_avg · t_j)` 等方式刻画遗忘。

学生智能体用 LSTM 更新知识状态：

`h_s^j = LSTM([X_qj; sum_c w_c X_c] ⊕ (Γ_j ⊙ F_j ⊙ h_s^{j-1}))`

最终预测学生答题结果 `A_j`，并输出推理路径 `P_j = [c^(1), ..., c^(k) → A_j]`。

### 2.4 双曲空间中的编码与对齐

- 构造异构图：题目—知识点边、知识点—知识点层级依赖边；
- 使用**关系感知双曲图神经网络（Relation-aware Hyperbolic GNN）**：
  - 对真实图和合成图分别设置曲率 `κ_real` 与 `κ_syn`；
  - 通过指数映射将对偶嵌入映射到双曲流形，在流形上执行多头注意力聚合；
  - 双曲空间支持基础知识点位于较平坦中心区、高阶知识点位于高曲率外围区的层次化分布。
- 设计**双曲对比对齐（Hyperbolic Contrastive Alignment）**：对真实数据和合成数据中的共享实体组成正样本对，通过对比损失拉近它们在双曲空间中的语义表示，从而修正合成数据与真实数据在“难度、遗忘、掌握程度”等特征上的分布偏移。

### 2.5 双曲知识状态追踪器

- 将双曲嵌入通过对数映射映射回切空间，与答题正确率 `R_t`、题目难度、知识点层级平均值 `c_L` 融合；
- 使用基于切空间的 GRU（HGRU）处理序列；再将隐状态映射回双曲空间；
- 预测当前题目正确概率；
- 总损失为 `L_total = L_KT + α L_con`，其中 `L_KT` 为二值交叉熵损失，`L_con` 为对比对齐损失。

---

## 3. 实验设计

### 3.1 数据集

论文使用四个公开教育数据集：

| 数据集 | 学生数 | 题目数 | 知识点数 | 交互数 |
| --- | --- | --- | --- | --- |
| ASSIST2009 | 4160 | 15643 | 167 | 206631 |
| ASSIST2012 | 5000 | 36054 | 242 | 713123 |
| EdNet | 5000 | 11700 | 1830 | 1147423 |
| Eedi | 5000 | 26702 | 1050 | 586234 |

其中 Eedi 含题目图像信息，论文用视觉大模型辅助解析题目内容，并额外标注了“模拟交互数据”的增强规模。

### 3.2 Benchmark 与对比方法

L-HAKT 与两类主流基线比较：

- 序列建模方法：DKT、DKT+、DKT+forgetting、DIMKT、DKVMN、Deep-IRT、ATKT、AT-DKT、AKT、SAKT、SAINT、CL4KT、simpleKT、MIKT 等；
- 图建模方法：GKT、GIKT 等。

### 3.3 关键实验内容

1. **主实验**：在四个数据集上比较 AUC 与 ACC；
2. **消融实验**：基于 GKT 作为骨干，比较完整 L-HAKT、去掉对比对齐（w/o con）、去掉双曲表示（w/o hyp）在四个数据集上的效果；
3. **知识图谱有效性验证**：对比“只用原始数据”“加入合成数据”“加入教师知识图谱与双曲对齐”三种配置，并可视化教师智能体生成的知识图谱片段。

---

## 4. 资源与算力

- 论文明确说明：所有模型均在**单个 Nvidia A100 40GB GPU** 上完成训练与测试；
- 没有报告 GPU 数量、训练时间、显存占用或推理成本等细节；
- 也没有详细分析 LLM Agent（教师/学生智能体）调用所需的额外算力开销。

因此，关于算力消耗的信息十分有限，可复现所需的完整资源细节并未给出。

---

## 5. 实验数量与充分性

### 实验数量

- 在四个真实数据集上进行主实验，对比约 17 种以上基线方法；
- 消融实验覆盖“无对比学习”“无双曲空间”两种变体，在四个数据集上分别报告 AUC/ACC；
- 还包括知识图谱有效性的补充验证与可视化分析，整体实验规模较大。

### 充分性评价

**优点**：

- 数据集数量较多，覆盖不同规模、不同领域（数学/通用教育等）的 KT 数据；
- 基线方法覆盖面广，既有序列模型也有图模型，且包含近年较新的 MIKT、simpleKT 等；
- 消融设计能初步分离“双曲结构建模”和“对比对齐”两个核心机制的作用。

**不足 / 风险**：

- 数据划分采用随机 80%/20% 单次划分，没有使用交叉验证，结果稳定性有限；
- 评测指标仅采用 AUC 和 ACC，缺少 F1、RMSE、预测校准度等其他指标；
- 没有报告对超参数 α、温度系数 τ、层数等的敏感性分析；
- 没有进行统计显著性检验（如 t-test/Wilcoxon），无法判断性能提升是否在多次运行下显著；
- 消融仅验证“去掉对比”和“去掉双曲”两个维度，没有单独去掉 Teacher Agent 或 Student Agent 做精细化归因。

总体上，实验丰富度较高，但严谨性和稳定性证据稍显不足。

---

## 6. 主要结论与发现

1. **L-HAKT 在四个数据集上的 AUC/ACC 均显著优于已有 KT 基线**，证明双曲空间和 LLM 生成数据有助于提升知识追踪效果。
2. **双曲表示学习能更好地区分基础知识点与高阶知识点的学习曲线差异**，曲率自适应机制有助于捕捉高阶知识点的“突变式掌握”。
3. **对比对齐机制能修正 LLM 合成数据的分布偏差**，使合成数据不仅带来数据增强，还能与真实学生行为在语义空间上保持一致性。
4. **教师智能体生成的知识图谱呈现符合教学直觉的层级依赖关系**，可用于后续下游任务。

---

## 7. 优点

- **方法创新性强**：首个将“LLM 教师—学生双智能体”协同框架引入 KT 的尝试，弥补了传统数据缺失“学生思维路径”的盲区；
- **几何空间选择合理**：双曲空间天然适合树状/层级结构，能有效建模知识点等级关系；
- **数据增强方式较为新颖**：不是简单生成虚假样本，而是通过“学生智能体模拟认知过程”生成具有行为解释的数据，再配合对比对齐；
- **实验覆盖面广**：主实验 + 消融实验 + 可视化，能完整展示方法在不同维度的贡献；
- **对多模态数据有适配**：通过 VLM 解析题目图像，适应如 Eedi 的真实教育场景。

---

## 8. 不足与局限

- **依赖 LLM 输出质量**：教师/学生智能体的知识点抽取、层级判断、难度评估均由 LLM 完成，存在幻觉或偏差风险；论文未报告人工评估或与专家标注的一致性验证。
- **合成数据的真实感有限**：学生智能体只是模拟模型，无法真正还原真实学生复杂情感、动机、策略等因素，对最终预测增益的上限有限。
- **计算开销未被评估**：大模型调用和双曲空间运算的实际成本、训练时间等缺少量化，实际部署可行性存疑。
- **实验设计仍有提升空间**：缺少交叉验证、显著性检验、超参数敏感性分析和更多评测指标；对不同数据集（如是否存在图像输入、不同知识点结构）的差异化表现没有深入讨论。
- **应用范围有限**：论文仅报告在教育知识追踪数据集上的离线预测性能，没有展示真实课堂或在线学习系统中的个性化干预效果。

---

（完）
