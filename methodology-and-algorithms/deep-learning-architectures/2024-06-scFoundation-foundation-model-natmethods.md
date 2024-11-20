# Large-scale foundation model on single-cell transcriptomics

nature methods (June 2024), biomap

paper link:
https://www.nature.com/articles/s41592-024-02305-7

github link:
https://github.com/biomap-research/scFoundation 

## Summary
这篇文章介绍了一个名为scFoundation的大规模预训练模型,用于单细胞转录组学数据分析。以下是文章的主要内容:

1. 研究背景与动机:
- 大型预训练模型在自然语言处理等领域取得了突破性进展,但在细胞和分子生物学领域还缺乏类似的基础模型。
- 单细胞RNA测序数据为开发细胞"语言"的基础模型提供了机会,但也面临数据规模大、基因数量多、测序深度变化大等挑战。

2. scFoundation模型:
- 包含约1亿参数,覆盖约20,000个基因,在超过5000万个单细胞转录组数据上预训练。
- 采用非对称的Transformer架构和特殊的预训练任务设计,能有效捕捉不同细胞类型和状态下基因间的复杂关系。

3. 主要创新:
- 收集和整合了大规模的单细胞数据用于预训练。
- 设计了xTrimoGene架构,能高效处理长序列的基因表达数据。
- 提出了考虑测序深度的预训练任务(RDA建模),使模型能处理不同测序深度的数据。

4. 性能评估:
在多个下游任务中取得了最先进的性能,包括:
- 基因表达增强
- 药物反应预测
- 单细胞药物反应分类
- 单细胞扰动预测
- 细胞类型注释
- 基因模块推断

5. 主要优势:
- 无需针对特定任务微调,就能在多种任务中表现出色。
- 能从单细胞数据中学习到有意义的细胞和基因表示。
- 在处理批次效应、预测药物反应等方面展现出潜力。

6. 局限性与未来方向:
- 预训练数据可能仍不足以完全反映人类器官发育和健康状态的复杂性。
- 预训练过程计算资源需求大,需要进一步优化。
- 未来可以尝试纳入基因组、表观基因组等多组学数据。

## Structure
这篇文章的结构如下:

1. 摘要

2. 引言
   - 大型预训练模型的背景
   - 单细胞RNA测序数据的特点和挑战
   - 研究目标和主要贡献

3. 结果
   3.1 scFoundation预训练框架
       - 模型设计
       - 预训练任务
       - 数据收集
   
   3.2 可扩展的读取深度增强模型(无需微调)
       - 模型规模和性能关系
       - 读取深度增强能力评估
       - 与其他方法的聚类性能比较
   
   3.3 改进癌症药物反应预测
       - scFoundation与DeepCDR的结合
       - 性能评估和比较
       - 新预测的验证
   
   3.4 将批量药物反应转移到单细胞水平
       - 与SCAD模型的结合
       - 性能评估
       - 嵌入表示的分析
   
   3.5 促进扰动响应预测
       - 与GEARS模型的结合
       - 单基因和双基因扰动预测
       - 基因相互作用类型的识别
   
   3.6 细胞类型注释
       - 性能评估和比较
   
   3.7 推断基因模块和基因调控网络
       - 基因模块识别
       - 基因调控网络推断

4. 讨论
   - 主要发现和贡献
   - 局限性
   - 未来研究方向

5. 方法
   - 预训练数据收集和预处理
   - scFoundation模型架构
   - RDA预训练任务
   - 下游方法详细说明
   - 评估指标

6. 参考文献

7. 致谢、作者贡献和竞争利益声明

## Workflow

![alt text](../../paper-figures/scFoundation_schematic.png)

这篇文章描述的研究工作流程可以概括如下:

1. 数据收集和预处理:
   - 从多个公共数据库收集超过50,000,000个人类单细胞RNA测序数据
   - 统一基因符号,进行质量控制
   - 将数据分为训练集和验证集

2. 模型设计:
   - 开发xTrimoGene模型架构
   - 设计嵌入模块、编码器和解码器
   - 设计读取深度感知(RDA)预训练任务

3. 模型预训练:
   - 使用收集的大规模数据集预训练scFoundation模型
   - 优化模型参数(约1亿个参数)

4. 模型评估和应用:
   4.1 读取深度增强:
       - 在独立测试数据上评估模型性能
       - 与其他方法进行比较(如MAGIC, SAVER, scImpute, scVI)

   4.2 癌症药物反应预测:
       - 结合DeepCDR方法
       - 在CCLE和GDSC数据集上进行评估

   4.3 单细胞药物敏感性预测:
       - 结合SCAD方法
       - 在多个药物数据集上进行评估

   4.4 扰动响应预测:
       - 结合GEARS方法
       - 在Perturb-seq数据上评估单基因和双基因扰动预测

   4.5 细胞类型注释:
       - 在Zheng68K和Segerstolpe数据集上进行评估
       - 与多种现有方法进行比较

   4.6 基因模块和调控网络推断:
       - 使用scFoundation生成的基因嵌入进行聚类和网络构建
       - 进行富集分析验证结果

5. 结果分析和讨论:
   - 总结scFoundation在各项任务中的表现
   - 分析模型的优势和局限性
   - 讨论未来的研究方向

6. 代码和数据发布:
   - 在GitHub上发布代码和预训练模型
   - 在公共平台上分享处理后的数据和生成的嵌入

## Algorithm Framework
scFoundation的算法框架主要包括以下几个关键部分:

1. 模型架构 (xTrimoGene):

   a. 嵌入模块:
      - 将连续的基因表达值直接转换为可学习的值嵌入
      - 加入基因名称嵌入,形成最终输入嵌入

   b. 编码器:
      - 仅处理非零和非掩码值的嵌入
      - 使用vanilla transformer块捕捉基因依赖关系

   c. 解码器:
      - 使用Performer变体处理全长基因序列
      - 结合零值和掩码嵌入

2. 读取深度感知(RDA)预训练任务:

   a. 输入处理:
      - 使用分层贝叶斯降采样策略生成低总计数变体
      - 计算原始和输入向量的总计数指标(T和S)

   b. 掩码策略:
      - 随机掩盖30%的基因表达值(包括零值和非零值)

   c. 预测目标:
      - 预测被掩盖位置的原始基因表达值
      - 使用回归损失进行优化

3. 预训练过程:

   - 输入: 单细胞基因表达向量,总计数指标T和S
   - 通过模型前向传播
   - 计算掩码位置的损失
   - 反向传播更新模型参数

4. 下游任务适配:

   a. 细胞嵌入生成:
      - 连接最大池化、平均池化、S token和T token的嵌入

   b. 基因上下文嵌入:
      - 从解码器提取每个基因的上下文嵌入

   c. 任务特定适配:
      - 药物反应预测: 结合DeepCDR
      - 单细胞药物敏感性: 结合SCAD
      - 扰动响应预测: 结合GEARS
      - 细胞类型注释: 添加MLP头

5. 模型训练和优化:

   - 使用梯度累积保持一致的批大小
   - 对于某些任务,仅微调编码器的一层
   - 使用加权交叉熵损失处理不平衡的细胞类型

## Baseline Model, Evaluation Metrics, and Datasets

1. 读取深度增强:

   Baseline Models: MAGIC, SAVER, scImpute, scVI
   Evaluation Metrics: 
   - Mean Absolute Error (MAE)
   - Mean Relative Error (MRE)
   - Pearson Correlation Coefficient (PCC)
   - Normalized Mutual Information (NMI)
   - Adjusted Rand Index (ARI)
   - Silhouette Coefficient (SIL)
   Dataset: 
   - 人类胰腺岛细胞数据集 (处理过的SAVER数据)
   - Zheng68K数据集 (约60,000个人类外周血单核细胞)

2. 癌症药物反应预测:

   Baseline Model: DeepCDR (原始版本)
   Evaluation Metrics: 
   - Pearson Correlation Coefficient (PCC)
   Dataset: 
   - Cancer Cell Line Encyclopedia (CCLE)
   - Genomics of Cancer Drug Sensitivity (GDSC)

3. 单细胞药物敏感性预测:

   Baseline Model: SCAD (原始版本)
   Evaluation Metrics: 
   - Area Under the ROC Curve (AUC)
   - Spearman Correlation
   Dataset: 
   - 四种药物 (sorafenib, NVP-TAE684, PLX4720, etoposide) 的单细胞数据

4. 扰动响应预测:

   Baseline Models: GEARS (原始版本), CPA
   Evaluation Metrics: 
   - Mean Square Error (MSE) of top 20 differentially expressed genes
   - Percentage of predicted values falling in 45-55% quantile of true expression
   - Pearson Correlation Coefficient (PCC) of magnitude scores
   Dataset: 
   - Perturb-seq数据 (Dixit et al. 和 Norman et al. 的数据集)

5. 细胞类型注释:

   Baseline Models: CellTypist, scBERT, scANVI, ACTINN, Scanpy, SingleCellNet
   Evaluation Metrics: 
   - Macro F1 Score
   Datasets: 
   - Zheng68K数据集
   - Segerstolpe数据集

6. 基因模块和调控网络推断:

   Baseline: 无直接基线比较
   Evaluation: 
   - 基因富集分析
   - 与已知生物学知识的一致性
   Dataset: 
   - Zheng68K数据集的子集 (300个细胞,来自3种细胞类型)

评估指标总结:
- 相关性指标: PCC, Spearman Correlation
- 聚类评估指标: NMI, ARI, SIL
- 预测准确性指标: MAE, MRE, MSE, AUC
- 分类性能指标: Macro F1 Score

数据集总结:
- 公开可用的大规模单细胞RNA-seq数据集 (如CCLE, GDSC, Perturb-seq等)
- 特定任务的基准数据集 (如Zheng68K, Segerstolpe等)
- 来自不同组织、疾病状态和实验条件的数据

## Computing Language, Tools, Packages, and Resources

这篇文章使用了多种计算语言、工具、包和资源。以下是对这些内容的总结：

计算语言：
1. Python（主要编程语言，用于模型实现和数据分析）

工具和包：
1. Seurat - 用于单细胞数据质量控制
2. Scanpy - 用于单细胞数据处理和分析
3. scikit-learn - 用于机器学习任务和评估指标计算
4. SCENIC - 用于基因调控网络推断
5. EnrichR - 用于基因富集分析
6. BBKNN - 用于批次效应校正

数据处理和分析包：
1. NumPy - 用于数值计算
2. Pandas - 用于数据处理
3. SciPy - 用于科学计算

深度学习框架：
（文章没有明确指出，但基于类似研究通常使用的框架，可能包括：）
1. PyTorch 或 TensorFlow - 用于构建和训练深度学习模型

可视化工具：
1. Matplotlib - 用于绘图（推测，因为是Python生态系统中常用的绘图库）
2. Seaborn - 用于统计数据可视化（推测）

资源：
1. Gene Expression Omnibus (GEO) - 用于获取公开的基因表达数据
2. Single Cell Portal - 单细胞数据资源
3. Human Cell Atlas (HCA) - 人类细胞图谱数据
4. European Molecular Biology Laboratory-European Bioinformatics Institute (EMBL-EBI) 数据库
5. HUGO Gene Nomenclature Committee - 用于基因符号统一
6. Cancer Cell Line Encyclopedia (CCLE) - 癌细胞系数据
7. Genomics of Cancer Drug Sensitivity (GDSC) 数据集
8. Perturb-seq 数据资源

计算资源：
（文章没有明确指出，但基于研究规模，可能使用了：）
1. 高性能计算集群 - 用于大规模模型训练
2. GPU 加速 - 用于深度学习模型训练

代码和数据共享平台：
1. GitHub - 用于发布代码和预训练模型
2. Figshare - 用于分享处理后的数据和生成的嵌入

其他：
1. Leiden 算法 - 用于聚类分析

#### 读取深度感知(RDA)
想象一下,你在阅读一本书,但是有些页面的字迹模糊不清。RDA预训练任务就像是在训练一个助手,让它能够"猜"出这些模糊的字是什么。
1. 模拟现实情况:
在单细胞测序中,不同细胞的测序深度(即能读到多少基因信息)是不同的。有些细胞信息很丰富,有些则信息稀少。RDA任务模拟了这种情况。
2. 创造"练习题":
- 原始数据: 想象这是一个完整的句子。
- 降采样: 随机删掉一些词,模拟测序深度不足的情况。
- 掩码: 再随机遮住一些词,这就是我们要让模型猜的部分。

3. 添加"提示": 给模型两个数字提示:
- T: 原始句子有多少个词
- S: 降采样后剩下多少个词 这样模型就知道要"脑补"多少信息。

4. 训练过程:
- 输入: 给模型看不完整的句子和两个数字提示。
- 任务: 让模型猜测被遮住的词。
- 学习: 比较猜测和真实答案,不断调整,提高猜测准确率。

5. 实际应用:
训练好后,当模型遇到信息不足的细胞数据时,可以根据已知信息和总体规律,推测出缺失的部分。

