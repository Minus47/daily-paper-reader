---
title: The hallmark of colour in EEG signals
title_zh: 脑电信号中颜色的标志性特征
authors: Arash Akbarinia
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=oTFEBSMCUh"
tags: ["query:eeg-align"]
score: 6.0
evidence: 用64导脑电在1800多张自然图像观看任务中考察颜色信息是否可解码，属于脑电信号本身与视觉刺激的关联研究
tldr: 颜色对视觉感知很重要，并且已有颜色脑解码研究多集中在简单均匀色块，自然复杂图像中颜色未被显式提示时能否从非侵入式脑电中解码仍不清楚。该工作利用THINGS-EEG数据集，在被试观看1800余张自然图像时记录64通道脑电，检验颜色信息在自然视觉场景下是否仍可从脑电信号中解码。这项研究有助于把脑电作为视觉体验信息的神经接口，为脑电与图像视觉特征对齐提供依据。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 已有颜色脑解码多限于简单色块，自然复杂图像中非显式提示的颜色信息可否由EEG解码仍不明确。
method: 以THINGS-EEG自然图像数据集为对象，分析64通道EEG中颜色信息的可解码性。
result: 研究有望证明自然图片条件下颜色信息仍能在EEG中留下可解码痕迹。
conclusion: 表明了EEG信号可作为自然视觉颜色信息的有效载体，支持脑电与图像属性的关联建模。
---

## Abstract
Our perception of the world is inherently colourful, and colour has well-documented benefits for vision: it helps us recognise objects more quickly and remember them more effectively. We hypothesised that colour is not only central to perception, but also a rich and decodable source of information in electroencephalography (EEG) signals recorded non-invasively from the scalp. Previous studies have shown that colour can be decoded from neuroimaging brain signal to simple, uniformly coloured stimuli, but it remains unclear whether this extends to natural, complex images where colour is not explicitly cued.
To investigate this, we analysed the THINGS-EEG dataset, in which 64-channel EEG was recorded while participants viewed over 1,800 Our perception of the world is inherently colourful, and colour provides well-documented benefits for vision: it helps us see things quicker and remember them better. We hypothesised that colour is not only central to perception but also a rich, decodable source of information in electroencephalography (EEG) signals recorded non-invasively from the scalp. While previous work has shown that brain activity carries colour information for simple, uniform stimuli, it remains unclear whether this extends to natural, complex images with no explicit colour cueing.
To investigate this, we analysed the THINGS EEG dataset, which contains 64-channel recordings from participants viewing 1,800 distinct objects (16,740 images) presented for 100 ms each, yielding over 82,000 trials. We established a perceptual colour ground truth through a psychophysical experiment in which participants viewed each image for 100~ms and selected the perceived colours from a 13-option palette. An artificial neural network trained to predict these scene-level colour distributions directly from EEG signals showed that colour information was robustly decodable (average F-score of 0.5).
We further examined the effect of colour features on object decoding. Using a contrastive learning framework, we modelled colour–object perception with the Segment Anything Model (SAM), in which all pixels within a segment were replaced with their average colour, followed by standard feature extraction using CLIP vision encoders. We trained an EEG encoder, CUBE (ColoUr and oBjEct decoding), to align features in both object and colour spaces. Across EEG and MEG datasets in a 200-class recognition task, incorporating colour improved decoding accuracy by approximately 5%.
Together, these findings demonstrate that EEG signals recorded during natural vision carry substantial colour information that interacts with object perception. Modelling this interaction enhances the power of neural decoding.

---

## 论文详细总结（自动生成）

根据提供的论文内容（包括元数据与摘要），以下是详细的中文总结：

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：颜色是人类视觉感知的核心组成部分，已知能够加速物体识别并增强记忆。然而，先前的神经解码研究多局限于简单的、颜色均匀的刺激（如色块），很少探究在观看自然、复杂图像时，颜色信息（且未被显式提示）能否从非侵入式脑电（EEG）信号中被有效解码。
- **核心问题**：颜色不仅是感知的中心，它是否也能在头皮记录的EEG信号中留下丰富的、可解码的痕迹？特别是在自然视觉场景中（图像复杂多变、颜色分布不均匀且无显式引导），这种解码能力是否依然成立。
- **整体含义**：这项研究致力于验证EEG信号能否作为自然视觉中颜色信息的有效载体，从而支持基于脑电与图像视觉属性（尤其是颜色）的关联建模，为脑机接口（BCI）和视觉体验解码提供依据。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：结合大规模自然图像EEG数据集、人类主观颜色感知实验以及深度学习模型（对比学习与大规模预训练视觉模型），检验颜色信息的可解码性，并进一步建模颜色与物体感知之间的交互作用。
- **技术细节与算法流程**：
  - **数据基础**：使用THINGS-EEG数据集，包含64通道EEG记录（被试观看100ms的图像刺激，共超过82,000次试验）。
  - **感知颜色真实值（Ground Truth）建立**：开展心理物理学实验。实验中，参与者以100毫秒的时长观看每张图像，并从包含13个选项的调色板中选择所感知到的颜色。这一步骤确立了“场景级颜色分布”的感知基准。
  - **颜色信息解码验证**：训练一个人工神经网络（ANN），直接以EEG信号为输入，预测上述由人类感知定义的场景级颜色分布。通过计算平均F-score来评估解码性能（报告平均F-score为0.5）。
  - **颜色-物体联合建模（CUBE模型，ColoUr and oBjEct decoding）**：
    - 使用Segment Anything Model（SAM）对图像进行分割，并将每个分割区域内的所有像素替换为该区域的平均颜色。
    - 采用CLIP视觉编码器对处理后的图像进行标准特征提取。
    - 训练一个EEG编码器（称为CUBE），将EEG特征与上述经过颜色处理的图像特征进行对齐（涉及物体空间与颜色空间两个维度）。

### 3. 实验设计：数据集、Benchmark与对比方法
- **数据集**：
  - **主要数据集**：THINGS-EEG。该数据集记录了64通道脑电，包含1,800多个不同对象类别的16,740张自然图像，总试验次数超过82,000次。
  - **辅助数据集**：研究还涉及MEG数据集（用于跨模态验证）。
- **Benchmark与实验场景**：
  - 在一个包含200个类别的物体识别任务中进行了评估（这构成了标准的基准测试）。
  - 涉及两组主要的解码实验：一是直接解码EEG中的场景级颜色分布；二是在物体识别任务中评估颜色特征对解码精度的贡献（结合MEG数据验证）。
- **对比与分析**：
  - 对比了是否将颜色特征纳入解码模型时的性能差异（即颜色特征的增益效应）。
  - 对比了EEG与MEG两种不同神经成像模态下的表现。

### 4. 资源与算力
- 提取的文本摘要中**未明确提及**所使用的GPU型号、数量、总训练时长或具体的算力资源消耗。
- 值得注意的是，该研究依赖大规模预训练模型（如SAM、CLIP）以及大型EEG数据集（82,000余次试验），整体运算成本可能较高，但论文原摘要部分并未提供相关细节。

### 5. 实验数量与充分性
- **实验数量**：
  - 主要实验包括：心理物理学实验（涉及1,800多张图像的颜色感知标注）、EEG颜色解码实验（F-score评估）、多模态（EEG与MEG）200类识别任务实验以及颜色特征消融/增益分析。
- **充分性与公平性评估**：
  - 数据集规模较大（超8万次EEG试验），使用了跨模态（EEG与MEG）数据验证，增强了结论的普适性。
  - 实验设计方面，采用人类主观感知颜色作为标签而非简单的像素统计值，提高了评估的真实性与生态效度。
  - 然而，文本摘要中未报告消融实验的具体数量（例如去掉颜色模块后的性能对比细节）、统计分析显著性以及被试间的差异分析，摘要中的“约5%提升”也缺乏误差线和具体统计学检验，因此关于公平性的细节需在完整论文中核实。

### 6. 论文的主要结论与发现
- **发现一**：自然视觉期间记录的EEG信号包含大量的颜色信息，通过人工神经网络可稳健地解码出感知层面的场景级颜色分布（F-score ≈ 0.5），表明颜色在EEG中是“可解码”且“富含信息”的。
- **发现二**：颜色特征与物体感知存在交互作用。在200类物体的识别任务中，通过CUBE模型联合建模颜色与物体信息，能够有效提升解码准确率（在EEG和MEG数据集上均提升约5%）。
- **总体结论**：颜色不仅对感知重要，而且在EEG信号中留下了神经痕迹，建模这种“颜色-物体”交互可以增强神经解码能力，促进脑电与图像视觉特征的对齐。

### 7. 优点：方法或实验设计上的亮点
- **生态效度高**：使用自然复杂图像数据集（THINGS-EEG）而非简单均匀色块，弥补了以往研究缺乏自然场景验证的不足。
- **定义了感知色彩基准**：通过心理物理学实验获得“人类感知的颜色分布”，而非依赖单纯的色彩像素统计，更贴近视觉感知的本质。
- **巧妙融合先进模型**：引入SAM进行物体分割并施加颜色扰动，配合CLIP编码器提取特征，再通过CUBE与EEG隐空间对齐，方法具备一定的新颖性。
- **多模态有效性验证**：同时验证了EEG与MEG数据，提高了结论的可靠性与泛化性。
- **实用性导向**：证明了颜色先验信息在视觉脑解码任务中的增益，为脑机接口与神经解码算法的改进提供了数据支持。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制
- **算力与细节缺失**：正文摘要中未报告计算资源（GPU）具体配置及训练耗时，不利于评估成本与复现难度。
- **结论的普适性风险**：THINGS数据集包含的是对象居中、无复杂背景变化的图像，与真实世界“无控制”的自然场景（如极具干扰的背景、多变构图）尚有差距，结论能否推广至完全开放式环境仍需考证。
- **时间动态分析缺乏**：脑电解码（如F-score=0.5）多是整体平均的结果，摘要中未涉及颜色信息出现的具体时间窗（如ERP成分或时频动态分析），难以精确解释颜色编码的神经时间机制。
- **参数维度受限**：颜色感知被压缩为13个调色板选项，忽略了连续色度空间、饱和度和明度的精细差异，可能损失部分颜色信息。
- **颜色-物体交互机制理解局限**：虽然提升了识别精度，但“模型如何作用”（是通过增强CLIP特征还是调整编码空间）的机制尚不清晰，仅报告了性能增益，未能提供神经科学层面对交互机制的深入解读。

（完）
