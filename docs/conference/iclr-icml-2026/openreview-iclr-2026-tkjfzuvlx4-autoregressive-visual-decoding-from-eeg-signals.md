---
title: Autoregressive Visual Decoding from EEG Signals
title_zh: 基于EEG信号的自回归视觉解码
authors: "Sicheng Dai, Hongwang Xiao, Shan Yu, Qiwei Ye"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=TKjfzuVLX4"
tags: ["query:eeg-align"]
score: 9.0
evidence: 通过对比微调预训练EEG模型并进行自回归视觉解码，弥合EEG与图像表征之间的模态鸿沟
tldr: 面向EEG的视觉解码目前存在模态鸿沟大、多阶段级联误差累积和扩散模型计算开销高等问题。本工作提出轻量高效的AVDE框架，以预训练EEG模型LaBraM为基础，用对比学习微调并利用自回归方式解码视觉内容。相比复杂的多阶段适配，AVDE降低了计算成本并提升了生成一致性。这为基于EEG的视觉重建与BCI应用提供了可行途径。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 基于EEG的视觉解码面临模态差距和多阶段误差累积，且大规模扩散模型带来高开销。
method: 提出AVDE，利用LaBraM预训练EEG模型做对比微调，并以轻量自回归解码器生成视觉输出。
result: 在缓解EEG与图像模态错位的同时降低计算负担，使视觉解码更实用且一致性更强。
conclusion: 展示了预训练EEG模型结合自回归生成可兼顾效率与效果，为BCI视觉重建提供了新方向。
---

## Abstract
Electroencephalogram (EEG) signals have become a popular medium for decoding visual information due to their cost-effectiveness and high temporal resolution. However, current approaches face significant challenges in bridging the modality gap between EEG and image data. These methods typically rely on complex adaptation processes involving multiple stages, making it hard to maintain consistency and manage compounding errors. Furthermore, the computational overhead imposed by large-scale diffusion models limit their practicality in real-world brain-computer interface (BCI) applications. In this work, we present AVDE, a lightweight and efficient framework for visual decoding from EEG signals. First, we leverage LaBraM, a pre-trained EEG model, and fine-tune it via contrastive learning to align EEG and image representations. Second, we adopt an autoregressive generative framework based on a "next-scale prediction" strategy: images are encoded into multi-scale token maps using a pre-trained VQ-VAE, and a transformer is trained to autoregressively predict finer-scale tokens starting from EEG embeddings as the coarsest representation. This design enables coherent generation while preserving a direct connection between the input EEG signals and the reconstructed images. Experiments on two datasets show that AVDE outperforms previous state-of-the-art methods in both image retrieval and reconstruction tasks, while using only 10% of the parameters. In addition, visualization of intermediate outputs shows that the generative process of AVDE reflects the hierarchical nature of human visual perception. These results highlight the potential of autoregressive models as efficient and interpretable tools for practical BCI applications.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **背景**：脑电图（EEG）因其成本低、时间分辨率高，逐渐成为视觉信息解码的常用神经信号模态。
- **核心问题**：现有从 EEG 重建或解码视觉内容的方法面临三大挑战：
  - EEG 与图像之间的**模态鸿沟**较大，跨模态对齐困难；
  - 传统方法依赖**多阶段适配流程**，中间误差易累积，导致重建一致性差；
  - 大规模扩散模型虽然生成质量高，但**计算开销大**，难以满足实际脑机接口（BCI）应用对效率的要求。
- **整体含义**：该论文旨在探索一种轻量、高效且具备可解释性的 EEG 视觉解码方式，为实际 BCI 应用提供更可行的技术路径。

### 2. 论文提出的方法论

- **核心思想**：提出 AVDE（Autoregressive Visual Decoding from EEG）框架，将 EEG 信号与图像的重建关系转化为“由粗到细”的自回归生成过程。
- **关键步骤**：
  1. **预训练 EEG 模型微调**：以预训练 EEG 模型 LaBraM 为基础，使用**对比学习**对 EEG 与图像表示进行对齐，缓解模态鸿沟。
  2. **多尺度图像 token 化**：利用预训练 VQ-VAE 将图像编码为**多尺度 token 图**。
  3. **自回归“下一尺度预测”**：训练一个 Transformer 模型，以 EEG 嵌入作为“最粗表示”，逐步自回归地预测更细尺度的图像 token。
- **设计优势**：
  - 自回归生成过程不依赖大规模扩散模型，计算复杂度更低；
  - 保留输入 EEG 到输出图像之间的直接连接，从而增强生成一致性和可解释性。

### 3. 实验设计

- **数据集 / 场景**：论文在两个 EEG 视觉数据集上进行了实验验证。
- **Benchmark 与任务**：实验覆盖两大任务——
  - 图像检索任务；
  - 图像重建任务。
- **对比方法**：与现有的 state-of-the-art（SOTA）方法进行对比，重点是验证 AVDE 在生成质量和检索性能上的优越性。

### 4. 资源与算力

- 从论文摘要中无法获取具体的算力信息，例如 GPU 型号、数量、训练时长、显存占用等均未明确说明。
- 唯一可确认的效率指标是：AVDE 仅使用了对比 SOTA 方法约 **10% 的参数规模**，说明其参数量显著更低，但具体硬件资源消耗仍需原论文补充。

### 5. 实验数量与充分性

- **实验数量**：涉及两个数据集、两个主任务（检索 + 重建）的对比实验，并包含对中间生成过程的可视化。
- **实验充分性评估**：
  - 仅凭摘要无法判断是否进行了系统性的消融实验、统计分析、跨受试者泛化测试或不同 EEG 协议下的验证；
  - 与 SOTA 对比并实现更低参数量下更好性能，具备一定说服力，但缺少详细误差分析和实验设置说明；
  - 总体而言，实验覆盖面较窄，结论的稳健性和公平性有待阅读全文后才能完整评估。

### 6. 论文的主要结论与发现

- AVDE 在图像检索和图像重建任务上**均优于先前 SOTA 方法**，同时参数量仅为其约 10%。
- 可视化中间输出显示：AVDE 的生成过程呈现出与**人类视觉感知的层次性**相符的特点，即从粗糙结构逐步精细化。
- 以上结果表明，**自回归模型可以成为高效且可解释的 EEG 视觉解码工具**，为 BCI 应用提供新的方向。

### 7. 优点

- **轻量高效**：避免使用大规模扩散模型，参数量大幅缩减，更符合 BCI 对实时性和算力受限环境的需求。
- **缓解误差累积**：不同于多阶段级联方法，自回归“下一尺度预测”将生成过程统一在单一框架中，降低了阶段间不一致。
- **模态对齐直接有效**：借助预训练 EEG 模型 LaBraM + 对比学习，更好地弥合 EEG 与图像之间的表征鸿沟。
- **可解释性较强**：生成过程的尺度推进结构可类比视觉感知的层级加工，便于理解模型内部行为。
- **预训练模型的有效利用**：验证了通用 EEG 预训练模型可以迁移至视觉解码任务并取得优秀效果。

### 8. 不足与局限

- **信息可用性有限**：当前文本仅为摘要，缺少模型结构细节、训练超参、数据规模、预处理流程等关键信息，难以复现。
- **实验覆盖有限**：只有两个数据集的检索与重建任务，缺乏跨数据集泛化、跨被试迁移、噪声鲁棒性等更全面评测；也没有看到明确的消融实验。
- **对比公平性存疑**：虽声称优于 SOTA，但未说明是否在相同的训练设置、数据划分和评估指标下进行，评审需谨慎看待。
- **应用限制**：EEG 信号本身信噪比低且个体差异大，实际 BCI 场景中的实用性能仍需在线实验验证。
- **算力信息缺失**：论文未报告 GPU 等资源使用情况，难以从算力维度评估其“轻量高效”的完整含义。

（完）
