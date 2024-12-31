## 📊 Paper Metadata
- **Title:** Modeling Cellular Perturbations with the Sparse Additive Mechanism Shift Variational Autoencoder
- **Authors:** Michael Bereket, Theofanis Karaletsos
- **Publication:** 37th Conference on Neural Information Processing Systems (NeurIPS 2023) https://arxiv.org/abs/2311.02794 
- **Institution:** insitro (formerly affiliated)
- **Code:** https://github.com/insitro/sams-vae
- **Tags:** #VAE #CellularPerturbation #DeepLearning #Bioinformatics #SingleCell

## 🎯 Core Contributions
1. Introduction of SAMS-VAE, a novel generative model that combines sparse perturbation-specific latent effects with natural cell variation
2. Development of sophisticated inference strategies for improved model fitting
3. Introduction of evaluation framework based on average treatment effects for perturbation models
4. Demonstration of superior performance in generalization and biological pathway recovery

## 📋 Paper Structure

![Framework](../../../paper-figures/sams-vae-framework.png)

### 1. Introduction
- Background: Need for modeling cellular responses to diverse interventions in drug discovery
- Problem: Existing models lack effective combination of compositionality, disentanglement, and interpretability
- Innovation: SAMS-VAE combines these elements through sparse additive mechanism shifts

### 2. Methods/Results
- SAMS-VAE Architecture
  - Models latent state as sum of basal state and perturbation effects
  - Uses sparse binary masks for feature selection
  - Employs correlated variational families for inference
- Evaluation Framework
  - Marginal likelihood estimation
  - Average treatment effects with posterior predictive checks
- Experimental Validation
  - Performance on single-cell RNA sequencing datasets
  - Comparison with baseline models

### 3. Discussion
- SAMS-VAE outperforms baselines in generalization tasks
- Shows improved ability to recover known biological mechanisms
- Provides interpretable latent structures
- Enables better prediction of combination effects

## 🔬 Technical Details

### Algorithm Framework
1. Core Components:
   - Latent basal state embedding
   - Global perturbation embeddings
   - Binary masks for sparsity
   - Variational inference scheme

2. Key Innovations:
   - Additive composition of perturbation effects
   - Sparse mechanism shifts
   - Correlated variational families

## 📊 Evaluation

### Baseline Models
- CPA-VAE
- SVAE+
- Conditional VAE

### Evaluation Metrics
1. Test IWELBO (Importance Weighted ELBO)
2. ATE-Pearson correlation
3. Mask pathway prediction accuracy

### Datasets
1. Replogle-filtered dataset:
   - ~118,461 cells
   - 1,187 gene features
   - 722 unique CRISPR guides

2. Norman et al. CRISPRa dataset:
   - 111,255 cells
   - 5,000 gene features
   - 105 unique perturbations

## 💭 Critical Analysis

### Strengths
1. Strong theoretical foundation combining multiple valuable properties
2. Comprehensive evaluation framework
3. Superior performance on real biological datasets
4. Interpretable results aligning with known biology

### Limitations
1. Requires careful tuning of sparsity parameters
2. Computational complexity of inference
3. Limited to binary dosage scenarios

## 📌 Key Takeaways
1. SAMS-VAE effectively combines compositionality, disentanglement, and interpretability
2. Shows superior performance in both predictive and interpretative tasks
3. Provides a valuable tool for understanding cellular responses to perturbations
4. Advances the field of machine learning-driven scientific discovery

## 💡 Personal Notes

A. What Makes SAMS-VAE Special:

1. **Additive Embedding Structure:**
  - Imagine each cell's representation (zi) as: zi = ziᵇ + ziᵖ
  - ziᵇ: base state (natural cell variation)
  - ziᵖ: perturbation effects (changes from treatments)
  - Like having a "before" and "what changed" picture for each cell

2. **Sparse Mechanism Shifts:**
  - Uses a clever masking system (mt) that identifies which parts of the cell actually change
  - Instead of assuming every treatment affects everything, it pinpoints specific changes
  - Example: If you add Drug A, maybe only 20% of cell features actually change

3. **Smart Composition for Multiple Treatments:**
  - Can predict what happens when you combine treatments
  - Unlike simpler models that treat each combination as brand new
  - Example: If you know how Drug A and Drug B work separately, it can predict what happens when you use both

Key Embedding Components:

```
Treatment Effects = Σ di,t(et ⊙ mt)
where:
- et: embedding showing how a treatment changes cells
- mt: binary mask showing which parts change
- di,t: whether treatment t was applied
- ⊙: element-wise multiplication
```

Treatment Effects = Σ di,t(et ⊙ mt)
这个公式描述了"药物如何改变细胞"。

想象你在玩一个积木游戏：

1. **di,t**: 是否使用了某种药物
   - 就像开关一样：用了这个药物 = 1，没用 = 0
   - 比如：用了阿司匹林 = 1，没用 = 0

2. **et**: 药物的影响方式
   - 就像是药物的"说明书"
   - 告诉我们这个药物会如何改变细胞
   - 比如：阿司匹林可能会影响细胞的某些特定部分

3. **mt**: 影响的位置
   - 像是一个筛子或者模板
   - 1 = 这个位置会被影响
   - 0 = 这个位置不会被影响
   - 帮助我们精确知道药物影响了哪些地方

4. **⊙**: 点乘运算
   - 就像是把两个模板叠在一起
   - 只有两个都是1的地方才会留下影响

5. **Σ**: 所有效果的总和
   - 如果用了多种药物，把所有效果加起来

例子：
假设你在给植物浇水
- di,t：今天浇水了吗？（是/否）
- et：水会怎么影响植物（让叶子变绿，茎变长等）
- mt：植物的哪些部分会被水影响（根部吸收多，叶子少）
- 最后把所有护理措施（浇水、施肥等）的效果加起来

Real-World Example:
- Think of studying cancer treatments
- Traditional: Must test every possible drug combination
- SAMS-VAE: Learn from single drug responses, then predict combinations
- Saves time and resources in drug development

It creates interpretable, composable embeddings that:
1. Show which cell parts are affected (through masks)
2. Can predict combination effects
3. Separate natural cell variation from treatment effects
