## 📊 Paper Metadata
- **Title**: Nucleotide Transformer: building and evaluating robust foundation models for human genomics
- **Authors**: Hugo Dalla-Torre, Liam Gonzalez, Javier Mendoza-Revilla, Nicolas Lopez Carranza, Adam Henryk Grzywaczewski, Francesco Oteri, Christian Dallago, Evan Trop, Bernardo P. de Almeida, Hassan Sirelkhatim, Guillaume Richard, Marcin Skwark, Karim Beguir, Marie Lopez, Thomas Pierrot
- **Publication**: Nature Methods
- **Institution**: InstaDeep, NVIDIA, Technical University of Munich
- **Paper Link**: https://doi.org/10.1038/s41592-024-02523-z
- **Code/Data**: https://github.com/instadeepai/nucleotide-transformer
- **Date Read**: 2025-02-18

## 🔄 Key Scientific Insights

### 1. Conceptual Innovation
- Development of transformer-based foundation models (Nucleotide Transformer) pre-trained on DNA sequences
- Models can learn context-specific representations of nucleotide sequences without task-specific supervision
```
这些模型能够在没有针对特定任务进行专门训练（没有任务特定监督）的情况下，学习到核苷酸序列的上下文特定表示。

详细解释：

1. **无需任务特定监督**：传统的深度学习方法通常需要大量带标签的数据来训练模型完成特定任务（如预测启动子、增强子或剪接位点）。这种方法被称为"有监督学习"，因为模型是在明确知道"正确答案"的情况下学习的。

2. **上下文特定表示**：这些模型能够理解DNA序列中核苷酸的上下文含义。例如，同一个核苷酸序列在不同基因环境中可能有不同的功能，模型可以捕捉到这种差异。这些表示（也称为嵌入或embeddings）编码了序列的生物学特性和功能信息。

3. **自监督学习方法**：Nucleotide Transformer使用了类似于BERT的掩码语言建模方法，通过预测被遮盖的核苷酸来学习DNA的通用表示。这种预训练方式不需要任何特定功能的标注数据。

4. **迁移学习潜力**：这种通用表示可以被"迁移"到各种下游任务，如启动子预测、增强子预测、剪接位点识别等，只需少量的微调或简单的探测。

简单类比：这就像一个人通过大量阅读各类文章学习了语言的一般规律，然后可以将这种理解应用到写作、翻译、文法分析等特定任务上，而不需要为每项任务从头学习语言。
```
  
- Models acquire knowledge about genomic elements and can predict variant effects
- Demonstrates that training on diverse species outperforms training on only human sequences

### 2. Methodological Framework
- Collection of models with varying parameters (50M to 2.5B) trained on different datasets:
  1. Human reference genome (500M parameters)
  2. 1000 Genomes Project - 3,202 diverse human genomes (500M and 2.5B parameters)
  3. Multispecies dataset - 850 diverse species (2.5B parameters)
- Two evaluation strategies:
  1. Probing: using embeddings as features for downstream models
  2. Parameter-efficient fine-tuning: adapting models for specific tasks

### 3. Validation Strategy
- Benchmarked on 18 diverse genomic prediction tasks:
  - Histone modifications
  - Enhancer/promoter prediction
  - Splice site prediction
- Compared against supervised baselines (BPNet) and other foundation models (DNABERT-2, HyenaDNA, Enformer)
- Additional evaluation on specialized tasks (DeepSEA, SpliceAI, DeepSTARR)
- Analysis of model attention to understand learned genomic patterns
- Zero-shot and fine-tuned prediction of variant effects

## 🔬 Critical Technical Details

![Overview of NT training](../../../paper-figures/Nucleotide_Transformer.png)

### 1. Model Architecture and Training
- Encoder-only transformer architecture
- 6-mer tokenization (vocabulary of 4,104 tokens)
```
**"6-mer tokenization (vocabulary of 4,104 tokens)"**的详细解释：

1. **什么是6-mer**：
   - DNA序列由A、T、G、C四种核苷酸组成
   - "6-mer"表示将DNA序列切分为长度为6的连续片段
   - 例如序列"ATGGCAATG"可以切分为"ATGGCA"、"TGGCAA"、"GGCAAT"、"GCAATG"这样的6-mer

2. **词汇表大小计算**：
   - 理论上，6-mer的可能组合有4^6 = 4,096种（因为每个位置有4种可能的核苷酸）
   - 4,104 = 4,096 + 8，其中额外的8个token包括：
     - 4个单独的核苷酸表示(A, T, G, C)
     - N表示（用于表示不确定的核苷酸）
     - 特殊标记如PAD（填充）、MASK（掩码）和CLS（序列开始）标记
这几个特殊标记(PAD, MASK, CLS)源自于自然语言处理中的BERT模型，在Nucleotide Transformer中被应用于DNA序列处理：

### PAD（填充）标记
- **目的**：使批处理中的所有序列长度一致
- **工作原理**：当DNA序列长度不足所需的长度时（如不足1000个token），用PAD标记填充
- **特点**：模型被训练为忽略这些标记的内容，不将它们纳入损失函数计算
- **实际应用**：允许GPU高效处理不同长度的序列

### MASK（掩码）标记
- **目的**：实现掩码语言建模预训练
- **工作原理**：随机遮盖输入序列中约15%的token，替换为MASK标记
- **训练任务**：模型需要预测这些被遮盖位置的原始核苷酸序列
- **作用**：强制模型学习DNA序列的上下文依赖关系和生物学模式

### CLS（序列开始）标记
- **目的**：标记序列的开始并提供整个序列的表示
- **工作原理**：每个输入序列的最前面添加CLS标记
- **特点**：与该标记关联的嵌入可以捕获整个序列的全局信息
- **应用**：在分类任务中，CLS标记的最终嵌入常用于预测整个序列的属性

这些特殊标记使模型能够：
1. 高效处理变长序列
2. 学习DNA序列的内在结构和功能
3. 生成既包含局部又包含全局信息的序列表示

在预训练阶段，这些机制帮助模型建立对DNA语言的基本理解，为下游任务提供良好的初始化。

3. **为什么使用6-mer**：
   - 6-mer是序列长度（最多可处理6kb）和嵌入大小之间的折中方案
   - 研究者发现6-mer在性能上优于其他token长度
   - 使用6-mer可以捕获短距离的核苷酸依赖关系

4. **tokenization过程**：
   - 序列首先添加CLS（class）标记
   - 然后从左至右，尽可能匹配6-mer标记
   - 当无法匹配（如序列中含N或长度不是6的倍数）时，使用单核苷酸标记

这种分词方式允许模型处理长达6kb（v1模型）或12kb（v2模型）的DNA序列，同时保持计算效率和有效捕获序列模式的能力。
```
- Context length: 6kb for v1 models, 12kb for v2 models
- Pre-training using masked language modeling approach (BERT-style)
- Training infrastructure: 128 A100 GPUs across 16 nodes
- NT-v2 models incorporate architectural improvements:
  - Rotary embeddings instead of learned positional embeddings
  - SwiGLU activations
  - Removal of MLP biases and dropout mechanisms


NT-v2模型引入了几项架构改进，这些改进基于最新的自然语言处理研究发现，目的是提高模型性能并增强训练效率:

### 1. Rotary embeddings 替代 learned positional embeddings
- **传统位置编码**：v1模型使用可学习的位置编码，为序列中每个位置分配一个唯一的向量表示
- **旋转位置编码(RoPE)**：一种数学上更优雅的位置编码方式，通过复数旋转操作将相对位置信息编码入token表示中
- **优势**：
  - 更好地处理序列中的相对位置关系
  - 更容易扩展到更长的序列
  - 在每个注意力层应用，而不仅是在输入层
  - 提高了模型对位置信息的编码效率
    
---
<details>
  <summary><b> 什么是“位置编码”？（Positional Embeddings）</b></summary>
## **1. 什么是“位置编码”？（Positional Embeddings）**
在神经网络（如Transformer）中，文本序列中的**每个单词（Token）** 都需要转换成一个数值向量来输入模型。但是，**自注意力机制（Self-Attention）** 本身不理解单词在句子中的顺序，因此需要给每个 Token **添加位置信息**，这样模型才能正确理解句子结构。

### **类比**
🔹 **想象一个管弦乐队**，所有乐器（Token）一起演奏，但如果没有指挥（Positional Embedding）告诉它们**在何时演奏**，音乐就会变得混乱。因此，位置编码就是**让模型知道每个 Token 在句子中的“时间位置”**。

---

## **2. 传统位置编码（Learned Positional Embeddings）**
🔹 **方法**：
- 每个 Token 位置 \( i \) 都有一个**唯一的可学习向量**，模型在训练过程中调整这些向量，以便找到最优的表示方式。
- 这些向量是**固定的**，一旦训练好，就无法随意扩展到更长的序列。

### **示意图**
Imagine 每个 Token（单词）有一个唯一的位置标记：
```
Token1 → 位置编码: [0.1, 0.2, 0.3, ...]
Token2 → 位置编码: [0.5, 0.4, 0.6, ...]
Token3 → 位置编码: [0.7, 0.9, 0.1, ...]
```
但如果句子长度比训练时的最大长度**更长**，模型就无法正确编码新的位置信息。

### **问题**
✅ 容易训练  
❌ 不能泛化到更长的序列  
❌ 只编码**绝对位置**（无法很好表示**相对位置**）  

---

## **3. 旋转位置编码（RoPE，Rotary Positional Embeddings）**
🔹 **方法**：
- 通过**旋转向量**（Rotations in complex space）来编码相对位置信息，而不是为每个位置创建固定的编码。
- RoPE 通过 **余弦（cos）和正弦（sin）函数** 进行旋转，使得**相对位置信息**自然地影响 Token 之间的交互，而不是只依赖绝对位置。

### **示意图**
Imagine **每个 Token 是一个钟表指针**，指针的角度（旋转方向）代表它的位置：
```
Token1 → 旋转角度: 10°
Token2 → 旋转角度: 20°
Token3 → 旋转角度: 30°
```
所有 Token 的角度**一起旋转**，这样它们的**相对角度**始终保持一致，意味着模型能**自动捕捉 Token 之间的相对位置**。

### **数学表示**

#### **传统位置编码**
$$
Pos(i) = Embedding(i)
$$

#### **旋转位置编码（RoPE）**
$$
Token_i = e^{i\theta} \times Token_i
$$

其中，
$$ 
e^{i\theta} 
$$ 
是 **复数旋转**，使得每个 Token 通过 **角度偏移** 来编码它的位置。



---

## **4. 为什么 RoPE 更好？**

| **方法**         | **特点**                         | **问题**                     |
|----------------|--------------------------------|-----------------------------|
| 传统位置编码    | 训练时学习固定编码，无法外推    | 不能泛化到更长的序列          |
| RoPE           | 通过旋转角度编码相对位置        | 计算更高效，适用于长文本      |


✅ **相对位置信息**：RoPE 直接编码相对位置，而不是依赖绝对位置。  
✅ **更长的序列**：RoPE 的旋转编码可以无限扩展，不像传统方法有长度限制。  
✅ **每一层都能使用**：RoPE 在**每个注意力层** 都可以应用，而不是只在输入层。

---

### **总结**
✅ RoPE 不是直接给每个 Token 一个独立的编码，而是**通过旋转角度**来表示位置。  
✅ 这样，即使输入序列变长，模型仍然可以正确理解 **相对位置关系**。  
✅ RoPE 可以在**每一层注意力机制中使用**，而不仅限于输入层，提高模型的**计算效率和泛化能力**。

---
</details>


<details>
  <summary><b> GELU vs Swish vs SwiGLU 激活函数 </b></summary>
  
### **1. 传统激活函数（GELU）**
- GELU（Gaussian Error Linear Unit）是 Transformer 里常用的激活函数，它是 ReLU 的改进版本：
  ```
  GELU(x) = 0.5 * x * (1 + tanh(√(2/π) * (x + 0.044715 * x^3)))
  ```
- **特点**：
  - 平滑过渡，不像 ReLU 那样直接剪裁负值。
  - 适用于深度学习，梯度稳定。

---

### **2. Swish 激活函数**
- Swish 由 Google 提出，计算方式如下：
  ```
  Swish(x) = x * σ(x)
  ```
  其中，σ(x) 是 Sigmoid 函数：
  ```
  σ(x) = 1 / (1 + e^(-x))
  ```

---

### **3. SwiGLU（Swish + Gated Linear Unit）**
- SwiGLU 结合了 **Swish** 和 **门控线性单元（GLU）**，计算方式如下：
  ```
  SwiGLU(x) = Swish(xW) ⊙ (xV)
  ```
  其中：
  - `W` 和 `V` 是两个不同的权重矩阵。
  - `⊙` 代表逐元素相乘（Hadamard 乘积）。

---

### **4. 为什么 SwiGLU 更好？**

| **方法**  | **特点**             | **优势**                      |
|----------|------------------|-----------------------------|
| **GELU**  | 平滑非线性激活   | 训练稳定，但可能信息流不够强 |
| **Swish** | 平滑自适应激活   | 信息流更好，梯度稳定        |
| **SwiGLU** | 结合 Swish 和门控 | **训练更快、性能更强、计算效率高** |

✅ **SwiGLU 提高信息流动**，比 GELU **更高效**，比 Swish **更具灵活性**。


### **5. 视觉类比**
如果你想象 **激活函数像水管控制水流**：
- GELU：普通水龙头，流量较稳定。
- Swish：可以调节流量的水龙头，使水更平稳流动。
- SwiGLU：带有**智能开关**的水龙头，只让重要的信息流过，提高计算效率。

</details>


---

### 3. 移除MLP偏置和dropout机制
- **MLP偏置(bias)**：传统前馈网络中的可学习偏置项
- **Dropout**：训练中随机关闭部分神经元的正则化技术
- **为什么移除**：
  - 研究发现在大型Transformer模型中，移除这些组件可以简化模型
  - 减少过拟合风险
  - 使模型行为更加可预测
  - 在大规模预训练设置中通常不会损失性能

这些架构改进共同带来的效果：
- NT-v2 50M模型性能可与NT-v1 500M模型相当
- 允许处理更长上下文(从6kb扩展到12kb)
- 训练效率提高
- 推理速度更快
- 参数利用效率更高



### 2. Parameter-Efficient Fine-Tuning
- IA3 technique requiring only 0.1% of total parameters

<details>
  <summary><b>IA3</b></summary>
# IA3 (Infused Adapter by Inhibiting and Amplifying Inner Activations) 技术

IA3是一种参数高效的微调技术，在Nucleotide Transformer研究中用于适应预训练模型到特定下游任务。

## 核心原理

IA3通过引入少量可训练向量来调节预训练模型的行为，而无需更新所有模型参数。具体来说：

1. **保持预训练参数冻结**：原始变换器模型的权重保持不变
2. **添加轻量级可训练向量**：为每个变换器层添加三个可训练向量
3. **元素级缩放**：这些向量通过元素级乘法来调节模型的内部激活

## 技术细节

对于每个变换器层，IA3添加三个可训练向量：
- **l<sub>k</sub> ∈ ℝ<sup>d<sub>k</sub></sup>**：用于调节Key计算
- **l<sub>v</sub> ∈ ℝ<sup>d<sub>v</sub></sup>**：用于调节Value计算
- **l<sub>f</sub> ∈ ℝ<sup>d<sub>f</sub></sup>**：用于调节前馈网络

这些向量通过以下方式应用：
1. 在自注意力机制中：`softmax((Q(l_k ⊙ K^T))/√d_k)(l_v ⊙ V)`
2. 在前馈网络中：`(l_f ⊙ γ(W_1x))W_2`，其中γ是前馈网络的非线性函数

## 主要优势

1. **极少的额外参数**：仅增加约0.1%的参数量（相比全模型微调）
- 例如：25亿参数模型只需额外训练约250万参数

2. **计算效率**：
- 微调速度快（15分钟内完成）
- 只需单个GPU即可处理大型模型

3. **存储效率**：
- 全模型微调需要存储完整模型副本（例如：9.5GB × 18任务 = 171GB）
- IA3只需存储少量参数（例如：171MB）

4. **性能相当**：实现与全参数微调相近的性能

## 工作原理解释

IA3的核心思想是通过微调几个关键"旋钮"来调整模型行为，而不是重新训练整个模型。可以将其想象为：

- 不是重新布置整个乐队，而是微调几个关键乐器的音量
- 不是重新训练整个模型识别不同的模式，而是通过强化或抑制已习得的模式来适应新任务

通过这种方式，IA3能够有效利用预训练知识，同时使模型适应特定的基因组预测任务。
</details>

- Introduces three learned vectors per transformer layer
- Allows fine-tuning on a single GPU in under 15 minutes
- Comparable performance to full model fine-tuning

### 3. Key Results Quantification
- NT-v2 250M model achieved best performance (MCC 0.769) across benchmark tasks
- Multispecies 2.5B model matched or exceeded BPNet baselines on 14/18 tasks
```

These 18 tasks can be grouped into three main categories:
### 1. Chromatin Profiles (10 tasks)
Tasks predicting histone marks and chromatin accessibility in the K562 human cell line:
1. H2AFZ
2. H3K27ac
3. H3K27me3
4. H3K36me3
5. H3K4me1
6. H3K4me2
7. H3K4me3
8. H3K9ac
9. H3K9me3
10. H4K20me1

### 2. Regulatory Elements (5 tasks)
Tasks predicting gene regulatory regions:
1. Enhancers (binary classification)
2. Enhancer types (tissue-specific vs. tissue-invariant)
3. Promoters (all)
4. Promoters with TATA-box motif
5. Promoters without TATA-box motif

### 3. Splicing (3 tasks)
Tasks predicting RNA splicing sites:
1. Splice site (both acceptor and donor)
2. Splice acceptor
3. Splice donor
```
- Zero-shot variant effect prediction: AUC 0.7-0.8 for functional variants
- NT-v2 500M-parameter model with 12kb context matched/outperformed SpliceAI-10k
- NT-v2 models reduced parameters by 50x while maintaining/improving performance

## 🧪 Baseline Models, Evaluation Metrics, and Datasets

### Baseline Models
- BPNet: State-of-the-art convolutional architecture (original 121K and large 28M parameters)
- DNABERT-2: Pre-trained transformer for DNA
- HyenaDNA: Models with 1kb and 32kb context lengths
- Enformer: Architecture for long-range genomic interactions

### Evaluation Metrics
- Matthews Correlation Coefficient (MCC) for classification tasks
<details>
  <summary><b>Matthews Correlation Coefficient (MCC)</b></summary>
# Matthews Correlation Coefficient (MCC)

Matthews Correlation Coefficient (MCC) 是一种用于评估二分类模型性能的指标，在 Nucleotide Transformer 研究中用于评估分类任务的效果。

## 基本概念

MCC 是一种考虑了所有混淆矩阵元素（真阳性、真阴性、假阳性、假阴性）的平衡指标，特别适用于类别不平衡的数据集。它的值域在 -1 到 1 之间：
- +1：完美预测
- 0：等同于随机预测
- -1：完全相反的预测

## 计算公式

MCC 的计算公式如下：


MCC = (TP × TN - FP × FN) / √[(TP + FP)(TP + FN)(TN + FP)(TN + FN)]


其中：
- TP：真阳性（True Positive）
- TN：真阴性（True Negative）
- FP：假阳性（False Positive）
- FN：假阴性（False Negative）

## 为什么在基因组学中使用 MCC

MCC 在基因组学研究中的优势：

1. **类别不平衡鲁棒性**：基因组数据通常高度不平衡（如增强子、启动子区域在基因组中占比很小），MCC 不受类别比例影响

2. **综合考量**：同时考虑所有预测类型（TP、TN、FP、FN），提供更全面的评估

3. **严格性**：比准确率、精确率或召回率更难获得高分，能更好区分模型质量

4. **一致性**：允许在不同任务间进行性能比较

## 在论文中的应用

在 Nucleotide Transformer 研究中，MCC 用于评估各种分类任务的性能，包括：
- 染色质标记预测
- 启动子识别
- 增强子识别
- 剪接位点预测

计算了不同模型在各任务上的 MCC 值，然后计算每类任务的平均 MCC 来比较模型整体性能。这种方法使研究者能够公平比较不同模型架构在不同基因组预测任务上的表现。
</details>

- Area Under Curve (AUC) for chromatin profiles
- Precision-Recall AUC and top-k accuracy for splice prediction
- Pearson correlation for enhancer activity prediction

### Datasets
- Pre-training:
  - Human reference genome: 3.2B nucleotides
  - 1000G: 3,202 human genomes (20.5T nucleotides)
  - Multispecies: 850 genomes across diverse phyla (174B nucleotides)
- Benchmark: 18 curated genomic datasets from:
  - ENCODE (histone marks, enhancers)
  - GENCODE (splice sites)
  - Eukaryotic Promoter Database
- Variant effect datasets:
  - eQTLs and meQTLs from GRASP
  - Pathogenic variants from ClinVar and HGMD

## 💭 Critical Research Implications

### 1. Methodological Impact
- Demonstrates viability of self-supervised learning for genomics
- Provides robust benchmark for evaluating genomic foundation models
- Shows scaling laws in genomics modeling similar to NLP
- Establishes parameter-efficient fine-tuning as effective approach
- Challenges need for supervised pre-training

### 2. Biological Insights
- Models learn biologically meaningful features without supervision
- Attention maps reveal focus on functional elements
- Framework for understanding variant effects beyond protein-coding regions
- Suggests multispecies training captures conserved regulatory elements

### 3. Practical Applications
- Efficient prediction of molecular phenotypes from DNA
- Variant prioritization for clinical interpretation
- Characterization of non-coding regulatory regions
- Cross-species genomic annotation transfer
- Model compression while maintaining performance

## 📊 Future Research Directions

### 1. Technical Extensions
- Extending context length beyond 12kb for capturing long-range interactions
- Specialized architectures for specific genomic tasks
- Integration with other omics data modalities
- Further architectural optimizations based on demonstrated scaling laws
- Exploration of different tokenization strategies

### 2. Biological Questions
- Characterization of novel regulatory elements
- Understanding species-specific vs. conserved genomic patterns
- Mechanistic interpretation of attention patterns
- Exploration of epigenetic marks and 3D chromatin structure

### 3. Application Areas
- Personalized genomic interpretation
- Disease risk prediction from genetic variants
- Cross-species comparative genomics
- Regulatory element discovery
- Synthetic genomics and genome engineering

## 💡 Implementation Notes

### 1. Key Requirements
- High-performance computing resources for larger models
- Jax/PyTorch environment for model training/inference
- Efficient data pipelines for processing genomic sequences
- Parameter-efficient fine-tuning implementation
- Evaluation framework for diverse downstream tasks

### 2. Critical Considerations
- Choice of tokenization strategy (6-mers vs alternatives)
- Balancing model size vs. performance requirements
- Managing genomic data heterogeneity
- Task-specific fine-tuning strategies
- Appropriately splitting genomic data to avoid contamination
- Interpreting model outputs in biologically meaningful ways
