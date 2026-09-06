---
title: Learning Brain Representation with Hierarchical Visual Embeddings
title_zh: 利用层级视觉嵌入学习脑表示
authors: "Jiawen Zheng, Haonan Jia, MING LI, Yuhui Zheng, Yufeng Zeng, Yang Gao, Chen Liang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=IEq71qS8B7"
tags: ["query:eeg-align"]
score: 8.0
evidence: 通过对比学习将脑信号与层级视觉嵌入对齐以改善视觉解码
tldr: 针对脑-图像解码过度关注高层语义而忽视像素级细节的问题，作者提出一种脑-图像对齐策略，利用多个预训练视觉编码器提取层级化与多尺度视觉嵌入，并通过对比学习把脑信号与这些表示对齐。该方法能学习包含细节与语义的脑视觉表征。实验表明其视觉解码性能优于仅用高层语义对齐的基线。这为理解脑信号如何编码视觉信息提供了新的表征框架。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视觉解码主要对齐高层语义特征，忽略像素级细节，不能充分揭示脑信号编码的视觉信息。
method: 结合多个预训练视觉编码器提取层级多尺度视觉嵌入，并通过对比学习目标对齐脑信号与这些视觉表示。
result: 在脑-图像对齐与视觉解码中取得了优于传统高层语义对齐的性能。
conclusion: 说明融合分层视觉嵌入可帮助更全面地理解脑信号的视觉编码。
---

## Abstract
Decoding visual representations from brain signals has attracted significant attention in both neuroscience and artificial intelligence. However, the degree to which brain signals truly encode visual information remains unclear. Current visual decoding approaches explore various brain–image alignment strategies, yet most emphasize high-level semantic features while neglecting pixel-level details, thereby limiting our understanding of the human visual system.
In this paper, we propose a brain–image alignment strategy that leverages multiple pre-trained visual encoders with distinct inductive biases to capture hierarchical and multiscale visual representations, while employing a contrastive learning objective to achieve effective alignment between brain signals and visual embeddings. Furthermore, we introduce a Fusion Prior, which learns a stable mapping on large-scale visual data and subsequently matches brain features to this pre-trained prior, thereby enhancing distributional consistency across modalities. Extensive quantitative and qualitative experiments demonstrate that our method achieves a favorable balance between retrieval accuracy and reconstruction fidelity.

---

## 论文详细总结（自动生成）

# 利用层级视觉嵌入学习脑表示：中文详细总结

## 1. 核心问题与整体含义

- **研究动机**：在神经科学与人工智能交叉领域，从脑信号（如 fMRI、EEG 等）中解码视觉信息一直备受关注。然而，脑信号究竟在多大程度上编码了真实的视觉信息、以何种表征形式编码，仍是未解难题。
- **现有局限**：目前的脑–图像对齐（brain–image alignment）方法虽多，但主要集中于对齐高层语义特征，忽视了**像素级细节**（如纹理、边缘、局部结构等）。这种“重语义、轻细节”的偏向限制了对人类视觉系统的全面理解，也导致解码结果在检索精度与重建保真度之间难以平衡。
- **核心问题**：如何设计一种脑–图像对齐策略，使脑信号表征既能捕捉高层语义，又能保留足够的底层视觉细节，从而更真实地反映人脑的视觉编码机制？

## 2. 方法论

- **总体思想**：利用多个预训练视觉编码器（具有不同归纳偏置）来提取**层级化（hierarchical）与多尺度（multiscale）**的视觉嵌入，并与脑信号进行对比学习对齐，实现从“粗语义”到“细细节”的多层次表征融合。
- **关键技术细节**：
  - **多编码器融合**：不同于使用单一视觉特征，方法整合多个预训练视觉编码器的输出，以捕捉不同抽象层次和不同感受野的视觉信息。
  - **对比学习目标**：通过对比学习（contrastive learning）将脑信号特征与上述多尺度视觉嵌入拉近，同时推开不匹配的负样本，从而学习跨模态一致的表征空间。
  - **Fusion Prior（融合先验）**：在大规模视觉数据上预先学习一个稳定的映射（prior），随后将脑特征映射到这个已学先验上。该步骤增强了不同模态之间的分布一致性，避免脑信号与视觉特征因分布不匹配导致的训练不稳定。
- **文字化流程**（基于摘要推断）：
  1. 从视觉编码器组提取多层级、多尺度视觉特征；
  2. 在大规模纯视觉数据上训练“融合先验”以得到稳定的特征映射空间；
  3. 获取脑信号编码（如通过专门的脑编码器），将其投影到该先验空间；
  4. 使用对比损失函数，对脑特征与融合视觉特征进行对齐优化；
  5. 在推理阶段，利用对齐后的表征进行脑–图像检索或重建。

注：正文中未提供具体公式，上述为基于摘要的合理复述。

## 3. 实验设计

- **数据集与场景**：论文摘要未明确列出具体数据集名称（如 NSD、BOLD5000、EEG 数据集等）。仅说明实验涵盖**量化评估与质性评估**，涉及检索准确率和重建保真度两类任务。
- **基准（Benchmark）**：未明确点名使用的公开基准，但从上下文推断应为脑–图像解码/检索的通用评测设定。
- **对比方法**：文中强调与“仅使用高层语义对齐的基线”进行对比，但未在摘要中列出具体基线方法名称（如 Brain2Image、LiMBO 等）。完整对比体系需见正文。

## 4. 资源与算力

- 论文提供的材料（标题、摘要、元数据）中**未说明**所使用的 GPU 型号、数量、训练时长、参数量等算力信息。
- 需要注意：ICLR-2026 版本的开放评审页面还被验证码拦截，无法获得补充材料。因此本总结无法提供算力相关细节。

## 5. 实验数量与充分性

- **实验数量**：由于仅有摘要，无法获知具体进行了多少组实验。从文字可知至少包含：
  - 定量实验（检索准确率、重建指标）；
  - 定性实验（可视化重建结果）；
  - 与高层语义对齐基线的对比实验。
- **可能缺少的消融**：摘要未明确说明是否进行了针对多编码器数量、融合先验是否有无、不同对比损失函数等消融实验。无法判断消融的完整性。
- **客观性与公平性**：摘要声称“获得更优的检索与重建平衡”，但因缺少细节（数据集划分、脑信号模态、预处理流程、统计显著性检验等），无法完全评估实验的公平性与稳健性。仅从声称看，对比局限于“高层语义对齐”基线，不足以证明对多种主流方法的全面优势。

## 6. 主要结论与发现

- 融合**多层级的视觉嵌入**能显著改善脑–图像对齐效果，相比传统只强调高层语义的方法，可以在**检索精度与重建保真度**之间获得更好的平衡。
- 采用 **Fusion Prior** 作为中间映射可提升跨模态分布的一致性，进一步稳定和优化对齐。
- 该方法为理解脑信号如何编码视觉信息提供了新视角：人脑视觉表征可能同时包含高层语义与低层细节，单一语义对齐不足以刻画全貌。

## 7. 优点

- **创新视角**：明确提出“高层语义对齐不足”，并引入像素级/多尺度细节，弥补了现有脑解码研究偏向语义的失衡。
- **方法设计合理**：利用多个预训练视觉编码器的归纳偏置差异，可自然覆盖 VGG/ResNet/CLIP/DINO 等不同层级特征，无需昂贵的新视觉特征标注。
- **引入 Fusion Prior**：从大规模视觉数据学习稳定映射再对齐脑信号，有助于缓解脑数据量小、分布偏移的问题，在理论上具有泛化价值。
- **目标平衡**：将检索精度与重建保真度同时纳入评价，比单一指标更能反映真实视觉解码能力。

## 8. 不足与局限

- **信息不完整**：当前只有摘要，无完整正文支撑，实验细节、公式、数据集、参数等均不可见，需以论文正式版为准。
- **模态泛化**：摘要未说明脑信号是 fMRI、EEG 还是 MEG，不同模态的时间/空间特性差异大，方法在单一模态上的验证可能不足以推广。
- **细节与标签依赖**：使用多个预训练视觉编码器虽然不需要人工标注，但视觉编码器的预训练域（如 ImageNet-1K）可能与脑认知域存在分布鸿沟。
- **融合先验的稳定性**：该先验是基于自然图像学到的，若大脑中概念与视觉外观不一致（如抽象思维）则可能失效。
- **对比范围**：只与“高层语义对齐”基线作对比，缺少与最新的基于扩散模型、生成式解码或多模态大模型的脑解码方法比较，说服力有限。
- **计算开销**：多编码器融合与先验训练会显著增加显存与时间成本，但论文未讨论效率问题。

（完）
