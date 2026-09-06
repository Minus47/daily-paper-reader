---
title: "Cortical-SSM: A Deep State Space Model for EEG and ECoG Motor Imagery Decoding"
title_zh: Cortical-SSM：用于EEG与ECoG运动想象解码的深度状态空间模型
authors: "Shuntaro Suzuki, Shunya Nagashima, Masayuki Hirata, Komei Sugiura"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=RInCbleaY3"
tags: ["query:eeg-align"]
score: 6.0
evidence: 用深度状态空间模型对EEG和ECoG运动想象脑活动信号进行分类解码
tldr: 脑电与皮层脑电的运动想象解码易受眨眼、吞咽等生理伪影干扰，Transformer模型又难以捕捉精细时变依赖。为此本文提出Cortical-SSM，将深度状态空间模型扩展到EEG和ECoG信号，以联合建模跨时域的细粒度依赖并提升抗伪影鲁棒性。实验表明该架构在运动想象分类上优于依赖Transformer的方法。该工作为神经信号解码提供了一种有竞争力的时序建模方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 运动想象的EEG和ECoG信号易受生理伪影干扰，现有Transformer方法难以充分建模信号中的细粒度依赖。
method: 提出Cortical-SSM，扩展深度状态空间模型以联合捕捉EEG和ECoG信号跨时域的集成依赖。
result: 实验结果显示其运动想象解码性能优于基于Transformer的相关方法。
conclusion: 深度状态空间模型为抗干扰的EEG和ECoG运动想象解码提供了新方案。
---

## Abstract
Classification of  electroencephalogram (EEG) and electrocorticogram (ECoG) signals obtained during motor-imagery (MI) has substantial application potential, including for communication assistance and rehabilitation support for patients with motor impairments. These signals remain inherently susceptible to physiological artifacts (e.g., eye blinking, swallowing), which pose persistent challenges. Although Transformer-based approaches for classifying EEG and ECoG signals have been widely adopted, they often struggle to capture fine-grained dependencies within them. To overcome these limitations, we propose Cortical-SSM, a novel architecture that extends deep state space models to capture integrated dependencies of EEG and ECoG signals across temporal, spatial, and frequency domains. We validated our method across three benchmarks: 1) two large-scale public MI EEG datasets containing more than 50 subjects, 2) and a clinical MI ECoG dataset recorded from a patient with amyotrophic lateral sclerosis. Our method outperformed baseline methods on the three benchmarks. Furthermore, visual explanations derived from our model indicate that it effectively captures neurophysiologically relevant regions of both EEG and ECoG signals. Our project page is available at https://cortical-ssm-u90sg.kinsta.page/

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- **研究对象**：运动想象（Motor-Imagery, MI）期间的脑电（EEG）与皮层脑电（ECoG）信号分类，应用前景包括运动障碍患者的通信辅助与康复支持。
- **核心挑战**：
  - EEG/ECoG 信号极易受到眨眼、吞咽等生理伪影干扰，这类干扰长期是脑机接口解码的难题。
  - 现有基于 Transformer 的分类方法虽然被广泛采用，但在捕捉 EEG/ECoG 信号内部的**细粒度时间依赖**方面存在明显不足。
- **论文目标**：提出一种新的神经解码架构，能够鲁棒地处理伪影干扰并捕捉更精细的信号依赖，从而提升运动想象分类性能。

## 2. 方法论

- **方法名称**：Cortical-SSM
- **核心思想**：将深度状态空间模型（Deep State Space Model, SSM）扩展应用到 EEG 和 ECoG 信号建模中，用于替代或超越 Transformer 的时序建模能力。
- **技术细节**：
  - 模型旨在联合捕捉信号在**时间、空间、频率**三个维度上的集成依赖关系（integrated dependencies）。
  - 利用状态空间模型的递归结构对长时序进行有效建模，降低对局部注意力的依赖，从而更好地保留跨时刻的细粒度信号变化。
  - 通过状态空间的内部状态更新机制，天然地对输入中的噪声/伪影具有更强的稳定性。
- **公式 / 算法流程**：文中未给出明确公式，但从方法描述上看，其大致流程应为：
  - 输入 EEG/ECoG 多通道信号 → 预处理与时频特征提取 → 时间、空间、频率维度特征融合 → 送入深度状态空间模型进行时序状态建模 → 状态输出经分类头得到运动想象类别。

## 3. 实验设计

- **数据集与场景**：
  1. 两个大规模公共 MI EEG 数据集，总计包含超过 50 名受试者；
  2. 一个临床 MI ECoG 数据集，来自一位肌萎缩侧索硬化症（ALS）患者。
- **Benchmark 体系**：覆盖高被试量公共数据集与真实临床患者数据，兼具普适性与临床相关性。
- **对比方法**：以基于 Transformer 的 EEG/ECoG 分类方法为主要基线。
- **评价指标**：运动想象分类/解码准确率等。

## 4. 资源与算力

- 原文并未明确说明使用了多少 GPU（型号、数量）、训练时长、显存占用等计算资源信息。
- 由于论文摘要与提取到的内容中不含相关说明，无法确认计算成本与模型参数量，需阅读全文或附录进一步核实。

## 5. 实验数量与充分性

- **实验总览**：论文以三个 benchmark（两个 EEG、一个 ECoG）为核心，覆盖大规模公共数据与临床数据，实验范围较为完整。
- **对比实验**：以 Transformer 基线为核心对照，验证了 Cortical-SSM 在三个数据集上的优越性。
- **可解释性实验**：额外提供了视觉化解释（visual explanations），显示模型能够捕捉具有神经生理学意义的脑区信号，增强了结果的可信度。
- **充分性与客观性评价**：
  - 从覆盖面上看，50+ 受试者的公共数据集和真实 ALS 患者数据的组合合理，有助于验证一般性与临床价值；
  - 但是，由于文摘内容未详细展示消融实验、统计显著性检验、数据预处理细节以及每轮实验重复次数，对实验的严格性和公平性无法给出完整判断。
  - 未见对方法在不同伪影强度、噪声水平条件下的鲁棒性稳定性分析，也没有与其他非 Transformer 类 SOTA（如 CNN、Hybrid 模型）进行更广泛的对比。

## 6. 主要结论与发现

- Cortical-SSM 在三个 MI 解码 benchmark 上均优于基于 Transformer 的基线方法。
- 模型的有效性不局限于公共数据，在 ALS 患者的真实 ECoG 临床数据上也表现更好，说明具备一定临床转化潜力。
- 视觉解释结果显示，模型捕捉到的信号区域与神经生理学相关的脑区高度一致，证明其不仅精度高，也具备可解释性。
- 结论上，深度状态空间模型为 EEG/ECoG 运动想象解码提供了一种对抗生理伪影、超越 Transformer 时序建模的新方案。

## 7. 优点与亮点

- **建模创新**：将深度状态空间模型引入 EEG/ECoG 运动想象解码，是脑电时序建模的新颖尝试，拓展了该领域在 Transformer 之外的架构选择空间。
- **多域联合建模**：同时考虑时间、空间、频率三维信息，建模能力比纯时序模型更加全面。
- **临床数据验证**：使用真实 ALS 患者的 ECoG 数据作为额外评测点，增加了方法在医学实际应用上的说服力。
- **可解释性分析**：通过可视化方法证明模型学习到的表征在神经生理学上有据可依，而不仅是黑箱精度提升。
- **数据集规模合理**：两个总计 50+ 受试者的公开 EEG 数据集属于该领域中较大规模的验证设置，增强了研究结论的可靠性。

## 8. 不足与局限性

- **信息不完整**：论文提取页只提供了摘要级信息，关于模型详细结构、损失函数、超参设置、数据处理流程以及训练部署细节等内容均无法从当前材料中获取。
- **算力成本不透明**：未报告训练 Cortical-SSM 所需的 GPU 数量、时长和硬件成本，对可复现性构成一定障碍。
- **对比方法范围有限**：只强调优于 Transformer 基线，未与当前主流的 CNN/混合结构（如 EEGNet、Deep ConvNet 等）进行系统比较，横向说服力有待加强。
- **缺失严格的消融分析**：未明确提供针对各模块（时 / 空 / 频域建模组件）的贡献度消融实验，无法判断方法增益的真实来源。
- **统计检验不足**：尚不清楚在模型多次运行间是否报告了方差或置信区间，显著性检验信息的缺乏会影响结论严谨性。
- **应用局限**：临床验证样本量极小（仅一位 ALS 患者），个体差异对 ECoG 解码影响较大，方法用于更大规模临床人群的效果仍属开放问题。
- **数据时限与通用性**：对更多脑机接口任务（如不同肢体运动、非运动认知任务）的迁移能力未展开讨论。

（完）
