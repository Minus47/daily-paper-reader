---
title: "BrainLMM: A Label-Free Framework for Mapping Multi-Semantic Representation in the Human Visual Cortex"
title_zh: BrainLMM：用于人脑视觉皮层多语义表示映射的无标签框架
authors: "Tan Gao, Mufan Xue, Haofang Zheng, Shuo Lv, Jia Xu, Dabin Sheng, Ziming Mao, Xinyu Wu, Andrew Luo, Guoyuan Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42413/46374"
tags: ["query:abstraction"]
score: 8.0
evidence: 以无标签方式将多个共存语义概念映射到人脑视觉皮层体素反应，直接涉及语义概念意义及其神经对应关系。
tldr: 以往研究用人工神经网络探究视觉皮层语义编码，但缺少无需标签就能刻画同时共存的多个语义概念的可解释映射框架。BrainLMM组合多种视觉编码器与Describe-and-Dissect策略，先建立体素级编码模型预测自然图像的皮层反应，再用无标签方式对每个体素映射多个语义概念。该方法支持对人脑高级视觉皮层进行假设自由的语义组织分析，并可将多个视觉编码器对应到不同语义维度。实验表明它能有效预测脑反应并发现多重语义表示，为后续构建脑对齐的语义空间提供了工具。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 已有研究依赖人工标注或单一模型，难以无假设地揭示视觉皮层中多个语义概念并存的表征结构。
method: BrainLMM结合多种视觉编码器与Describe-and-Dissect策略，在无标签条件下建立体素级编码模型并映射多重语义成分。
result: 该框架可预测自然图像皮层反应，并对视觉皮层进行多语义概念的无标签映射，支持假设自由分析。
conclusion: 提供从脑活动到多语义表征的可解释映射工具，对构建类脑语义空间和人机概念对齐研究有重要价值。
---

## Abstract
Previous studies leveraging artificial neural networks have been used to investigate the semantic coding within human visual cortex. However, building an interpretable label-free framework that can effectively map brain responses to multiple coexisting semantic concepts remains largely unexplored. Here, we propose BrainLMM, a label-free framework for multi-semantic mapping of voxel responses by combining diverse vision encoders with the Describe-and-Dissect strategy, enabling a hypothesis-free analysis of the human high-level visual cortex. First, we construct voxel-wise encoding models leveraging diverse vision encoders to predict visual cortical responses to natural scene images. Then, we use BrainLMM to map individual brain voxels to multiple semantics without requiring any predefined labels. To evaluate the effectiveness of our method, we compute Pearson correlation coefficients to compare the multi-semantic mappings produced by BrainLMM and CLIP-MSM with ground-truth voxel responses within selective cortical areas. Our findings indicate that BrainLMM achieves more accurate predictions of visual responses compared to CLIP-MSM. Finally, to demonstrate the multi-semantic mapping capability of our method, we project multiple representative semantic concepts onto the cortical surface for visualization. Our method enables the discovery of voxels that exhibit strong activation in response to previously undefined semantic concepts across two independent datasets: the Natural Scenes Dataset (NSD) and the Natural Object Dataset (NOD).

---

## 论文详细总结（自动生成）

# BrainLMM论文详细中文总结

## 一、论文的核心问题与整体含义

### 研究动机与背景
- 人类视觉皮层能将高维感官输入转化为结构化的语义表征，在多个皮层区域形成类别选择性激活模式（如面孔、地点、身体、文字、食物等）。
- 已有大量研究表明，高级视觉区域存在功能特化，但传统研究大多依赖**预定义标签**（如人工标注的类别），容易引入主观偏差。
- 现有可解释编码模型存在两类局限：
  - 支持多语义映射的方法依赖预定义标签集，无法脱离类别先验；
  - 支持无标签分析的方法通常将每个体素关联到**唯一的主导概念**，无法捕捉一个体素同时对应多个语义概念的真实神经编码特性。
- 因此，论文提出一个核心问题：**能否构建一个无需标签、可解释的框架，实现人脑视觉皮层中单个体素对多个共存语义概念的联合映射？**

### 整体含义
- 该研究属于认知神经科学与人工智能的交叉领域，旨在建立一个“假设自由”（hypothesis-free）的分析工具，直接从fMRI数据中发现视觉皮层中先前未被定义/预测的语义选择性，推进对大脑语义组织原理的理解。

## 二、论文提出的方法论

### 核心思想
BrainLMM（Brain Language-Model-based Multi-semantic Mapping）结合**多种视觉编码器**与改进的**Describe-and-Dissect（DnD）策略**，将单个体素映射到多个语义标签，全程不依赖任何预定义类别。

### 总体架构（分三阶段）
1. **体素级编码模型构建**
   - 使用CLIP（ViT-B/32与ResNet50骨干）及ImageNet预训练模型（ResNet50、AlexNet）提取图像特征。
   - 为每个体素单独建立岭回归（ridge regression）编码模型，预测该体素对自然图像的平均fMRI响应。
   - 通过最小化预测值与真实响应之间的均方误差（MSE）进行优化。
2. **标签解码/映射**
   - 在训练好的编码模型上执行改进的DnD分析：
     - 数据集增强：利用Otsu阈值法分割激活图的前景/背景，通过轮廓检测和边界框裁剪生成聚焦显著性区域的增强图像。
     - 候选标签生成：对体素筛选Top-N个强激活图像，输入BrainVLPT模块（基于BLIP图像转文本模型 + 分词器），提取名词作为候选标签集合。
     - 多语义概念选择：对每个候选标签用扩散模型合成t张图像，送回编码模型，根据体素响应排名计算概念得分，最终选取多个代表性标签。
   - 概念得分公式：

     \[
     V(T_i) = -\frac{1}{a}\sum_{j=1}^{a} T_{ij}^2
     \]
     
     - 其中 \(T_{ij}\) 为标签 \(l_i\) 对应合成图像在体素响应排序中的秩次，取排名最低（激活最弱）的前\(a\)张图像计算平方秩均值，以降低生成图像质量差带来的噪声影响。
3. **皮层表面可视化**
   - 使用UMAP降维 + CLIP文本编码器嵌入语义标签，将语义映射结果以伪彩色投影到皮层表面（Pycortex渲染）。

### 关键技术创新点
- DnD（原用于深度神经网络单元解释）被**首次改进并应用于fMRI响应优化的编码模型**。
- 引入BrainVLPT模块，实现了视觉-语言的开放式词汇标签生成，避免了对固定类别词汇表的依赖。
- 扩散模型生成合成图像 + 排名打分机制，弥补了真实刺激图像有限的不足，使语义映射更加稠密和精细。

## 三、实验设计

### 数据集
| 数据集 | 内容与规模 | 被试 | 评估区域 |
|--------|-----------|------|----------|
| NSD (Natural Scenes Dataset) | 7T fMRI，COCO图像刺激，8名被试 | 8人（6女，19-32岁） | FFA、EBA、RSC、VWFA、FOOD |
| NOD (Natural Object Dataset) | 3T fMRI，ImageNet图像刺激，30人中选9名高质量被试 | 9人（5女，19-26岁） | FFA、EBA、RSC、VWFA |

- 训练/测试分割：85:15。
- 体素编码模型性能采用决定系数R²评估，通过2000次bootstrap重采样 + FDR校正进行显著性检验。

### 对比方法
- **CLIP-MSM**（Yang et al., AAAI 2025）：同样是基于CLIP的多语义映射方法，作为主要基准。
- 两种方法均使用相同CLIP编码模型与体素权重，保证公平对比。

### 评估策略
- **语义映射精度评估**：计算两种方法得到的标签文本嵌入与体素权重的余弦相似度。
- **脑对齐评估**：计算两种方法的多语义映射与真实脑响应之间的Pearson相关系数。
- **可视化评估**：皮层表面投影和区域级标签频率统计。

## 四、资源与算力

- 论文正文中**未明确说明**使用的GPU型号、数量或具体训练/推理时长。
- 仅提到“Pytorch”作为实现框架；未量化整体算力消耗。
- 论文致谢提及获得北京理工大学昇腾计算中心的支持，但未提供具体算力配置。
- **总体评估**：文中缺乏可复现的算力信息。结合方法本身（多个骨干网络 + 扩散模型推理），可以推测计算成本不低，但无法量化和验证。

## 五、实验数量与充分性

### 实验组数概览
- **编码模型×数据集组合**：
  - NSD：4种骨干（ResNet50 CLIP、ViT-32 CLIP、ResNet50 ImageNet、AlexNet ImageNet）× 8名被试。
  - NOD：2种骨干（ResNet50 CLIP、ViT-32 CLIP）× 9名被试。
- **主实验（图2、图5）** 展示S5的详细结果，附录包含其余被试的结果。
- **语义可视化**涵盖FFA/EBA/RSC/FOOD/VWFA五个选择性区域。
- 与CLIP-MSM的对比在多个数据集、多个被试、多个ROI区域中重复验证。

### 充分性与客观性分析
- **优点**：
  - 采用两个大规模独立数据集跨验证，具有较高的外部效度。
  - 多被试、多骨干网络的重复实验增强了结论稳健性。
  - 对比实验使用“标签文本嵌入 vs. 体素权重”的客观余弦相似度，而非依赖主观评判。
- **不足**：
  - **未报告消融实验**（如不使用扩散模型、不引入BrainVLPT等变体对比），难以量化各组件对性能的独立贡献。
  - 跨被试的统计检验主要在示例被试S5上可视化，其他被试的结果仅放入附录，未在正文充分讨论个体差异。
  - 部分ROI的NSD定义使用t>0的低阈值，可能包含较多非选择性体素，影响结果解读。
  - 图中显示的“cake在VWFA中排第一”这类反直觉结果，论文解释为“蛋糕带有文字纹样”，反映了文本/图像混淆风险，提示语义映射可能存在一定噪声。

## 六、主要结论与发现

1. **BrainLMM比CLIP-MSM在语义映射精度上更优**：在EBA、RSC、FOOD、VWFA等区域中，BrainLMM的标签与体素权重之间的余弦相似度显著更高；FFA区域二者中位数相当，但BrainLMM的最低值更高、表现更稳定。
2. **BrainLMM与真实脑活动的对齐更紧密**：在面孔、身体、地点、食物、文字等类别概念上，BrainLMM重建的响应与真实体素响应的Pearson相关系数显著高于CLIP-MSM。
3. **实现了真正的无标签多语义映射**：能够发现先前未定义的新语义概念对应的选择性体素，并在皮层表面呈现出与已知功能区域（FFA、EBA、RSC等）一致的清晰拓扑结构。
4. **跨数据集验证了稳健性**：在NSD和NOD两个独立fMRI数据集上均表现一致，说明框架具有良好的泛化能力。

## 七、优点

- **方法论创新性强**：首次将DnD框架迁移到fMRI体素编码模型，结合BrainVLPT + 扩散模型 + 排序打分机制，形成一套完整的无标签多语义反事实解释流程。
- **支持“体素-多语义”映射**：突破传统“一体素一概念”的研究范式，更真实地反映皮层神经元编码多重语义的特性。
- **假设自由的探索范式**：不依赖预设类别，有效避免研究者先验对发现的引导与限制。
- **丰富的可视化呈现**：通过皮层表面投影、区域标签频率分布、UMAP语义色彩编码等多种方式，直观展示高级视觉皮层的语义组织图谱。
- **公开代码与数据可复现**：提供了GitHub代码仓库（BIT-YangLab/BrainLMM），符合可复现科研要求。

## 八、不足与局限

- **计算资源未披露**：未报告GPU数量、型号、训练时长，不利于同行评估复现成本和效率。
- **缺少消融实验**：无法证明各模块（Otsu增强、BrainVLPT、扩散模型、排序打分）的独立贡献和最优组合。
- **统计比较不够完整**：正文主要展示一名被试的详细比较，其他被试的统计分析放在附录，主文的代表性与说服力有所不足。
- **ROI定义标准不统一**：NSD使用t>0的宽松阈值，NOD使用t>2.3，这一标准差异可能降低跨数据集结果的可比性。
- **语义标签可能存在偏置**：BLIP生成的描述词语受其训练数据分布影响，对某些类别存在词汇偏向；扩散模型生成图像的语义保真度不足时也可能引入噪声（如VWFA中“cake”示例所示）。
- **高视觉皮层之外的泛化未验证**：论文讨论聚焦于腹侧视觉通路中的类别选择性区域，对更广泛脑区（如听觉、额叶、抽象语义表征）的适用性尚未检验。
- **应用场景限制**：需要高质量的fMRI数据和预训练视觉编码模型，对于低信噪比数据或物种间通用性有待进一步探讨。

（完）
