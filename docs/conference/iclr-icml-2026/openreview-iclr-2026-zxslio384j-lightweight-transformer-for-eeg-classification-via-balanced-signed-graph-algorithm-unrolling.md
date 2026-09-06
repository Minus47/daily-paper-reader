---
title: Lightweight Transformer for EEG Classification via Balanced Signed Graph Algorithm Unrolling
title_zh: 基于平衡符号图算法展开的轻量级Transformer用于EEG分类
authors: "Junyi Yao, Parham Eftekhar, Gene Cheung, Xujin Chris Liu, Yao Wang, Wei Hu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=zxsLio384j"
tags: ["query:eeg-align"]
score: 6.0
evidence: 把平衡符号图上的谱去噪先验展开成轻量Transformer，用于癫痫EEG分类
tldr: EEG传感器样本存在固有反相关，可用负边图建模。该工作把平衡符号图上基于谱去噪的算法展开为轻量、可解释的Transformer网络，并在等效正图上用Lanczos近似高效实现理想低通滤波。利用可学习的最优截止频率将图先验与分类任务端到端联合优化，在癫痫与非癫痫EEG识别中兼顾精度与可解释性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: EEG信号天然带有电极间反相关关系，现有深度模型缺少对这种图结构的显式利用。
method: 将平衡符号图上的谱去噪算法展开为Transformer，并用Lanczos近似低通滤波实现可学习截止频率。
result: 在癫痫患者与健康对照的EEG识别任务上获得高效且可解释的分类结果。
conclusion: 把图信号先验注入网络结构可提升EEG分类的效能和可解释性。
---

## Abstract
Samples of brain signals collected by EEG sensors have inherent anti-correlations that are well modeled by negative edges in a finite graph. 
To differentiate epilepsy patients from healthy subjects using collected EEG signals, we build lightweight and interpretable  transformer-like neural nets by unrolling a spectral denoising algorithm for signals on a balanced signed graph---graph with no cycles of odd number of negative edges.
A balanced signed graph has well-defined frequencies that map to a corresponding positive graph via similarity transform of the graph Laplacian matrices.
We implement an ideal low-pass filter efficiently on the mapped positive graph via Lanczos approximation, where the optimal cutoff frequency is learned from data.
Given that two balanced signed graph denoisers learn posterior probabilities of two different signal classes during training, we evaluate their reconstruction errors for binary classification of EEG signals.
Experiments show that our method achieves classification performance comparable to representative deep learning schemes, while employing dramatically fewer parameters.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：脑电图（EEG）信号由多个传感器采集，不同电极之间的样本存在固有的**反相关（anti-correlation）**关系。这种关系可以用带有负边的有限图来建模。然而，现有的深度学习方法往往没有显式利用这种图结构先验。
- **核心问题**：如何利用 EEG 传感器之间的反相关图结构，区分**癫痫患者**与**健康对照**，同时保证模型**轻量**且**可解释**。
- **整体含义**：作者尝试将经典的图信号处理先验（平衡符号图上的谱去噪）以“算法展开”的方式嵌入网络结构，从而替代大量黑箱参数，提升 EEG 分类任务的效率和可解释性。

### 2. 论文提出的方法论

- **核心思想**：把**平衡符号图**（balanced signed graph，即不含奇数条负边环路的图）上的**谱去噪算法**展开为一种类似 Transformer 的轻量神经网络。平衡符号图具有良好的频率定义，可通过图拉普拉斯矩阵的相似变换映射到对应的**正图**（positive graph），从而简化处理。
- **关键技术细节**：
  - **低通滤波实现**：在映射后的正图上，使用 **Lanczos 近似**高效实现理想低通滤波，替代直接的特征分解。
  - **可学习截止频率**：最优截止频率不是手工设定的，而是通过数据端到端学习得到。
- **分类策略**：训练阶段分别构建两个平衡符号图去噪器，用来学习两类信号（如癫痫/健康）的后验概率；推理阶段则比较两个去噪器的**重构误差**，根据误差大小做出二分类判断。
- **算法流程（文字说明）**：
  1. 根据 EEG 电极通道间的反相关关系构建平衡符号图；
  2. 对图拉普拉斯矩阵执行相似变换，得到对应的正图；
  3. 在正图上通过 Lanczos 近似实现低通滤波，并令截止频率可学习；
  4. 将上述去噪迭代过程“展开”为类 Transformer 的网络层；
  5. 端到端训练两个类别对应的去噪器；
  6. 部署时，比较两个去噪器对测试 EEG 信号的重构误差，输出分类结果。

### 3. 实验设计

- **任务场景**：利用采集到的 EEG 信号区分癫痫患者和健康受试者。
- **数据集**：摘要及元数据中**没有明确给出数据集名称、规模或采集设备**。
- **对比方法**：摘要仅提到与“有代表性的深度学习方案”（representative deep learning schemes）进行了比较，但未列出具体对比模型名称。
- **评价指标**：未在摘要中说明使用了哪些指标（如准确率、F1、AUC 等）。

### 4. 资源与算力

- 论文提供的摘要和元数据中，**没有提及所使用的 GPU 型号、数量、训练时长或任何算力资源信息**。

### 5. 实验数量与充分性

- **实验数量**：现有信息不足以判断具体实验组数。摘要中没有描述多数据集验证、消融实验或误差分析等细节。
- **充分性评估**：由于缺少数据集细节、指标细节、对比方法明细和统计显著性检验，无法从当前文本中确认实验是否充分、客观、公平。

### 6. 论文的主要结论与发现

- 所提出的基于平衡符号图算法展开的轻量 Transformer 方法，在癫痫与非癫痫 EEG 信号的二分类上，能够取得与有代表性的深度学习方法**相当的分类性能**。
- 同时，该方法使用的**参数量显著更少**，且由于结构上内嵌了图谱去噪先验，具备一定的**可解释性**。

### 7. 优点

- **显式利用图结构先验**：将 EEG 通道间的反相关关系建模为符号图负边，并用信号处理理论指导网络设计。
- **轻量高效**：用 Lanczos 近似避免显式特征分解，显著降低计算/存储开销，整体参数量远小于典型深度模型。
- **可解释性强**：网络层对应谱去噪算法的迭代步骤，且滤波截止频率可学习，建立了图信号处理与深度学习之间的解释性桥梁。
- **端到端优化**：图先验与分类目标通过可学习参数联合优化，避免了两阶段处理的次优性。

### 8. 不足与局限

- **实验信息不足**：摘要未提供数据集规模、具体对比基线、评价指标及实验次数等细节，难以全面评估其泛化性和优势幅度。
- **任务单一**：目前只报告了癫痫/健康二分类，未展示其他 EEG 任务或多分类结果，应用广度有限。
- **先验假设依赖**：方法依赖于 EEG 信号能够被平衡符号图良好建模的假设，在通道间反相关并非平衡结构或存在噪声边时，效果可能受影响。
- **近似误差可能性**：Lanczos 近似和理想低通滤波的离散化可能引入误差，文中未讨论该近似带来的性能损失。
- **缺少资源刻画**：未说明训练所需算力，阻碍了与其他方法在“公平效率对比”维度的评估。
- **架构名称带有“Transformer”**：但网络实际是算法展开的结果，与常见的基于自注意力的 Transformer 关系需要进一步澄清和对比验证。

（完）
