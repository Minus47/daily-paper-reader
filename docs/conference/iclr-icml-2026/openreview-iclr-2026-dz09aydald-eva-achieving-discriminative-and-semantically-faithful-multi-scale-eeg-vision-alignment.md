---
title: "EVA: Achieving Discriminative and Semantically Faithful Multi-Scale EEG-Vision Alignment"
title_zh: EVA：实现可判别且语义保真的多尺度脑电-视觉对齐
authors: "Enze Shi, Huawen Hu, Yincheng Yao, Sigang Yu, Shu Zhang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=dZ09ayDALD"
tags: ["query:eeg-align"]
score: 9.0
evidence: 用统一对比学习架构实现脑电与图像、视频、三维旋转等视觉表征的多尺度对齐
tldr: 脑电解码视觉语义常局限于单一视觉刺激，难以在图像、视频等不同尺度间泛化。EVA在统一对比学习架构中建立多尺度脑电与异构视觉刺激的对齐，可同时处理快速图像、连续视频序列和三维物体旋转。其通用脑电编码器通过频率感知动态编码模块适应不同刺激下的动态特征。实验结果显示EVA在判别力与语义保真度上均表现良好，为脑电-视觉对齐提供通用框架。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有脑电-视觉对齐方法通常面向单一视觉模态，难以处理图像、视频等多刺激尺度。
method: 提出统一对比学习框架，用频率感知动态编码模块在多个视觉刺激条件下对齐脑电表征。
result: EVA在图像、视频和三维刺激上均获得判别性强且语义保真的脑电对齐结果。
conclusion: 统一的多尺度对比对齐可支持复杂视觉场景的脑电解码，具有较强通用性。
---

## Abstract
Decoding semantic information from electroencephalography (EEG) signals elicited by diverse visual stimuli remains a critical challenge in brain-computer interfaces and cognitive neuroscience. Existing approaches typically align EEG with single-modality visual stimuli but struggle to generalize across multiple modalities and temporal scales. We propose EVA (EEG-Vision Alignment), the first framework that unifies multi-scale EEG alignment with heterogeneous visual stimuli, including rapid image presentations, continuous video sequences, and 3D object rotations, within a single contrastive learning-based architecture. EVA’s Universal EEG Encoder features two key innovations: (1) a Frequency-Aware Dynamic Encoding (FADE) module that transforms EEG signals into the frequency domain via real-valued fast Fourier transform, enabling compact, adaptive representations through adjustable band-pass filtering; and (2) an Adaptive Channel Clustering (ACC) module that dynamically updates channel groupings using cross-attention and gradient-based optimization, capturing inter-channel synergies while mitigating noise. By optimizing EEG features to achieve both discriminative power for robust classification and semantic fidelity for high-quality reconstruction from brain signals, our framework achieves state-of-the-art performance across diverse tasks, including image retrieval, video classification, and 3D object recognition, on multiple datasets. Notably, our zero-shot reconstruction of 200 object categories from the THINGS-EEG dataset, using only aligned EEG features without textual or low-level cues, surpasses prior state-of-the-art by a significant margin. These results underscore EVA’s capability to extract robust, generalizable representations from EEG signals, demonstrating the superiority of our unified framework. Code will be released upon publication.

---

## 论文详细总结（自动生成）

## EVA：实现可判别且语义保真的多尺度脑电-视觉对齐——中文总结

> 说明：以下总结仅基于 OpenReview 页面提供的论文标题、摘要、元数据及 tldr 信息生成。因未能获取完整 PDF 正文，部分细节（如公式、实验表格、具体数据）无法展示，相关位置会明确标注“原文未提供”。

### 1. 核心问题与研究动机

- **背景问题**：从脑电（EEG）信号中解码视觉语义信息，是脑机接口与认知神经科学中的关键挑战。
- **现有局限**：已有脑电-视觉对齐方法大多只针对**单一模态的视觉刺激**（如图像）进行对齐，难以在多种视觉模态（如图像、视频、3D 物体）以及不同时间尺度上泛化。
- **核心缺口**：缺少一个能够统一处理“快速图像呈现”“连续视频序列”“三维物体旋转”等异构视觉刺激的脑电-视觉对齐框架。
- **总体意义**：如果能实现跨模态、多尺度的 EEG 对齐，可大幅提升脑电解码的通用性与可迁移性，为更复杂视觉场景中的脑机交互提供基础。

### 2. 方法论

#### 核心思想

- 提出 **EVA（EEG-Vision Alignment）**框架——据论文称是**首个**在单一对比学习架构中，将 EEG 与异质视觉刺激（图像、视频、3D 旋转物体）进行**多尺度统一对齐**的框架。

#### 总体架构

- 采用统一对比学习范式，将脑电特征映射到与视觉表征对齐的共享语义空间；
- 优化目标兼顾两方面的特征质量：
  1. **判别力（Discriminative Power）**：支持强健的分类/识别；
  2. **语义保真度（Semantic Fidelity）**：支持从脑电信号高质量重建视觉内容。

#### 关键技术模块

- **Universal EEG Encoder（通用脑电编码器）**，内部包含两个关键创新模块：

1. **Frequency-Aware Dynamic Encoding（FADE），频率感知动态编码模块**
   - 使用**实值快速傅里叶变换（real-valued FFT）**将 EEG 从时域变换到频域；
   - 通过**可调节的带通滤波**生成紧凑、自适应的频域表征；
   - 目的是根据不同刺激条件下的动态特征进行自适应编码。

2. **Adaptive Channel Clustering（ACC），自适应通道聚类模块**
   - 通过**交叉注意力（cross-attention）**与**基于梯度的优化**动态更新脑电通道分组；
   - 可捕获通道间的协同效应（inter-channel synergies），同时抑制噪声干扰。

- **训练/对齐方式**：在同一对比学习框架内，使 EEG 表征与对应视觉刺激的表征相互对齐，同时通过分类等辅助任务提升判别性；重建任务约束则用于保证语义保真。

> **关于公式与算法流程**：本材料所提供信息有限——摘要未给出具体损失函数公式、网络层数、注意力维度或训练伪代码。若需详细公式与伪代码，请查阅原论文正文。

### 3. 实验设计

- **视觉刺激/任务场景**（均由论文明确提及）：
  1. **快速图像呈现**：图像检索/分类；
  2. **连续视频序列**：视频分类；
  3. **3D 物体旋转**：3D 物体识别。
- **核心数据集**：论文明确提及 **THINGS-EEG** 数据集；并使用该数据集进行 **200 类物体**的零样本重建实验。
- **Benchmark 与对比方法**：论文声称在图像检索、视频分类、3D 物体识别等多个任务上与**现有最先进方法（SOTA）**进行了对比，达到最优效果；但摘要未列出具体对比方法名称与数值。

### 4. 资源与算力

- 原文（仅摘要与元数据）**未明确说明**：
  - GPU 型号与数量；
  - 训练总时长；
  - 模型参数量；
  - 具体显存/能耗开销。
- 因此无法就算力成本做出评估。若需分析效率与可复现性，需查阅论文正文的实验设置部分。

### 5. 实验数量与充分性

- 从摘要可见的实验覆盖面至少包括：
  - **图像任务**：图像检索（含分类相关评估）；
  - **视频任务**：视频分类；
  - **三维任务**：3D 物体识别；
  - **重建任务**：THINGS-EEG 数据集 200 类物体的零样本重建。
- 是否进行了消融实验：摘要未明确列出消融表；但 FADE 与 ACC 两个模块作为“关键创新”，很可能会在正文中有消融分析——需要以原文为准。
- **充分性初步评估**：
  - 优点：实验场景覆盖了“静态图像—时间序列视频—三维空间旋转”三种不同视觉尺度，形式丰富；
  - 不确定性：单一数据集（THINGS-EEG）的零样本重建结果虽醒目，但跨数据集的泛化验证情况未知；对比方法的数量、同设置公平性、多次重复的误差范围等信息摘要中未给出；
  - 结论的完整性需等待原文实验章节确认。

### 6. 主要结论与发现

- EVA 在图像检索、视频分类、3D 物体识别等多样任务上均达到 SOTA 性能，实现了**判别性强且语义保真**的 EEG 表征。
- 在 THINGS-EEG 的 200 类物体零样本重建任务中，**仅使用对齐后的 EEG 特征**、不借助文本或低级视觉线索，即显著超过此前 SOTA。
- 表明统一的多尺度对比对齐框架有助于从 EEG 中提取**鲁棒、可泛化**的表征，展示了跨模态脑电视觉对齐的通用性优势。

### 7. 主要优点

1. **突破单一视觉模态局限**：首次尝试用统一框架同时覆盖图像、视频、3D 旋转三类刺激尺度，方向新颖；
2. **频率域动态编码设计合理**：FADE 通过实值 FFT 与带通滤波，有效适应不同刺激的动态频域特征；
3. **自适应通道建模**：ACC 利用交叉注意力与梯度信息动态聚类通道，能兼顾通道协同与噪声抑制；
4. **优化目标兼顾判别与生成**：将分类/检索所需的判别力与重建所需的语义保真结合，避免常用对比学习方法过于侧重判别、丢失细粒度语义的问题；
5. **零样本重建表现突出**：在无文本/低级特征辅助的条件下，重建正确率大幅超过 SOTA，体现出对齐空间的质量较高；
6. 将开源代码，具备一定可复现性基础。

### 8. 不足与局限

1. **信息不全**：本材料不包含完整实验细节、具体公式、超参数设置与网络结构描述，无法充分评估技术实现的完整度；
2. **数据集覆盖有限**：目前明确报告数据集主要是 THINGS-EEG，多数据集泛化证据不足；真实脑电采集成本高，实验中是否充分覆盖不同被试、不同设备仍需原文确认；
3. **跨尺度公平性存疑**：图像、视频、3D 刺激下脑电信号的时间特性和噪声水平差异较大，统一框架是否在不同任务中使用了相同超参、是否保证各任务调参一致，摘要中无从判断；
4. **可解释性欠缺**：FADE 的频段选择与 ACC 的通道分组虽可优化性能，但其神经生理学可解释性未有进一步讨论；
5. **计算成本未知**：未报告训练与推理开销，可能限制资源有限的研究组复现或实时脑机接口应用；
6. **受试者/跨时间稳定性问题**：EEG 信号个体差异大，摘要未提及跨被试对齐、域适应或脑电解码鲁棒性的相关方案；
7. **应用局限**：零样本重建虽然效果好，但仅限 200 类物体，面向开放真实场景的泛化仍需验证。

---

（完）
