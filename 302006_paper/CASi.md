
# CASi: A framework for cross-timepoint analysis of single-cell RNA sequencing data 
Scientific Reports (2024 May)

The University of Texas MD Anderson Cancer Center 

文章link：
https://www.nature.com/articles/s41598-024-58566-x 


Github link：
https://github.com/yizhuo-wang/CASi 

Summary
This paper presents CASi, a comprehensive framework for analyzing single-cell RNA seq data from multi-timepoint experiments.
The key aspects of CASi are:
1. Cross-timepoint cell annotation using an artificial neural network classifier, which borrows information across timepoints to accurately anntate cells. 
2. Detection of potentially novel cell types that emerge over time. CASi uses a pipeline based on correlation and t-tests to reliably distinguish novel cell types that only appear in later timepoints.
3. Visualization of cell population evolution over time using a combination of UMAP plots and fish plots.
4. Identification of temporal differentially expressed genes (tDEGs) that change significantly over time and have distinct patterns between different groups/conditions. This is done by combining a generalized linear model with iterative feature selection.
1. 跨时间点细胞注释：使用人工神经网络分类器，借用跨时间点的信息来准确注释细胞。
2. 检测潜在的新细胞类型：CASi使用基于相关性和t检验的管道来可靠地区分仅在后期时间点出现的新细胞类型。
3. 细胞群体演变的可视化：结合使用UMAP图和鱼图来可视化细胞群体随时间的演变。
4. 识别时间差异表达基因（tDEGs）：通过结合广义线性模型和迭代特征选择，识别在不同组/条件之间随时间显著变化并具有不同模式的基因。

Structure
1. Introduction
- Background on scRNA and its analysis pipeline
- Challenges in analyzing multi-timepoin scRNA-seq data
- Lack of comprehensive computational tools for multi-timepoint scRNA-seq analysis
2. Methods and Materials
- Overview of the CASi framework
- Description of the neural networks used for cross-timepoint cell annotation
- Pipeline for identifying novel cell types that emerge over time
- Identification of temporal tDEGs
- Evaluation metrics used to assess CASi's performance
3. Results
- Simulation study
    - A. CASi's accuracy in cross-timepoint cell annotation
    - B. CASi's ability to address the possibility of novel cell types
    - C. Visualization of cell type evolution using fish plots
    - D. CASi's performance in identifying tDEGs
- Application to a real-world multi-timepoint dataset (mantle cell lymphoma)
    - A. Cross timepoint annotation results
    - B. Identification of tDEGs and enrichment analysis
4. Discussion
- Summary of CASi's advantages and innovations
- Potential limitations and future extensions of the framework

算法解释

在这篇论文中，作者使用人工神经网络(Artificial Neural Networks, ANNs)来完成跨时间点的细胞注释任务。我们可以将神经网络看作是一个模仿人脑学习过程的计算模型。它由大量的互联节点（神经云）组成，通过调整节点之间的连接权重，不断从数据中学习特征，最终建立起输入数据和输出结果之间的复杂映射关系。

在跨时间点细胞注释的场景下：
1. 输入数据：来自初始时间点(t0)的细胞的基因表达数据及其对应的细胞类型标签，以及来自后续时间点(t1, t2)的细胞的基因表达数据。
2. 网络结构:作者设计了一个包含输入层、三个隐藏层和输出层的全连接神经网络。每层之间通过权重矩阵连接,并使用ReLU激活函数引入非线性变换。
3. 训练过程:使用t0时间点的数据对网络进行训练,通过前向传播计算预测输出,再通过反向传播算法更新权重以最小化预测标签与真实标签之间的差异。这个过程不断迭代,直到网络收敛或达到预设的停止条件。
4. 推断过程:将训练好的网络应用于t1和t2时间点的数据,根据细胞的基因表达谱预测其对应的细胞类型。
5. 优化策略:为了提高训练效率和模型性能,作者采用了一些优化策略,如使用Adam优化器、小批量训练、早停法和Dropout正则化等。

## datasets
1. Simulated datasets:
The authors generated simulated single-cell RNA-seq datasets to mimic various scenarios that could be encountered in real-world studies. They used a publicly available dataset of peripheral blood mononuclear cells (PBMCs) from 10X Genomics as the basis for their simulations. The simulated datasets were generated for three different scenarios:
a. Scenario 1: t0, t1, and t2 data contain the same cell types but with different cell type compositions.
b. Scenario 2: A cell type present in t0 disappears in t1 and t2 data.
c. Scenario 3: A new cell type appears in t1 and t2 data, which is not present in t0 data.
For each scenario, the authors performed 200 Monte Carlo simulations to evaluate the performance of CASi and compare it with baseline models using the accuracy and ARI metrics for cell annotation and the TDR metric for identifying tDEGs.

2. Real-world dataset:
The authors applied CASi to a real-world single-cell RNA-seq dataset of mantle cell lymphoma (MCL) patients. The dataset was obtained from the European Genome-Phenome Archive (EGA) database with the accession code EGAS00001005019. The MCL dataset included samples from 5 patients (3 ibrutinib-responsive and 2 non-responsive) collected at different time points (pre-treatment and post-treatment).

The authors used this dataset to demonstrate CASi's performance in cross-timepoint cell annotation, novel cell type detection, and identifying tDEGs in a real-world setting. They compared CASi's cell annotation results with baseline models using accuracy and ARI metrics. For tDEG identification, they compared the number of tDEGs detected by CASi and the Seurat FindMarker() function and performed enrichment analysis on the identified tDEGs to validate their biological relevance.

## Baseline models for cell annotation:
1. scmap
2. CHETAH
3. scPred
Baseline models for detecting differentially expressed genes (DEGs):
1. DESeq2
2. MAST
3. Wilcoxon Rank Sum test (as implemented in the FindMarker() function of the Seurat package)

## Model evaluation metrics:

For cell annotation:
a. Accuracy: The proportion of correctly labeled cells in the t1 and t2 datasets.
b. Adjusted Rand Index (ARI): A measure of clustering similarity that compares the predicted cell labels with the ground truth labels. ARI accounts for chance agreements and is more suitable for imbalanced datasets.

For detecting novel cell types:
They visually compared the UMAP plots of the ground truth and the predicted cell types to assess the ability of CASi to identify novel cell types.

For identifying temporal differentially expressed genes (tDEGs):
a. True Discovery Rate (TDR): The proportion of true positives among all the genes identified as tDEGs. TDR is equivalent to 1 - False Discovery Rate (FDR).



计算机语言:
- Python:主要用于实现神经网络模型和数据分析管道。
- R:用于部分数据分析和可视化任务。

Python软件包:
- Keras:一个高层神经网络API,用于构建和训练神经网络模型。
- UMAP (Uniform Manifold Approximation and Projection):用于数据降维和可视化。
- Louvain algorithm:用于细胞聚类和社区检测。

R软件包:
- Seurat:用于单细胞数据分析和可视化。
- scran:单细胞数据分析工具包。
- SINCERA:单细胞RNA测序数据分析流程。
- scmap:跨数据集投影单细胞RNA测序数据。
- CHETAH:层次化的细胞类型鉴定方法。
- DESeq2:差异表达分析工具。
- MAST:单细胞差异表达分析工具。

