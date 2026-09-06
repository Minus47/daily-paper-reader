---
title: "When the Brain Sees Beyond Pixels: Creative Brain-to-Vision Reconstruction"
title_zh: 当大脑超越像素：创造性的脑到视觉重建
authors: "Xi Ding, Lei Wang, Piotr Koniusz, Yongsheng Gao"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=ad1A3bZpkf"
tags: ["query:eeg-align"]
score: 4.0
evidence: 面向fMRI图像的语义生成，引入CLIP语义先验进行脑与视觉对齐，但并非EEG模态
tldr: 传统基于fMRI的图像重建偏执于像素还原，忽略脑信号中的抽象语义和想象成分。该文提出频率引导的脑到视觉生成框架，用图谱谱变换刻画fMRI，以掩码频率建模处理图像，并利用CLIP文本语义嵌入提供语义先验。系统按粗到细的频率结构对齐神经与视觉域，生成比像素复制更具语义保真度的图像。该方法对脑信号与视觉语义对齐具有参考价值，但实验模态为fMRI而非EEG。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 以像素保真为目标的大脑图像重建忽略了脑信号包含的抽象语义信息，限制了重建价值。
method: 融合fMRI图谱谱变换、图像掩码频率建模和CLIP文本语义先验，进行多尺度粗到细对齐生成。
result: 实验显示生成图像超越像素复制，在语义层面保留更多创造性脑内信息。
conclusion: 频率结构与语义先验的联合对齐有望扩展到其他脑信号视觉解码任务。
---

## Abstract
Reconstructing images from fMRI has traditionally been framed as maximizing pixel fidelity to visual input. While useful for benchmarking, this perspective overlooks what brain signals truly encode: not only perception, but also abstraction, semantics, and imagination. We introduce a frequency-informed framework for brain-to-vision generation that shifts the objective from replication to creative alignment across neural and visual domains. Our method applies graph spectral transforms to fMRI signals and masked frequency modeling to images, enabling coarse-to-fine reconstruction by selectively aligning low-, mid-, and high-frequency structures. To ground generation in meaning, we incorporate semantic priors via CLIP-text embeddings and multi-level visual features, with attention mechanisms that allow frequency-masked brain signals to interact with both reconstructions and textual cues. The model integrates pretrained VDVAE, CLIP, and diffusion backbones, while introducing three novel frequency-aligned projection layers: (i) a low-level hierarchical brain-to-vision layer, (ii) a high-level semantic brain-to-vision layer, and (iii) a brain-to-text alignment layer. The resulting generations may deviate from pixel-level ground truth yet capture emergent structures that show how the brain creatively encodes and reinterprets visual experience. By bridging frequency structures across neural, visual, and semantic modalities, our approach reframes fMRI-to-image reconstruction as a study of how humans perceive, imagine, and create, beyond simple replication.

---

## 论文详细总结（自动生成）

## 论文基本信息

- **标题**：When the Brain Sees Beyond Pixels: Creative Brain-to-Vision Reconstruction
- **作者**：Xi Ding, Lei Wang, Piotr Koniusz, Yongsheng Gao
- **会议/投稿状态**：根据提供的元数据，该文标记为 ICLR-2026-Rejected-Public（即曾在 ICLR 2026 投稿但被拒）
- **模态**：fMRI（非 EEG）

> 说明：以下总结基于所提供的摘要与元数据生成，而非论文完整 PDF 正文，因此部分实验细节、数据集和算力信息无法确证。

---

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：传统的基于 fMRI 的图像重建任务，几乎都把目标定义为“对视觉输入像素的最大化保真”，即让重建图像尽可能接近被试看到的原始图片。
- **核心问题**：这种“像素复制”式的研究视角，忽略了大脑信号中真正重要的信息——大脑不仅感知外界图像，还会对视觉经验进行抽象、语义加工、想象与创造性再解释。
- **整体含义**：本文试图回答一个更本质的问题：大脑如何“超越像素地”编码、存储和重构视觉信息，而不是仅把 fMRI 解码为一张像素级相似的图像。
- **研究意义**：将“脑到视觉”的重建任务从**复制感知**转向**理解创造性认知加工**，从而为人脑视觉机制研究、脑机接口和生成式 AI 提供新方向。

## 2. 论文提出的方法论

- **核心思想**：放弃过度强调像素保真的做法，改为在**神经信号域与视觉图像域**之间进行“频率引导下的创造性对齐”，使生成结果在保留语义意义的前提下，能映射出大脑内部的抽象编码。
- **技术流程大致分为模块**：
  
  **脑信号建模**
  - 使用**图谱谱变换（graph spectral transforms）** 刻画 fMRI 信号的非欧空间结构特征。
  
  **图像信号建模**
  - 采用**掩码频率建模（masked frequency modeling）** 处理图像频率域信息。
  
  **粗到细对齐**
  - 通过选择性对齐 **低频、中频、高频**结构，实现由整体到细节的渐进式重建。
  
  **语义先验注入**
  - 引入 **CLIP 文本嵌入**以及多层级视觉特征，将文字语义作为生成过程的锚点。
  - 通过**注意力机制**，让经过频率掩码的脑信号同时与图像重建信息、文本语义线索交互。
  
  **模型架构**
  - 整合预训练的 **VDVAE**、**CLIP** 和 **diffusion backbone** 作为底层生成架构。
  - 新提出三个“频率对齐投影层”：
    1. 低级分层脑到视觉对齐层（low-level hierarchical brain-to-vision layer）
    2. 高层语义脑到视觉对齐层（high-level semantic brain-to-vision layer）
    3. 脑到文本对齐层（brain-to-text alignment layer）

- **说明**：摘要中只是对整个方法逻辑给出概览，未提供正式的公式定义或算法伪代码，因此无法从给定内容中提取精确的数学表达与训练目标细节。

## 3. 实验设计

- **数据集**：该论文摘要中**没有明确提及任何 fMRI 数据集名称**（例如 Generic Object Decoding、NSD、BOLD5000 等），无法从给定信息中判断具体数据来源与规模。
- **Benchmark 标准**：摘要同样未提供所采用的基准或评价协议；如通常图像重建中使用的 PixCorr、SSIM、Inception Score、FID 等指标，在现有文本中没有列出。
- **对比方法**：没有给出任何被比较的基线方法名称（如先前工作中的 SD、Mind-Vis 等）。因此无法客观评估其与 SOTA 的相对位置。
- **实验侧写**：从摘要表述中可以推断，实验展示的可能是定量结果之外的定性结果——即生成图像可以在“偏移像素级 ground truth 的情况下”，呈现出某种与语义内容相关的涌现结构。

## 4. 资源与算力

- **完全未提及**。
- 在给定摘要和元数据中，没有提供所使用的 GPU 型号、GPU 数量、训练时长、参数量或能耗等任何资源信息。
- 如果依赖于该文进行复现，则需要另找论文全文/附录，或联系作者，才能获知具体的算力配置。

## 5. 实验数量与充分性

- **实验数量**：给定内容中无法确认做了多少组实验（主实验、多个数据集验证、消融实验等均未提及）。
- **充分性判断**：由于该摘要既没有给出指标、图像重建示例，也没有列出基线、消融或统计显著性测试，因此**从文本层面看实验证据不足**，无法充分支撑这些技术模块的独立贡献。
- **客观性/公平性问题**：
  - 无透明基线、无跨数据集结果，读者无法判断方法在统一条件下是否具有优势。
  - 鉴于“创造性重建”本身难以量化，若缺少用户调研或认知科学层面的评测设计，实验结果容易被认为过于主观。
  - 该文被标记为 ICLR-2026-Rejected，可能与实验验证的不足有关（至少这是元数据层面的一个重要信号）。

## 6. 论文的主要结论与发现

- **方法与目标改变重建范式**：本文证明可以用“频率结构 + 语义先验”的方式，将 fMRI 重建从单纯的像素复制导向生成性的神经—视觉语义对齐。
- **生成结果有语义层面的涌现结构**：模型生成的图像可以合理偏离原始刺激的像素级 form，但却能保留或创造性地体现视觉场景所蕴含的高层语义与脑内再编码信息。
- **频率桥接是有效切入点**：通过在脑信号、视觉图像与文本语义之间协同建模不同频率成分，可以让生成过程兼顾整体结构（低频）、物体轮廓（中频）和细节纹理（高频）。
- **为“感知、想象、创造”服务**：作者认为该框架不仅适用于重建任务，更潜在适用于研究人类如何感知视觉材料、如何想象以及如何在记忆中重新组织感知信息。

## 7. 优点

- **问题提出有新意**：把“像素还原”问题提升到“人脑如何创造性地表征视觉信息”这一层面，研究视野更宽，而不仅仅是在生成指标上优化数字。
- **多频段粗到细设计的合理性**：图谱谱变换 + 掩码频率建模，能够在脑信号和图像之间建立更自然的频域对应关系，在理论层面具有较强可解释性。
- **多模态语义支撑**：联合使用 fMRI、图像低频纹理和 CLIP 文本语义，使模型能够将非语言的生理信号映射到可命名、可推理的语义空间。
- **模块化架构思路清晰**：引入三个分工明确的投影层（低级视觉层、高级语义层、脑到文本层），结构上体现了解剖学或信息流分层的思想。
- **超越了纯技术应用**：其结论和实验叙事传递了面向认知科学/神经科学的潜在启发，对跨学科脑与视觉生成研究有一定参考价值。

## 8. 不足与局限

- **模态局限性**：该工作是针对 fMRI 设计的，并非 EEG。脑电图等便携式模态的信号结构差异很大，因此方法不能直接迁移；元数据中也明确提示“并非 EEG 模态”，所以它无法直接支持 EEG 相关脑视觉解码需求。
- **实验证据与量化不足**：从摘要中没有看到任何数据集名称、评价指标体系、消融实验、误差棒或统计测试，难以从实证上确认该方法确实优于常见的像素级重建基线。
- **“创造性”评估难度大**：作者强调生成图像可以偏离 ground truth，但衡量这种偏离是否属于“更合理的创造性编码”需要有认知/行为学评估依据，否则容易陷入无约束的自由生成。
- **缺少完整技术细节与源码信息**：没有给出具体频率分层方式、图谱谱变换的图结构构建方法、公式化损失函数以及训练阶段使用的数据增强策略，降低了复现性。
- **被拒稿带来的风险**：根据检索元数据该文处于 ICLR 2026 Rejected 状态，说明其在评审中可能存在较强质疑（例如实验量不足、论证不够严谨、贡献范围不够广泛等），阅读时应保持批判态度。
- **未报告算力与基准**：这也是一大短板——缺少算力资源和基准评测，限制了领域内研究者在真实标准数据集上对比验证其效果的可操作性。

---

**（完）**
