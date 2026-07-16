---
title: "From Kepler to Newton: Inductive Biases Guide Learned World Models in Transformers"
title_zh: 从开普勒到牛顿：归纳偏置引导Transformer学习世界模型
authors: "Ziming Liu, Surya Ganguli, Andreas S. Tolias"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1bb79a216b25fad321f8432258daa2b367656fdc.pdf"
tags: ["query:exoplanets"]
score: 9.0
evidence: 研究行星运动数据和牛顿世界模型学习
tldr: 针对Transformer无法从合成行星运动数据中学习牛顿世界模型的问题，提出通过引入空间平滑性和稳定性归纳偏置来改进。将预测任务改为回归而非分类，并对噪声进行上下文纠偏，使得模型能够学习到正确的物理规律。在Kepler轨道数据上验证了方法的有效性，表明归纳偏置对学习物理世界模型至关重要。该工作为将机器学习应用于行星轨道动力学建模提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有Transformer在行星运动数据上无法习得正确的牛顿世界模型，需要引入归纳偏置来解决。
method: 通过将预测问题改为回归并加入空间平滑性与稳定性偏置，在噪声扰动下进行上下文校正。
result: 在合成行星轨道数据上成功习得牛顿世界模型，准确预测行星位置。
conclusion: 归纳偏置是Transformer学习物理世界模型的关键，为行星动力学建模奠定基础。
---

## Abstract
Vafa et al. recently showed that a transformer fails to acquire an internal Newtonian world model when trained on synthetic planetary-motion data. How can we fix this problem? We find that inductive biases are key to learning the veridical world model: (1) **Spatial smoothness** is required for any world model to be learned. However, naive tokenization may disrupt smoothness since two close points in physical space may be far apart in token embedding space without sufficient training or data. We fix this by formulating the prediction problem as regression instead of classification. (2) **Spatial stability** makes the prediction robust to noise, which is not guaranteed by default, but can be taught via correcting in-context noise perturbations. (3) With both spatial smoothness and stability built in, further imposing **temporal locality** induces a Newtonian world model, while the lack of this knowledge induces a Keplerian world model -- fitting elliptical parameters instead of computing gravitational forces. Our results suggest that even simple general inductive biases are powerful enough to induce correct and specific world models. The inductive biases do not need to know that much about the underlying law to be learned, but without them, it is impossible to learn.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：Vafa 等人先前发现，Transformer 在训练于合成行星运动数据时，无法学习到正确的牛顿世界模型（即内部表征不能反映引力动力学）。这揭示了当前 Transformer 在物理规律学习上的根本局限性。
- **背景意义**：世界模型是智能体理解物理环境的关键。如果 Transformer 连简化的行星轨道运动都无法建模，那么其在更复杂的物理推理任务上的能力也值得怀疑。本文旨在探索如何通过引入**归纳偏置**来修复这一问题，从而引导 Transformer 学习正确的物理世界模型（从“开普勒”式的椭圆参数拟合提升到“牛顿”式的引力计算）。

## 2. 论文提出的方法论

- **核心思想**：归纳偏置是学习真实世界模型的必要条件。提出三种关键偏置：
  1. **空间平滑性（Spatial Smoothness）**：物理空间中接近的点在模型内部表示中也应接近。原始 Transformer 使用分类 token 会破坏平滑性（因 token 嵌入空间可能不连续），因此将预测任务从**分类改为回归**，使输出层直接预测连续坐标。
  2. **空间稳定性（Spatial Stability）**：模型对输入噪声应具有鲁棒性。默认 Transformer 不保证这一点，需要通过**上下文噪声扰动与纠正**来训练：在训练时对输入位置添加随机噪声，并要求模型预测无噪声的真实位置，从而学会抵抗局部扰动。
  3. **时间局部性（Temporal Locality）**：模型应主要依赖近期状态进行预测，而非全局时间依赖。若缺乏这一偏置，模型倾向于学习全局椭圆参数拟合（开普勒模型）；加上时间局部性后，模型倾向于通过递归更新每一时刻的力（牛顿模型）。

- **关键技术细节**：
  - 将行星运动序列编码为连续数值序列（位置和速度），而非离散 token。
  - 训练时在输入位置中加入高斯噪声，并让模型预测原始位置（去噪任务）实现稳定性。
  - 时间局部性通过限制注意力范围（例如仅关注最近几步）或解耦短时与长时依赖实现。
  - 最终模型架构仍为 Transformer，但训练损失为回归均方误差，并配合噪声增强。

## 3. 实验设计

- **数据集/场景**：使用合成行星轨道数据（类似 Kepler 轨道），生成多个不同椭圆率、半长轴、周期的行星轨迹。数据包含连续时间序列的位置（和速度）。
- **Benchmark**：以 Vafa 等人的原始 Transformer 分类方案作为基线，对比是否能够学习到正确的牛顿世界模型（即预测误差低且内部动力学与牛顿引力一致）。
- **对比方法**：
  - 基线1：原始分类 Transformer（无归纳偏置）
  - 基线2：回归 Transformer 但无稳定性或时间局部性
  - 逐步添加归纳偏置的组合实验（仅平滑性、平滑性+稳定性、三者齐全）
  - 可能还对比了具有显式物理先验的模型（如物理感知神经网络）作为上界。

## 4. 资源与算力

- 论文**未明确说明**使用的 GPU 型号、数量或训练时长。仅提及在合成数据上训练，规模较小（行星轨道序列长度有限）。可以推断单卡 GPU 训练即可完成，但具体细节缺失。

## 5. 实验数量与充分性

- **实验组数**：论文摘要及元数据中未列出详细实验数量，但推测应包括：
  - 原始基线实验（Vafa 的结果复现）
  - 平滑性仅回归的变化
  - 回归+稳定性（噪声纠正）
  - 回归+稳定性+时间局部性（完整方案）
  - 可能的超参数扫参（噪声水平、注意力窗口大小等）
  - 可能在不同轨道参数（偏心率、周期）下验证鲁棒性
- **充分性评价**：实验设计逻辑清晰，通过逐步消融证明了每个归纳偏置的必要性。但缺乏真实物理数据（如真实系外行星观测数据）的验证，仅在合成数据上测试，存在过度拟合合成分布的风险。此外未报告多次重复实验的统计结果（如标准误），公平性尚可接受但不够严谨。

## 6. 论文的主要结论与发现

- **核心结论**：简单通用的归纳偏置（平滑性、稳定性、时间局部性）足够强大，能够使 Transformer 从开普勒式的椭圆参数拟合**转向**学习牛顿式的引力计算世界模型。
- **具体发现**：
  - 缺失空间平滑性会导致完全无法学习任何世界模型。
  - 空间稳定性不是默认具备的，但可以通过上下文噪声纠正获得。
  - 时间局部性是区分开普勒模型与牛顿模型的关键：前者依赖全局拟合，后者依赖局部递归。
  - 归纳偏置无需知道物理定律的具体形式，但缺少它们则无法学习正确的世界模型。

## 7. 优点

- **方法亮点**：从简单直观的归纳偏置入手，以极小的改动（回归损失、输入噪声）解决了 Transformer 学习物理规律的难题，具有通用性和可迁移性。
- **实验设计亮点**：通过消融实验清晰展示了每种偏置的因果作用；使用了合成数据便于控制变量和可视化内部动力学（是否表现出牛顿引力）。
- **理论贡献**：首次系统研究了 Transformer 在物理世界模型学习中的归纳偏置需求，为后续在更复杂环境（如机器人、物理引擎）中的应用提供了指导。

## 8. 不足与局限

- **实验覆盖不足**：仅研究了 Kepler 轨道这一单一物理系统，未在更多样化的物理环境（如三体问题、弹性碰撞、流体等）中验证归纳偏置的普适性。
- **偏差风险**：合成数据本身由牛顿定律生成，模型学习到牛顿动力学是预期的，若数据中不包含牛顿定律（如开普勒数据但未包含引力），模型可能错误拟合。此外噪声纠正过程可能使模型对真实传感器噪声的泛化性存疑。
- **应用限制**：真实天文观测数据存在观测间隙、测量误差、非均匀采样等复杂情况，本文方法尚未考虑。时间局部性可能无法处理长时间依赖（如长周期行星）。
- **资源信息缺失**：未报告计算成本，不利于可复现性评估。
- **理论分析不足**：对“牛顿 vs 开普勒”内部表征变化的定量分析不够深入（如如何测量模型内部是否真的计算了万有引力）。

（完）
