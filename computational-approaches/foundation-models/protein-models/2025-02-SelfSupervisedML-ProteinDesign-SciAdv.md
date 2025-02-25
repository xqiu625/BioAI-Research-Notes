## Self-supervised machine learning methods for protein design improve sampling but not the identification of high-fitness variants

## 📊 Paper Metadata
- **Publication**: Science Advances  
- **Paper Link**: [https://www.science.org/doi/10.1126/sciadv.adr7338](https://www.science.org/doi/10.1126/sciadv.adr7338)  
- **Code/Data**: [https://github.com/meilerlab/probabilities_design](https://github.com/meilerlab/probabilities_design)  

---

## **1. 研究背景**
- 机器学习正在变革蛋白质设计领域，数据驱动的方法在实验成功率上已经超越了传统基于物理的建模方法（如 Rosetta）。
- 但目前这些 ML 方法通常仅以案例研究的形式出现，缺乏标准化和系统化的比较，导致难以评估其真正的效果。

## **2. 研究目标**
- 本研究开发了一种 **标准化工具**，用于在 Rosetta 框架内比较不同 ML 方法预测氨基酸概率的能力。
- 评估这些 ML 方法在 **蛋白质适应性（fitness）预测和设计中的表现**，特别是它们在 **采样突变（sampling mutations）和打分（scoring mutations）** 方面的能力。

## **3. 主要发现**
- **ML 方法在突变采样方面表现出色**，能够有效减少有害突变，优化序列空间。
- **但在评分和筛选高适应性突变方面没有明显优势**，尤其是在零样本（zero-shot）预测的情况下，ML 方法的评分效果不一定优于传统的 Rosetta 方法。
- 研究发现，在许多情况下，**使用 Rosetta 评分仍然可以获得与 ML 评分相当的结果**，甚至在某些任务上表现更好。

## **4. 研究方法**
- **数据集**：本研究选取了四个不同的蛋白质适应性数据集，包括 **GB1、avGFP、trastuzumab（赫赛汀）和 emibetuzumab**，涵盖了蛋白质结合亲和力、荧光活性等不同设计目标。
- **评估方法**：
  - 16 种不同的 ML 和传统方法被用于评估采样和评分能力。
  - 采用 **oracle 预测模型**（基于已知数据训练的回归或判别模型）来评估突变序列的适应性。
  - 比较了 **ProteinMPNN、ESM-2、MIF-ST** 等 ML 方法的表现，以及 Rosetta 评分方法（如 total score、dG separated、shape complementarity）。
  - 通过 **Spearman 相关性分析** 评估不同评分指标与实验适应性数据的匹配程度。

## **5. 实验结果**
- **ML 采样方法可以显著提高序列的可行性**，降低有害突变的比例。
- **在识别高适应性变体时，ML 方法的得分表现不如预期**，没有比 Rosetta 显著更好，尤其是在无下游微调（zero-shot）的情况下。
- **AF2 评分在某些情况下比 ML 评分更有用**，特别是在预测蛋白结构稳定性方面。
- **不同方法的排名能力有限**，实验中几乎没有方法能够 consistently 选出最优变体。

## **6. 研究意义**
- 该研究表明，**ML 方法可以提高蛋白质设计的突变采样效率，但目前还无法完全取代传统物理建模方法**。
- **混合方法（结合 ML 和生物物理方法）可能是未来的发展方向**，如在 **ML 采样的基础上使用更精确的评分方法进行筛选**。

---

## **结论与启示**
- **机器学习在蛋白质设计中的作用是 “补充” 而非完全替代 传统方法**。
- **目前的 ML 方法 可以优化采样，但 在高效筛选最佳序列方面仍有不足**。
- **未来的改进方向 可能包括**：
  1. **结合 ML 和物理模型**，优化评分功能；
  2. **提高 ML 方法的泛化能力**，使其在不同蛋白质和适应性任务上都能稳定工作；
  3. **发展更强的监督或自监督模型**，通过更大的数据集进行训练，提高预测精度。

