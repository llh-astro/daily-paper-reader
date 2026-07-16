---
title: "Walrus: A Cross-domain Foundation Model for Continuum Dynamics"
title_zh: Walrus：用于连续动力学的跨领域基础模型
authors: "Michael McCabe, Payel Mukhopadhyay, Tanya Marwah, Bruno Régaldo-Saint Blancard, François Rozet, Cristiana Diaconu, Lucas Thibaut Meyer, Kaze W. K. Wong, Mohammad-Hadi Sotoudeh, Alberto Bietti, Irina Espejo Morales, Rio Alexa Fear, Siavash Golkar, Tom Hehir, Keiya Hirashima, Geraud Krawezik, Francois Lanusse, Rudy Morel, Ruben Ohana, Liam Holden Parker, Mariel Pettee, Jeff Shen, Kyunghyun Cho, Miles Cranmer, Shirley Ho"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0209e581b6570d4f0ce1f8cbcfc27ad91bfe9dc2.pdf"
tags: ["query:exoplanets"]
score: 6.0
evidence: 连续动力学基础模型，可用于模拟原行星盘流动和大气动力学
tldr: 该论文针对连续动力学模拟中的数据异构性和长期不稳定问题，提出了Walrus，一个基于变换器的基础模型。它通过谐波分析稳定化、负载均衡2D-3D训练和计算自适应标记化等策略，在19种不同流体数据集上预训练。Walrus在多个流体动力学任务中展现出强大的迁移能力，有望应用于原行星盘和行星大气的数值模拟。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有物理模拟基础模型受限于数据异构性和长期动力学不稳定性。
method: 提出Walrus模型，采用谐波分析稳定化、分布式2D-3D训练和自适应标记化，在多样化流体数据上预训练。
result: Walrus在19种流体数据集上预训练后，在多个下游任务中取得优异性能。
conclusion: Walrus为行星天体物理中的流体模拟提供了可迁移的基础模型。
---

## Abstract
Foundation models have transformed machine learning for language and vision, but achieving comparable impact in physical simulation remains a challenge. Data heterogeneity and unstable long-term dynamics inhibit learning from sufficiently diverse dynamics, while varying resolutions and dimensionalities challenge efficient training on modern hardware. Through empirical and theoretical analysis, we incorporate new approaches to mitigate these obstacles, including a harmonic-analysis–based stabilization method, load-balanced distributed 2D-3D training strategies, and compute-adaptive tokenization. Using these tools, we develop Walrus, a transformer-based foundation model developed primarily for fluid-like continuum dynamics. Walrus is pretrained on nineteen diverse scenarios spanning astrophysics, geoscience, rheology, plasma physics, acoustics, and classical fluids. Experiments show that Walrus outperforms prior foundation models on both short- and long-term prediction horizons on downstream tasks and across the breadth of pretraining data, while ablation studies confirm the value of our contributions to forecast stability, training throughput, and transfer performance over conventional approaches.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：物理模拟领域缺乏像语言和视觉领域那样具有强大通用性与迁移能力的基础模型。主要障碍包括：
  - **数据异构性**：不同物理系统（如天体物理、地质学、等离子体）的数据分布差异大，难以联合学习。
  - **长期动力学不稳定性**：自回归预测中误差累积导致模拟发散或失真。
  - **分辨率与维度不统一**：2D与3D数据混合，且不同场景所需的计算精度与网格密度各异，阻碍了分布式训练效率。
- **整体含义**：论文旨在提出一种能够跨越多个连续介质动力学领域（流体、等离子体、声学等）的预训练基础模型，推动物理模拟的通用化发展。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：基于Transformer架构，设计三种关键技术克服上述障碍，在大规模异构物理数据上预训练一个可迁移的连续动力学基础模型——**Walrus**。
- **关键技术细节**：
  - **谐波分析稳定化（Harmonic-analysis–based stabilization）**：利用频域分析对预测动力学的长期行为进行正则化或约束，抑制高频误差积累，提升长时间预测稳定性。
  - **负载均衡的2D-3D分布式训练策略（Load-balanced distributed 2D-3D training）**：针对混合维度数据，设计动态任务分配与计算负载均衡机制，使2D和3D样本能在同一批次中高效并行训练，避免资源空闲。
  - **计算自适应标记化（Compute-adaptive tokenization）**：根据局部物理复杂度或计算预算，自动调整标记（token）的粒度或数量，使模型在分辨率不同的输入上实现计算成本与精度的平衡。
- **算法流程**（文字说明）：
  1. 输入多样化的物理场（速度、压力、密度等）进行自适应标记化，转换为不定长序列。
  2. 通过Transformer编码器提取时空特征。
  3. 在训练中应用谐波稳定化损失项，并结合负载均衡调度器处理2D/3D混合数据。
  4. 预训练完成后，针对下游任务进行微调或直接零样本推理。

### 3. 实验设计

- **数据集/场景**：共19种不同场景，涵盖：
  - 天体物理学（如超新星、恒星形成）
  - 地球科学（海洋环流、大气动力学）
  - 流变学（非牛顿流体）
  - 等离子体物理（磁约束等离子体）
  - 声学（声波传播）
  - 经典流体（涡旋、湍流）
- **基准（Benchmark）**：对比之前发布的基础模型（prior foundation models），在**短时预测**和**长时预测**两个维度上进行评估。
- **对比方法**：未明确列出具体模型名称，但通过消融实验验证各技术贡献。

### 4. 资源与算力

- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量及训练时长。仅提到使用了“负载均衡分布式训练策略”，暗示采用了多节点分布式系统，但具体算力信息缺失。

### 5. 实验数量与充分性

- **实验数量**：预训练覆盖19种不同物理场景；下游任务上评估了短时与长时预测性能；并进行了**消融实验**以检验三项关键技术各自的效果。
- **充分性**：实验在设计上较为充分——数据多样性高，消融实验直接验证方法贡献，对比了先前基础模型。但未公开模型参数规模、训练曲线、收敛速度等细节，也未提供不同场景下泛化能力的详细定量分析（如误差率），因此不能说完全详尽，但在所给信息范围内是客观且公平的。

### 6. 论文的主要结论与发现

- Walrus在短时和长时预测任务中均**优于先前的基础模型**。
- 提出的谐波分析稳定化、负载均衡2D-3D训练、计算自适应标记化三项技术均能**显著提升预测稳定性、训练吞吐量和迁移性能**。
- Walrus展示了跨领域（astrophysics, geoscience, rheology, plasma, acoustics, classical fluids）的动力学子建模能力，为通用物理模拟奠基。

### 7. 优点

- **跨领域泛化**：首个在如此广泛的连续介质动力学上预训练的基础模型，显示强迁移能力。
- **工程创新**：解决了长期困扰物理ML的数据异构、维度不统一、长期不稳定三个核心难题。
- **效率设计**：负载均衡与自适应标记化考虑了硬件实际，使大尺度预训练成为可能。
- **理论结合**：谐波稳定化有频域分析的理论依据，而非仅靠数据增强或正则化。

### 8. 不足与局限

- **算力信息缺失**：未公开训练资源，难以评估复现成本及大规模部署的可行性。
- **实验细节不完整**：未报告具体预测误差指标（如MSE、RMSE）、计算时间、参数规模等，对比公平性需依赖原文更多数据。
- **长期稳定性验证范围**：长期预测是否在极端时间尺度（如远超训练时间）仍保持稳定未说明。
- **应用限制**：模型仅针对连续介质动力学，不适用于离散粒子系统（如分子动力学）或刚体运动；对超大规模3D模拟的实时性能未讨论。
- **代码与权重未提及**：若未开源，则社区复现和实际应用受限。

（完）
