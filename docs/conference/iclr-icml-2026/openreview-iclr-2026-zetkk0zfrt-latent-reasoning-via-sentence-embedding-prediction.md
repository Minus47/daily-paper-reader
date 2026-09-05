---
title: Latent Reasoning via Sentence Embedding Prediction
title_zh: 基于句子嵌入预测的隐式推理
authors: "Hyeonbin Hwang, Byeongguk Jeon, Seungone Kim, Jiyeon Kim, Hoyeon Chang, Sohee Yang, Seungpil Won, Youbin Ahn, Dohaeng Lee, Minjoon Seo"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ZETkK0zfrt"
tags: ["query:abstraction"]
score: 6.0
evidence: 在句与命题级嵌入层进行高层抽象推理，与类脑概念空间的模型侧抽象相应
tldr: 针对语言模型在词元序列上逐token生成、难以在高层抽象语义单元上推理的问题，本文将预训练词元级语言模型改造为在句子嵌入空间中自回归预测下一句连续表示。文章比较了基于自编码的语义嵌入等两种范式。实验表明该方法可利用模型已有表示进入抽象句子空间。该路线为构建面向概念与命题的类脑抽象概念表示提供了模型侧基础。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 人类推理基于句子、命题和概念等高层抽象，而语言模型只在词元序列上逐步生成，缺少抽象的中间语义单元。
method: 将预训练token级语言模型改造为预测下一句连续嵌入，在句子空间中自回归生成，并比较多种嵌入学习范式。
result: 验证了预训练模型可被提升到句子嵌入抽象空间中进行推理性预测。
conclusion: 为语言模型在抽象语义层面推理提供可行框架，有助于构造分层语义概念空间。
---

## Abstract
Autoregressive language models (LMs) generate one token at a time, yet human reasoning operates over higher-level abstractions - sentences, propositions, and concepts. This contrast raises a central question- Can LMs likewise learn to reason over structured semantic units rather than raw token sequences? In this work, we investigate whether pretrained LMs can be lifted into such abstract reasoning spaces by building on their learned representations. We present a framework that adapts a pretrained token-level LM to operate in sentence space by autoregressively predicting continuous embeddings of next sentences. We explore two embedding paradigms inspired by classical representation learning: 1) semantic embeddings, learned via autoencoding to preserve surface meaning; and 2) contextual embeddings, trained via next-sentence prediction to encode anticipatory structure. We evaluate both under two inference regimes: Discretized, which decodes each predicted embedding into text before re-encoding; and Continuous, which reasons entirely in embedding space for improved efficiency. Across four domains - mathematics, logic, commonsense, and planning - contextual embeddings under continuous inference show competitive performance with Chain-of-Thought (CoT) while reducing inference-time FLOPs on average by half. We also present early signs of scalability and modular adaptation. Finally, to visualize latent trajectories, we introduce SentenceLens, a diagnostic tool that decodes intermediate model states into interpretable sentences. Together, our results indicate that pretrained LMs can effectively transition to abstract, structured reasoning within latent embedding spaces.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景与动机**：自回归语言模型逐 token 生成文本，而人类推理则基于句子、命题和概念等更高层的抽象语义单元。这一差异引出了论文的核心问题：语言模型能否同样学会在结构化语义单元（如句子）的层面上进行推理，而非仅仅在词元序列上进行预测？作者希望探索一种途径，将预训练的 token 级语言模型"提升"到更具抽象性的语义空间中进行推理——这不仅触及大模型推理范式的根本变革，也响应了类脑计算中对概念空间与抽象表示的研究方向。
- **整体含义**：论文并不追求对预训练模型的彻底重构，而是利用模型已有的表示能力，使其在连续的句子嵌入空间中自回归推理。这项工作为在大模型内部构建概念/命题层面的抽象表征提供了可行基础，具有较强的前瞻性和范式意义。

## 2. 论文提出的方法论

- **核心思想**：将预训练的 token 级自回归语言模型改造为在"句子空间"中运作的推理模型。其基本做法不是直接生成下一个 token，而是预测下一个句子的连续向量表示（sentence embedding），即把原本的离散 token 序列生成过程转变成连续嵌入的自回归过程。
- **两种嵌入学习范式**：
  - 语义嵌入（Semantic Embeddings）：通过自编码（autoencoding）学习，以保留句子的表层语义信息。
  - 上下文嵌入（Contextual Embeddings）：通过下一句预测（next-sentence prediction）训练，以编码前文的结构期望和预判信息。
- **两种推理方案**：
  - 离散化推理（Discretized）：每预测一个句子嵌入后，先通过解码器将其还原为自然语言文本，再将文本重新编码为句子表示，送入下一步推理——本质上是"嵌入-文本-嵌入"的交替过程。
  - 连续推理（Continuous）：整个推理过程完全发生在嵌入空间内。预测的句子向量被直接传递给下一步，不做解码和重编码，从而降低推理时的计算开销。
- **算法流程说明**：模型接收前文中已被嵌入的句子序列作为条件，在句子嵌入空间中逐步预测下一个句子的连续表示；根据不同范式选择由自编码器或下一句预测模型生成表示，再经过下游解码转换回所需输出。作者强调关键改进在于：预测在"句子层级"上进行，模型不再需要对句子内部的词元组合进行逐一细致规划，而可以在更高语义抽象粒度上完成决策。

## 3. 实验设计

- **Benchmark 覆盖领域**：论文在四个不同推理领域进行实验：
  - 数学推理（mathematics）
  - 逻辑推理（logic）
  - 常识推理（commonsense reasoning）
  - 规划任务（planning）
- **对比方法**：主要基线是 **Chain-of-Thought (CoT)**，同时也比较了不同的嵌入范式（semantic vs contextual）和不同的推理方式（discretized vs continuous）之间的组合效果。
- **附加评估工具**：论文还介绍了 **SentenceLens** 诊断工具，用于将模型的中间状态解码为可解释的句子，以可视化模型在潜在句子空间中的轨迹变化，帮助理解"隐式推理"路径的质量与走向。
- **扩展性观察**：初步探索了该方法的可扩展性与模块化适配信号。

## 4. 资源与算力

- 论文中**未明确说明**具体使用的 GPU 型号、数量、训练时长、参数量级等资源细节。若需评估该方法的实际训练成本与能耗，仍然缺乏明确的数值依据。这是论文在复现和工程评估层面留有的信息缺口。

## 5. 实验数量与充分性

- **实验数量**：笔者所获得的论文文本主要来自摘要，并未公开具体的表格数据、详细的四领域数值对比、不同设置间的消融结果、以及每个实验重复次数等细节。从摘要来看，实验至少覆盖了四领域主实验、两种嵌入范式×两种推理方案的条件组合、以及可扩展性初步观察等。
- **充分性与客观性评估**：
  - 优点在于领域覆盖面较宽（数学、逻辑、常识和规划），避免只在单一任务上证明效果，这为结论提供了较广的外部效度证据。
  - 需要谨慎之处是：论文没有提供多个运行的方差、与更多方法（如自回归语言模型直接生成 CoT、树搜索类方法等）的全面对比、以及在不同模型规模下的完整扩展曲线。此外，该论文是 ICLR-2026 投稿中被拒的版本，评委可能指出了某些实验比较或设计的不充分之处。因此结论虽具启发性，但全面性和说服力尚非充分，需要进一步通过更细粒度的消融、鲁棒性验证和更大规模基准测试来支撑。

## 6. 论文的主要结论与发现

- **有效性**：经过适配后，预训练语言模型可以在句子嵌入的抽象空间中进行"类推理"操作，这种能力依托于预训练模型已有的内部表示。
- **上下文嵌入 + 连续推理是最佳组合** ：在数学、逻辑、常识、规划四个领域中，该组合表现出与 CoT 相当的性能，同时平均减少约一半的推理期 FLOPs。
- **可扩展性萌芽**：出现了一些可扩展性和模块化适配的早期正向信号，即这种嵌入级推理方法可能随模型或训练数据的扩大而收益。
- **工具产出**：SentenceLens 可有效把隐式推理过程投射为可读的句子，为可视化和分析潜在语义轨迹提供了新工具；隐式表示中出现的连续语义过渡可由句子解码进行一定程度的解释。
- **总体判断**：这一结果表明，大模型完全可以在潜在连续空间中实现更高层级的推理步骤，而非必定逐词生成全文——为"抽象推理"与"类脑概念空间"研究提供了模型侧的依据。

## 7. 优点

- **范式价值突出**：从 token 级生成向句子嵌入级推理的转变在思想层面很有启发性，指向了降低推理成本与匹配人类抽象认知结构的新维度，突破一般只围绕自回归词元展开的研究惯性。
- **方法兼具创新性与实用空间**：提供 discretized 与 continuous 两种推理模式，兼顾可解释性（discretized）与高效性（continuous），展现了很强的实用灵活性。
- **嵌入学习方案源自经典表征学习的两种思想路线**，有助于厘清不同嵌入目标对抽象推理的影响。
- **多领域验证提升了结论可信度**，数学、逻辑、常识、规划带来的任务多样性使得效率与性能的结论不是单一领域中的偶然现象。
- **配套诊断工具**（SentenceLens）增强了隐式推理过程的可解释性，而可解释性恰恰是"隐式推理"通路中容易被忽视的难点。

## 8. 不足与局限

- **信息不完整，可复现性存疑**：论文内容摘要未给出具体超参数、各任务的数值分数、计算资源、训练数据规模等关键细节；这使得第三方难以快速重现已获得的性能或开销结论。
- **潜在偏差风险**：摘要中仅报告了"与 CoT 竞争性相当并减半 FLOPs"，并未展示相对于 CoT 的失败案例、在较长推理链或多个句子组合中的稳定性，也未进行误差分析，因此整体偏差或系统失败模式尚不清晰。
- **嵌入式推理的可控性问题**：连续式推理天然缺乏文本形式的中间步骤，当推理出错时难以定位是由语义嵌入质量不足、句子嵌入预测误差、还是更高层规划错误导致的；SentenceLens 仅作为诊断工具出现，尚未作为一种可调控的推理机制。
- **应用限制**：该方法目前更多集中在特定推理任务的初步验证上，与从大规模语料端到端训练嵌入式推理模型之间仍有关键跨度；尚未见到复杂指令跟随、长程多跳推理、开放域问答或真实场景决策上的评测证据。
- **对模型规模与泛化能力的讨论不足**：虽然提到"早期可扩展性信号"，但没有呈现强模型/弱模型之间的性能差异趋势，无法判断该路线是否依赖模型能力达到某个门槛。
- **Benchmark 与不公平对比风险**：摘要中没有说明 CoT 是使用同量级推理预算（如 token 约束）进行对比的，也未提供在同一 FLOPs 约束下各种方法的匹配比较；因此在"平均减半 FLOPs而性能相当"的宣称上，其对照公平性需要读者打一定折扣看待。

（完）
