# Generative modeling of single-cell time series with PRESCIENT enables prediction of cell trajectories with interventions

Nature communications (2021 May), MIT

paper link:
https://www.nature.com/articles/s41467-021-23518-w

github link:
https://github.com/gifford-lab/prescient 

## Summary
这篇文章介绍了PRESCIENT（潜在能量基础上的单细胞梯度），这是一个生成建模框架，从时间序列单细胞RNA测序（scRNA-seq）数据中学习潜在的分化景观。
1. PRESCIENT模型：
    - 将细胞分化建模为基因表达景观上的扩散过程，该景观由一个势函数参数化，该势函数引导细胞向低潜能区域移动。
    - 通俗解释：
        站在山顶，底下的山峦就是基因表达景观，每个位置的高度代表了细胞在该状态下的“势能”。“势能”越高，细胞越不稳定；“势能”越低，细胞越稳定。
        现在，假设你是一个细胞，正在这个景观中旅行，在分化过程中，会自然从高势能滚落到低势能。这个”滚落“就是”扩散过程“。
        但是，这个过程是不完全确定的，会偶尔跳跃到一个意想不到的状态，这就像球在滚落的过程中偶尔会碰撞并偏离轨道。这种随机过程就是代表了分化过程的噪声和不确定性。
        势函数的作用就像是一张地图，告诉细胞在景观中的每个位置应该朝哪个方向滚落，引导细胞向着更稳定、更成熟的状态发展。
2. 模型拟合：
    - 通过最小化经验和预测细胞群体在观察到的时间点上的正则化Wasserstein损失来拟合模型。
    - 通俗解释：
        假设我们有一群细胞在基因表达景观中"旅行"。我们在不同的时间点对这群细胞进行快照,记录下它们的位置。这就是我们的"经验"数据。
        现在,我们要建立一个模型来模拟这个过程。这个模型就像一个虚拟的景观,我们希望它能尽可能地反映真实的情况。我们可以在模型中放置一群虚拟的细胞,让它们在这个虚拟的景观中"旅行",并在相同的时间点记录它们的位置。这就是我们的"预测"数据。
        如何判断我们的模型是否足够好呢?一个直观的方法是比较每个时间点上真实细胞和虚拟细胞的位置。如果它们非常接近,那么我们的模型就是一个好的模型。这就是"最小化经验和预测细胞群体之间的损失"的意思。
        但是,仅仅比较位置还不够。我们还需要考虑细胞的分布。如果真实细胞和虚拟细胞的位置很接近,但它们的分布却很不一样(例如,真实细胞聚集在一起,而虚拟细胞分散开来),那么我们的模型仍然不够好。Wasserstein距离就是一种能够同时考虑位置和分布的度量方法。
        "正则化"是一种防止模型过于复杂的技术。如果我们的模型太复杂,它可能会过度拟合数据中的噪声,反而失去了对真实过程的洞察。正则化就像是一个惩罚项,鼓励模型保持简单。
    #### Wasserstein距离 :
        Wasserstein距离(也称为推土机距离或Earth Mover's Distance)是一种衡量两个概率分布之间差异的指标.
    #### 正则化Wasserstein损失:
        在训练生成模型(如GAN)时,我们希望生成的分布尽可能接近真实数据分布。传统的损失函数如JS散度存在不连续和模式丢失等问题。正则化Wasserstein损失通过以下方式改进:
        使用Wasserstein距离作为衡量生成分布和真实分布差异的指标,利用其良好的数学性质。
        在损失函数中加入正则化项,鼓励生成分布在样本空间中更加平滑和连续。
        具体来说,正则化Wasserstein损失可以表示为:
            oss = Wasserstein距离(生成分布,真实分布) + λ * 正则化项
        其中正则化项可以是生成样本之间的Wasserstein距离、梯度惩罚项等,用于约束生成分布的平滑性和连续性。
3. 细胞增殖的纳入：
    - PRESCIENT通过根据每个细胞的预期后代数量对源细胞群中的每个细胞进行加权，纳入了细胞增殖的影响。
4. 性能表现：
    - 在谱系追踪数据集中，当考虑细胞增殖时，PRESCIENT在预测祖细胞的命运偏向方面优于现有方法。
5. 生成模型能力：
    - 作为生成模型，PRESCIENT可以模拟训练过程中未观察到的细胞的轨迹，包括基因表达谱被计算扰动的细胞。
6. 计算扰动实验：
    - 对参与造血和胰腺β细胞分化的转录因子进行计算扰动，重现了预期的细胞命运分布变化。
    - 通俗解释：
       在计算机模型中模拟了特定转录因子活性的改变,并观察到了与实际生物学知识相符的细胞命运变化。这表明模型准确地捕捉了这些转录因子在细胞分化过程中的作用,并可以用于预测未知的扰动的效果。
7. 大规模组合计算扰动实验：
    - PRESCIENT能够进行大规模组合计算扰动实验，以帮助设计分化协议和基因筛选。
    - 通俗解释：
        在模型中系统地测试大量不同的开关组合,看看哪些组合可以产生我们想要的细胞命运分布。
        这就像在虚拟的山峦景观中尝试了许多不同的开关组合,看看哪些组合可以使更多的小球滚落到我们想要的山谷中。
        一旦我们在模型中找到了有前景的开关组合,我们就可以在实际的生物学实验中重点测试这些组合。这可以大大缩小实验的范围,节省时间和资源。
## Structure
1. Introduction
    - Motivation and background on modeling developmental landscapes from scRNA-seq data
    - Limitations of existing computational approaches (pseudo-time ordering, fate prediction methods)
    - Overview of PRESCIENT framework and its advantages
2. Results
    - Learning a generative model of cellular differentiation from high-dimensional scRNA-seq data
        - Modeling differentiation as a diffusion process over a potential landscape
        - Fitting the model by minimizing regularized Wasserstein loss
        - Incorporating cell proliferation by weighting cells based on expected descendants
    - Validation on experimental lineage tracing dataset
        - Recovery of held-out timepoint
        - Predicting clonal fate bias, outperforming existing methods when accounting for cell proliferation
    - Simulating trajectories for cells not observed during training
        - Generative nature allows simulating trajectories for unobserved or perturbed cells
    - In silico perturbation experiments
        - Recapitulating expected outcomes of perturbing transcription factors in hematopoiesis and pancreatic β cell differentiation
        - Introducing perturbations at different timepoints and developmental stages
        - Enabling large-scale combinatorial perturbation screens
3. Discussion
    - Significance of generative modeling for studying differentiation landscapes
    - Comparison to existing methods and advantages of PRESCIENT
    - Limitations and assumptions of the model
    - Potential future extensions (e.g., incorporating lineage tracing data, RNA velocity)
    - Applications in designing differentiation protocols and genetic screens
4. Methods
    - Mathematical formulation of the diffusion process and potential landscape
    - Model implementation and optimization
    - Preprocessing of scRNA-seq datasets
    - Experimental details and evaluation metrics for various analyses
## Workflow
1. 数据预处理
    - 下载并预处理Weinreb等人和Veres等人的单细胞RNA测序数据集
    - 特征选择: 识别高变基因,去除与细胞周期相关的基因
    - 数据标准化、缩放和主成分分析(PCA)降维
2. 训练PRESCIENT模型
    - 将细胞分化建模为基因表达景观上的扩散过程,由势函数参数化
    - 通过最小化经验分布和预测分布之间的正则化Wasserstein损失来拟合模型
    - 根据每个细胞的预期后代数量为其加权,以考虑细胞增殖的影响
    - 使用深度学习方法学习势函数的参数
3. 模型验证
    - 在留出的时间点上评估模型的恢复能力
    - 预测克隆命运偏差,在考虑细胞增殖时优于现有方法
    - 对未在训练中观察到的细胞模拟轨迹
4. 体外扰动实验
    - 对造血和胰腺β细胞分化中的转录因子进行扰动,重现预期的细胞命运变化
    - 在不同时间点和发育阶段引入扰动
    - 进行大规模组合扰动筛选
5. 分析和解释结果
    - 与现有方法比较,展示PRESCIENT的优势
    - 讨论模型的局限性和假设
    - 提出未来的拓展方向,如整合谱系示踪数据和RNA速度信息
    - 在设计分化方案和遗传筛选中的应用
## Algorithm framework 
![alt text](image-5.png)
a. 现有的单细胞发育模型可以描述为在伪时间或实际时间（x轴）中运行，并根据它们对基础分化过程的建模程度（y轴）进行分类。PRESCIENT用红色突出显示。

b. 人口水平时间序列数据的观测值用于一个生成框架，该框架在物理时间中建模基础动态过程。细胞状态的演变由漂移项和噪声项控制。漂移（实线箭头表示）定义为势函数的负梯度（背景中的颜色梯度表示）。虚线表示噪声。模型使用人口水平时间序列数据的观测值（实心圆表示）进行拟合。细胞状态的模拟（虚线圆表示）。

c. 模型拟合过程的示意图。参数化基础漂移函数μ的神经网络以观察到的时间点的基因表达数据的PCA投影作为输入（同样用实线表示）。然后通过一阶时间离散化模拟随机过程，以生成下一个时间步的群体，依此类推。直到下一个观察到的时间点，此时最小化模拟和预测群体之间的损失。模型通过两项任务进行验证。

d. 留出恢复，模型被要求预测留出时间点的边际分布。

e–f. 命运预测，模型被要求预测给定祖细胞的命运分布结果。命运预测可以应用于数据集中观察到的细胞（e）或通过计算机模拟施加了一些扰动的细胞状态（f）。如图所示，扰动导致命运分布结果的显著变化。

PRESCIENT算法框架巧妙地将细胞分化建模为扩散过程,通过最小化正则化Wasserstein损失学习势函数参数,同时考虑了细胞增殖的影响。这种方法能够生成未观察到的细胞轨迹,并模拟体外扰动实验,为理解细胞分化机制和优化分化方案提供了强大的工具。
1. 细胞分化的扩散过程建模
    - 将细胞分化建模为基因表达景观上的扩散过程，由势函数Ψ(x)参数化
    - 细胞状态的演变由漂移项(drift term)和噪声项(noise term)控制
    - 漂移项定义为势函数的负梯度,μ(x)=-∇Ψ(x),表示驱动细胞向低势能区域移动的力
2. 势函数的参数学习
    - 通过最小化经验分布和预测分布之间的正则化Wasserstein损失来学习势函数的参数
    - 使用深度神经网络来参数化势函数,允许复杂的景观表示
    - 通过自动微分计算势函数的梯度,实现漂移函数的灵活参数化
3. 细胞增殖的建模
    - 通过根据每个细胞的预期后代数量为其加权,将细胞增殖纳入模型
    - 当谱系示踪数据可用时,直接估计每个细胞的后代数量
    - 当谱系示踪数据不可用时,使用基因表达估计细胞增殖率
4. 模型训练和优化
    - 使用Adam优化器和批量随机梯度下降法训练模型
    - 采用预训练策略,先优化熵正则化项,再优化整个损失函数
    - 使用多尺度Sinkhorn算法近似计算Wasserstein距离
5. 扰动实验的体外模拟
    - 通过设定目标基因的表达水平(z-score),引入体外扰动
    - 将扰动后的基因表达谱转换到PCA空间,用于初始化训练好的模型
    - 模拟扰动后细胞的轨迹,并评估其对最终细胞命运分布的影响

## Baseline models, Evaluation metrics, and Datasets
## Computing language，tools, packages and resources
