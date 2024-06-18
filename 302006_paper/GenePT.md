# GenePT: A Simple But Effective Foundation Model for Genes and Cells Built From ChatGPT

preprint (2023 Oct) Stanford

paper link: 
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10614824/

github link:
https://github.com/yiqunchen/GenePT
 
 ## Summary
 本文介绍了GenePT，这是一种简单但有效的方法，使用GPT-3.5的自然语言嵌入来表示基因和细胞。
 1. 基因嵌入：GenePT


 ## Structure 
 ## Workflow
 ## Algorithm framework 
 ![alt text](image-4.png)
    (a) 对于每个基因，我们从NCBI提取其相应的文本摘要，并使用GPT-3.5文本嵌入作为其表示。
    (b) 在GenePT-w细胞嵌入框架中，我们对步骤(a)中的基因嵌入按其在细胞中的表达水平进行加权平均，并将这些细胞嵌入归一化到单位ℓ2范数。
    (c) 在GenePT-s细胞嵌入框架中，输入的单细胞数据中的每个细胞都根据基因表达排名被翻译成一句自然语言句子，并使用整句的GPT-3.5嵌入来表示该细胞。

 ## Baseline models, Evaluation metrics, and Datasets
 1. 基线模型(Baseline Models):
    - Geneformer:一个在大规模单细胞转录组数据上预训练的transformer模型,用于基因和细胞嵌入表示学习。
    - scGPT:另一个在大规模单细胞多组学数据上进行生成式预训练的transformer模型。
    - Gene2Vec:基于基因共表达模式学习基因分布式表示的一种方法。
    - BioLinkBERT:一种在生物医学文本上预训练的语言模型。
 2. 评估指标(Evaluation Metrics):
    - 精确率(Precision)、召回率(Recall)、F1值:用于评估分类任务的预测性能。
    - ROC-AUC:评估二分类任务的预测性能。
    - 调整兰德指数(Adjusted Rand Index, ARI)和调整互信息(Adjusted Mutual Information, AMI):衡量聚类结果与真实标签的一致性.

    #### Adjusted Rand Index (ARI)
    是一种评估聚类算法性能的指标,用于衡量两种聚类结果之间的相似程度。它是对 Rand Index 的改进,通过考虑随机情况下的期望值,从而更准确地反映聚类质量。

        ARI 的计算过程如下:
        1. 构建两个聚类结果的contingency表,表示样本在两种聚类中的分配情况。
        2. 计算 Rand Index (RI),即样本对被正确分类(同一类或不同类)的比例。
        3. 计算 RI 在随机情况下的期望值 E(RI)。
        4. 使用公式:
            ARI = (RI - E(RI)) / (max(RI) - E(RI))\
            对 RI 进行调整,得到 -1 到 1 之间的 ARI 值。\
            其中 ARI=1 表示两个聚类结果完全一致,ARI=0表示与随机分配无异,ARI<0 表示两个聚类结果的差异大于随机水平。
    #### Adjusted Mutual Information (AMI) 
    是一种评估两个聚类结果相似度的指标,它是基于互信息(Mutual Information)的改进版本。\
    什么是互信息
        互信息衡量两个随机变量之间的相关性。在聚类分析中,可以将两个聚类结果U和V看作两个随机变量,互信息I(U,V)反映了这两个聚类结果之间的一致性程度。
        然而,互信息的值域没有固定范围,难以解释和比较。而且,即使两个聚类结果是完全随机的,互信息也不为0,这使得评估变得困难。

    AMI的改进 \
        - 为了解决上述问题,AMI对互信息进行了调整:
        1. 计算互信息I(U,V)。
        2. 计算在随机情况下的期望互信息E[I(U,V)]。
        3. 使用公式:
            AMI = (I(U,V) - E[I(U,V)]) / (max(H(U),H(V)) - E[I(U,V)])  \
            对互信息进行归一化,其中H(U)和H(V)分别是U和V的熵。\
        通过减去期望互信息,AMI消除了随机效应的影响。归一化后,AMI的取值范围是[-1,1],更易于解释:
        - AMI=1,表示两个聚类结果完全一致
        - AMI=0,表示两个聚类结果与随机分配无异
        - AMI<0,表示两个聚类结果的差异大于随机水平
        - AMI广泛应用于评估聚类算法的性能,尤其在存在类别不平衡或噪声数据时,AMI比其他指标如Rand Index更可靠。

 3. 数据集(Datasets):
    - 基因功能分类数据集:包含15个最常见的基因功能类别的标注数据。
    - 基因特性预测数据集:包括剂量敏感性、组蛋白修饰、转录因子作用范围等多个二分类任务。
    - 基因-基因相互作用数据集:根据共享基因本体论(Gene Ontology)标注构建的基因相互作用预测任务。
    - 人类免疫组织单细胞数据集:用于无监督探索GenePT得到的基因模块。
    - 心肌细胞和主动脉单细胞数据集:用于评估GenePT去除批次效应同时保留生物学差异的能力。
    - 几个具有细胞类型标注的单细胞数据集(如hPancreas、Myeloid等):用于评估细胞嵌入表示捕获生物学变异的能力。


 ## Computing language，tools, packages and resources

