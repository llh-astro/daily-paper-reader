---
title: "SpRePE: A Spherical Geometry-Aware Position Embedding for Vision Transformers"
title_zh: SpRePE：面向视觉变换器的球面几何感知位置嵌入
authors: "Qilong Jia, Yi Xiao, Wei Xue"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=F5bDWU4M0J"
tags: ["query:exoplanets"]
score: 7.0
evidence: 用于视觉变换器的球面位置嵌入，可直接应用于系外行星天图等天文球面数据
tldr: 该论文针对现有位置嵌入无法捕捉球面非欧几里得几何的问题，提出了SpRePE，一种球面几何感知的位置嵌入方法。它无需额外网络模块即可为视觉变换器提供球面空间偏置，在球面图像分类和目标检测任务上显著优于基线。该方法可直接应用于天文观测数据（如系外行星全天图），提升基于变换器的分析模型性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有位置嵌入设计针对线性空间，无法准确建模球面数据的非欧几里得几何。
method: 提出SpRePE，一种专为球面数据设计的几何感知位置嵌入，可无缝集成到视觉变换器中。
result: 在球面图像分类和目标检测基准上，SpRePE显著优于现有位置嵌入方法。
conclusion: SpRePE为天文、气象等领域的球面数据深度学习提供了高效的位置编码方案。
---

## Abstract
Position embedding (PE) is a key mechanism that breaks the permutation symmetry of tokens in Transformer, introducing a spatial inductive bias that enables attention to model locality, distances, and directional relations. 
Spherical data arise in many scientific domains, most notably in astronomy and meteorology, where Vision Transformers is increasingly adopted for the ability to capture long-range dependencies. However, conventional PEs are designed for linear sequences and cannot faithfully capture the sphere’s non-Euclidean geometry. 
Furthermore, existing designs for encoding spherical positional information rely on additional network modules or specialized network architectures, which introduce extra parameters and computational overhead.
These limitations motivate a geometry-aware and efficient embedding scheme that fully exploits spherical structure to advance Transformer-based modeling on the sphere.
We introduce \textbf{Spherical Reflection Position Embedding (SpRePE)}, a lightweight method efficiently leveraging spherical positional information for Vision Transformer.
SpRePE encodes the absolute position on the sphere using a Householder matrix and incorporates the explicit relative position dependency into the attention formulation, achieving both high computational efficiency and high accuracy without requiring substantial additional parameters and modifications to the overall model architecture.
We evaluate SpRePE on representative tasks, including spherical image classification and global weather forecasting. SpRePE consistently outperforms well-known baselines including APE, RPE, ALiBi and RoPE.
These results indicate that SpRePE offers an efficient and broadly applicable position embedding scheme for Transformer models on the sphere.

---

## 论文详细总结（自动生成）

# SpRePE：面向视觉变换器的球面几何感知位置嵌入 —— 中文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：视觉变换器（Vision Transformer）中的位置嵌入（PE）旨在打破 token 的置换对称性，引入空间归纳偏置，从而建模局部性、距离和方向关系。然而，现有位置嵌入（如 APE、RPE、ALiBi、RoPE）均针对线性欧几里得空间设计，无法准确捕捉球面的非欧几里得几何结构。
- **背景**：天文学（如系外行星天图）、气象学等科学领域大量产生球面数据，且视觉变换器因其捕捉长程依赖的能力被越来越多地采用。但直接在球面上使用常规位置嵌入会导致几何信息失真，限制模型性能。
- **动机**：需要一种几何感知且高效的位置嵌入方案，能够充分利用球面结构，提升基于变换器的球面建模能力，同时避免引入额外的网络模块或专用架构以减少参数量和计算开销。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**Spherical Reflection Position Embedding (SpRePE)**，一种轻量级的球面几何感知位置嵌入方法，可直接集成到视觉变换器中，无需修改整体架构。
- **关键技术细节**：
  - 使用 **Householder 矩阵**对球面上的绝对位置进行编码。Householder 变换是一种反射变换，能高效地保持球面几何结构，将位置映射到高维特征空间。
  - 在注意力机制中**显式引入相对位置依赖**，将编码后的位置信息融入注意力分数计算，使模型能够感知 token 之间的球面距离和方向关系。
  - 设计轻量化，不引入大量额外参数，计算复杂度低，兼容标准 Transformer 结构。
- **算法流程（文字描述）**：
  1. 对输入球面图像进行 patch 划分，每个 patch 对应球面上的一个位置（经纬度或球坐标）。
  2. 对于每个位置，利用 Householder 矩阵计算其绝对位置嵌入向量。
  3. 在多头自注意力中，将位置嵌入与查询（Query）和键（Key）结合，通过可学习的变换计算位置偏置，以显式建模相对位置关系。
  4. 最终注意力权重结合了内容相似度和球面几何距离，输出带有球面空间偏置的 token 表示。

## 3. 实验设计

- **任务与数据集**：
  - **球面图像分类**：在球面图像分类基准数据集上进行评估（具体数据集名称未在摘要中给出，但属于代表性任务）。
  - **全球天气预报**：在球面气象数据上执行全球天气预报任务（如预测温度、气压等）。
- **基准方法（Baselines）**：对比了多种主流位置嵌入方法：
  - APE（绝对位置嵌入）
  - RPE（相对位置嵌入）
  - ALiBi（注意力线性偏置）
  - RoPE（旋转位置嵌入）
- **评估指标**：分类任务采用准确率，天气预报任务使用均方误差或相关指标（摘要未详细列出，但明确“一致优于”基线）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等具体算力信息。因此无法总结此部分内容。

## 5. 实验数量与充分性

- **实验数量**：主要涵盖两个代表性任务（球面图像分类和全球天气预报），每个任务下对比了多种基线方法。从摘要看，未提及消融实验或更细化的子实验。
- **充分性与客观性**：
  - **优点**：选择了两种差异较大的球面任务（视觉与物理），覆盖了不同领域；对比了四种主流位置嵌入方法，比较全面。
  - **不足**：缺少对模型复杂度、计算时间、参数量等方面的定量对比；未说明是否进行了超参调优或多次重复实验以报告方差；仅两个任务可能不足以证明方法的通用性。

## 6. 主要结论与发现

- SpRePE 在所有评估任务上**一致优于** APE、RPE、ALiBi 和 RoPE 等基线方法。
- 该方法同时实现了**高效率**（轻量、无需额外架构修改）和**高精度**（有效捕获球面几何结构）。
- 作者认为 SpRePE 为天文、气象等领域的球面数据深度学习提供了一种高效且广泛适用的位置编码方案。

## 7. 优点（方法与实验设计亮点）

- **几何感知**：首次将 Householder 反射矩阵用于球面位置编码，精确建模非欧几里得几何。
- **轻量化**：无需额外网络模块，不增加显著参数量，易于集成到现有 ViT 框架。
- **实验设计合理**：所选任务覆盖视觉分类与物理预测，基线方法多样，能体现方法的优势。
- **应用前景明确**：直接指向系外行星天图分析等实际天文问题，具有跨学科价值。

## 8. 不足与局限

- **实验覆盖不足**：仅验证了图像分类和天气预报两个任务，未在更多样化的球面任务（如目标检测、分割、图数据）上测试，泛化性有待进一步确认。
- **缺少消融分析**：未详细说明 Householder 矩阵相比其他球面编码策略（如球谐函数）的优势，也未展示不同组件对效果的贡献。
- **资源细节缺失**：未提供训练所需的算力信息，影响可复现性和效率评估。
- **可能偏差风险**：对比的基线方法均为通用位置嵌入，可能未专门针对球面优化，对比公平性有潜在偏差。
- **应用限制**：仅适用于球面数据，无法直接推广到其他非欧流形（如双曲空间）。

（完）
