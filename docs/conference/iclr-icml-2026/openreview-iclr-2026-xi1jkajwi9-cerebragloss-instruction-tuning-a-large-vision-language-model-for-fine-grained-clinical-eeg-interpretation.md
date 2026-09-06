---
title: "CerebraGloss: Instruction-Tuning a Large Vision-Language Model for Fine-Grained Clinical EEG Interpretation"
title_zh: CerebraGloss：指令微调大型视觉语言模型用于细粒度临床脑电图解读
authors: "Wei Gu, Luo Tianming, Qiran Zhang, Mohan Ye, Xiao Shen, Wenxin Chen, Yunhuan Li, Yichen Zhang, Jing Hong, Bao-liang Lu, Wei-Long Zheng"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Xi1jkajWi9"
tags: ["query:eeg-align"]
score: 7.0
evidence: 将脑电可视化与专家文本配成指令数据并微调多模态大模型，属于脑电特征与文本表征的对齐研究
tldr: 临床脑电图判读费时且主观，现有模型多局限于窄分类任务，缺乏结合脑电图像与专家级标注的数据。作者提出基于YOLO波形检测器的自动数据生成流水线，构建大规模EEG-文本指令数据，并以此指令微调大型视觉语言模型CerebraGloss，实现对临床脑电的细粒度解读。该路线为脑电与语言对齐提供了可扩展的数据构造方式，也为脑电异常判读带来通用模型基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 临床脑电解读费时主观且缺乏大规模EEG-文本配对数据，阻碍大模型用于整体判读。
method: 用自研YOLO波形检测器自动生成EEG-文本指令语料，对大型视觉语言模型CerebraGloss做指令微调。
result: 该方法有望促进对临床脑电图的细粒度、整体化解释，并缓解专家标注稀缺问题。
conclusion: 自动数据生成与视觉语言指令微调是训练临床脑电解读模型的可扩展有效路线。
---

## Abstract
Interpreting clinical electroencephalography (EEG) is a laborious, subjective process, and existing computational models are limited to narrow classification tasks rather than holistic interpretation. A key bottleneck for applying powerful Large Vision-Language Models (LVLMs) to this domain is the scarcity of datasets pairing EEG visualizations with fine-grained, expert-level annotations. We address this by introducing CerebraGloss, an instruction-tuned LVLM for nuanced EEG interpretation. We first introduce a novel, automated data generation pipeline, featuring a bespoke YOLO-based waveform detector, to programmatically create a large-scale corpus of EEG-text instruction data. Using this data, we develop CerebraGloss, the first model of its kind capable of unified, generative analysis—performing tasks from detailed waveform description to multi-turn, context-aware dialogue. To evaluate this new capability, we construct and release CerebraGloss-Bench, a comprehensive benchmark for open-ended EEG interpretation. CerebraGloss demonstrates strong performance, surpassing leading LVLMs, including proprietary models like GPT-5, on this benchmark and achieving a new state-of-the-art on the TUSZ seizure detection task. Models, benchmark and tools are available at https://github.com/iewug/CerebraGloss.

---

## 论文详细总结（自动生成）

# CerebraGloss 论文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：临床脑电图（EEG）判读是一项耗时且主观性较强的工作。现有计算模型大多局限于狭窄的分类任务（如癫痫发作检测），缺乏对脑电图的整体化、细粒度解读能力。
- **关键瓶颈**：将大型视觉语言模型（LVLMs）应用于该领域的主要障碍在于，缺少将 EEG 可视化图像与专家级细粒度标注配对的大规模数据集。
- **整体含义**：论文提出 CerebraGloss，一种经过指令微调的大型视觉语言模型，用于对临床 EEG 进行统一、生成式的细粒度解读，覆盖从波形描述到多轮上下文对话等多种任务，并配套构建了开放评测基准，旨在推动 EEG 自动判读从窄分类走向通用、细粒度分析。

## 2. 论文提出的方法论

- **核心思想**：通过自动化数据生成管线，构造大规模 EEG-文本指令数据，再利用该数据对大型视觉语言模型进行指令微调，使模型具备细粒度 EEG 解读能力。
- **关键技术细节**：
  - 采用一种自主设计的 **YOLO 基波形检测器**，用于从 EEG 图像中自动识别并定位关键波形特征。
  - 基于检测结果，编程式地生成 “EEG 图像 + 专家级文本描述/指令” 的配对语料，实现大规模数据自动化构建。
  - 对基础 LVLM 进行 **指令微调（instruction tuning）**，发展出 CerebraGloss，使其能完成多种任务：细粒度波形描述、整体判读、多轮对话等。
- **公式/算法流程（文字描述）**：
  1. 输入原始临床 EEG 标准化可视化图像；
  2. 使用 YOLO 波形检测器识别图像中的独立波形事件（如尖波、慢波等）及其时空属性；
  3. 将检测结果转化成结构化的文本描述，并与专家规则/模板或已有标注结合，形成指令-响应对；
  4. 构建大规模 EEG-文本指令数据集；
  5. 在该数据集上对 LVLM 进行监督式指令微调，得到 CerebraGloss；
  6. 推理时输入 EEG 图像与用户指令，模型生成细粒度的文本解读。

## 3. 实验设计

- **数据集/场景**：
  - 自建的大规模 EEG-文本指令语料（通过自动化流水线生成）。
  - **CerebraGloss-Bench**：论文构建并开放的细粒度开放式 EEG 解读综合基准，用于评测整体化判读能力。
  - **TUSZ（Temple University Seizure Corpus）** 癫痫发作检测任务，用于验证经典任务上的性能。
- **基准与评测**：CerebraGloss-Bench 覆盖开放式 EEG 解读任务，包括波形描述和多轮上下文对话等。
- **对比方法**：
  - 多种领先的大型视觉语言模型；
  - 专有模型，如 **GPT-5**；
  - 以及已有癫痫检测相关模型（在 TUSZ 任务上对比）。
- **评测指标**：文中未给出具体指标名称，但表明在 CerebraGloss-Bench 上超过上述对比模型，并在 TUSZ 任务上达到新 SOTA。

## 4. 资源与算力

- 论文当前提供的内容中 **未明确说明** 所使用的 GPU 型号、数量、训练时长或总体算力规模。
- 仅提示模型、基准与工具将在 GitHub 上公开（https://github.com/iewug/CerebraGloss），但未列出训练所需资源细节。

## 5. 实验数量与充分性

- **实验组数**：文中明确提到的实验主要包括：
  - 在 CerebraGloss-Bench 上对 CerebraGloss 与多个 LVLM（包括 GPT-5）的对比评测；
  - 在 TUSZ 癫痫发作检测任务上的评测，报告了 SOTA 结果。
- **充分性评估**：
  - 从摘要看，实验覆盖了“生成式开放式解读”和“经典判别式检测”两类场景，能初步验证模型通用性与专用性。
  - 但描述较概括，未报告具体实验组数、消融实验（如对自动数据生成管线有效性的消融、指令数据规模影响、YOLO 检测器准确率等）、统计显著性、跨中心验证或不同 EEG 设备泛化等，因此**实验充分性信息不足**，难以全面评估其客观性与公平性。
  - 对比协议、评测指标、数据划分等细节缺失，需阅读全文才能判断是否公平（例如是否确保 GPT-5 的提示方式最优、是否避免数据泄漏等）。

## 6. 论文的主要结论与发现

- CerebraGloss 是首个能够进行**统一生成式细粒度 EEG 分析**的指令微调 LVLM，涵盖波形级描述与多轮上下文感知对话。
- 该模型在 CerebraGloss-Bench 上**超越了众多领先 LVLM，包括专有模型 GPT-5**。
- 在 **TUSZ 癫痫发作检测任务上达到新的 SOTA**，说明生成式预训练/微调得到的能力可以迁移到传统判别式任务。
- 整体证明：**“自动数据生成 + 视觉语言指令微调”** 是训练临床 EEG 解读模型的可扩展且有效的路线，有望缓解专家标注稀缺问题。

## 7. 优点

- **任务范式创新**：将 EEG 判读从窄分类提升为细粒度、甚至多轮对话的生成式理解，更贴近临床实际。
- **数据瓶颈突破**：提出基于 YOLO 波形检测器的自动化数据生成流水线，可大规模构造 EEG-文本配对指令数据，减少人工专家标注依赖。
- **开放式基准贡献**：构建并发布 CerebraGloss-Bench，为后续开放式 EEG 解读研究提供评测基础。
- **模型能力广**：统一支持波形描述、整体判读、多轮对话，兼具专用检测性能（TUSZ SOTA），体现通用性与专用性兼顾。
- **开源开放**：模型、基准与工具全部公开，利于复现与后续研究。

## 8. 不足与局限

- **算力与资源细节缺失**：未说明训练所需 GPU 规模与时长，影响复现与可扩展性评估。
- **实验细节不透明**：具体任务数、消融研究、与 GPT-5 等模型对比时的评测设置、指标选择、人工评估方式等都未在摘要中呈现，可能影响结论的客观性判断。
- **潜在的偏差风险**：
  - 自动生成的指令数据可能继承 YOLO 检测器自身的检测误差（假阳性/假阴性），导致文本描述与真实波形之间不完全对齐；
  - 模板/规则生成文本可能缺乏自然语言多样性与专家级语义深度；
  - 单一公开数据集（TUSZ）验证可能不足以证明模型在不同采集设备、不同病理类型和不同患者群体上的泛化性。
- **临床落地限制**：模型输出为生成式文本，仍存在幻觉风险，需经过严格临床验证才能用于医疗环境；伦理责任、可解释性和对罕见波形的处理能力尚待考察。
- **相比全文的信息覆盖局限**：本总结仅基于论文摘要与元数据，很多细节（方法细节、多组实验、消融、人工评价等）未能获取，未来应结合全文进行更深入评估。

（完）
