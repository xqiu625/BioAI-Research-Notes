# Deep generative modeling of transcriptional dynamics for RNA velocity analysis in single cells
 Nature Methods (2023 August)
 University of California, Berkeley

 文章link：
 https://www.nature.com/articles/s41592-023-01994-w 

 Github link：
 https://github.com/YosefLab/velovi_reproducibility 

 ## Summary:
 RNA速度分析框架。
 1. A principled framework for RNA velocity estimation that models unspliced and spliced RNA abundances as a function of kinetic parameters, latent time, and transcriptional state, while sharing information across cells and genes.
 2. A Bayesian approach that quantifies uncertainty in velocity estimates at the cell-gene level, enabling assessment of the reliability and applicability of velocity analysis for a given dataset.
 3. Improved performance compared to previous approaches (EM and steady-state models) in terms of parameter recovery, consistency across similar cells, alignment with orthogonal information, stability across quantification pipeline. 
 4. Introduction of metrics such as intrinsic and extrinsic uncertainty, velocity cohenrence score, and permutation score to enhance interpretation and scrutiny of velocity results at the level of individual cells, genes, and entire datasets.
 5. Provide am extensible framework for RNA velocity analysis. 


 ## Structure:
 1. Introduction:
 介绍了RNA速度的概念以及现有方法的局限性，提出了veloVI模型来解决这些问题。
 2. Results:
    a. 介绍了veloVI的变分推断框架以及如何估计RNA速度。
    b. 在模拟数据和真实数据集上将veloVI与现有方法（EM模型和稳态模型）进行了比较，展示了veloVI在各种指标上的优越性能。
    c. 介绍了veloVI的不确定性量化和速度相干性得分，以增强对RNA速度结果的解释。
    d. 提出了置换得分来评估数据集是否适合进行RNA速度分析。
    e. 展示了veloVI作为一个可扩展建模框架的灵活性，通过结合时间相关的转录速率来拟合更复杂的基因动力学。

3. Discussion：总结了veloVI的主要贡献,讨论了研究的意义以及未来的研究方向。
4. 方法(Methods):
    a. 详细介绍了veloVI模型的数学推导和实现细节。
    b. 描述了数据预处理、基准测试和分析中使用的步骤和参数设置。

## Algorithm Framework:
1. veloVI采用了一种基于变分推断的贝叶斯建模框架，将未拼接和已拼接的RNA的丰度建模为动力学参数，潜在时间和转录状态的函数，同时在细胞核基因之间共享信息。这使得模型能更好地捕捉细胞间的异质性和基因间的相关性。
2. 通过引入随机潜变量和近似后验推断，veloVI可以定量评估RNA速度分析对于给定数据集的可靠性和适用性。
3. 在模拟和真实数据集上的广泛验证表明，与先前的方法（如EM模型和稳态模型）相比，veloVI在参数恢复，细胞间一致性，与其他信息的一致性，不同定量流程下的稳定性以及拟合优度等方面都有更好的表现。
4. veloVI引入了内在不确定性，外在不确定性，速度相干性得分和置换得分等指标，可以在单个细胞，基因和整个数据集的层面上增强对RNA速度结果的解释和审查。
5. veloVI通过采用贝叶斯深度生成模型,提供了一种更严谨、可解释和可扩展的RNA速度分析框架。

## Baseline model, Evaluation metrics and Datasets:
1. 数据集(Datasets):

- 胰腺内分泌发育数据集(胰岛细胞分化)
- 精子发生数据集
- 小鼠发育中的齿状回数据集
- 小鼠前额皮层数据集
- 21-22月龄小鼠大脑数据集
- 小鼠视网膜数据集
- 人类外周血单个核细胞(PBMC)数据集
- 模拟数据集(用于评估方法的基本性能)
- 人类RPE1-FUCCI和U2OS-FUCCI细胞系的细胞周期数据集(提供了正交的细胞周期信息)

2. 基线模型(Baseline Models):
- EM模型(Expectation-Maximization model):一种先前提出的RNA速度估计方法,使用期望最大化算法来推断转录动力学参数和潜在时间。
- 稳态模型(Steady-state model):一种简化的RNA速度估计方法,假设转录过程处于稳态,并使用线性回归来估计速度。

3. 模型评估指标(Model Evaluation Metrics):

- 均方误差(Mean Squared Error, MSE):比较模型预测的RNA丰度与观察到的RNA丰度之间的差异。
- 速度一致性(Velocity Consistency):量化每个细胞的速度向量与其邻居的速度向量之间的一致性。
- 速度符号准确性(Velocity Sign Accuracy):将估计的速度符号(正、负或零)与基于正交信息(如细胞周期标记)得到的真实速度符号进行比较。
- 速度相关性(Velocity Correlation):计算不同定量流程下估计的速度之间的相关性,以评估方法的稳健性。
- 参数恢复准确性(Parameter Recovery Accuracy):在模拟数据上,比较估计的动力学参数与真实参数之间的相关性。
- 置换得分(Permutation Score):通过置换输入数据并比较置换前后的拟合误差,评估推断动力学对噪声的稳健性。

## Computing details:
计算机语言:

Python:文章中的大部分分析和实现都是使用Python编程语言完成的。

软件包:

1. PyTorch:veloVI模型是使用PyTorch深度学习库实现的,PyTorch提供了灵活的张量计算和自动微分功能。
2. scVelo:文章使用scVelo软件包进行数据预处理、RNA速度估计和可视化。scVelo是一个专门用于RNA速度分析的Python库。
3. scanpy:用于单细胞RNA测序数据处理和可视化的Python工具包。
4. NumPy:用于数值计算和数组操作的基础Python库。
5. SciPy:用于科学计算和统计分析的Python库。

计算资源:

1. CPU:文章中提到,EM模型是在配备Intel(R) Core i9-10900K CPU @ 3.70GHz的机器上运行的,使用了8个内核。
2. GPU:veloVI模型是在配备Nvidia RTX3090 GPU的机器上运行的,以加速模型的训练和推断过程。

数据预处理:

alevin、kallisto/bustools、velocyto、dropEst和STARsolo等工具被用于从原始测序数据中定量RNA丰度。

可视化:

1. matplotlib:用于创建静态、动画和交互式可视化的Python绘图库。
2. UMAP (Uniform Manifold Approximation and Projection):用于将高维数据降维到二维或三维空间进行可视化的非线性降维技术

#### 随机潜变量:
一个装满不同颜色球的罐子，每个球代表一个细胞的状态。罐子里球的颜色分布反映了不同细胞状态的总体分布。但是外面看不到里面球的分布。在veloVI模型中，这些球就是随机潜变量，它们表示了细胞的隐藏状态，如转录动力学和时间信息等。我们无法直接观察到这些隐藏状态，但是我们通过观察到的基因表达数据来推断它们。

#### 近似后验：
现在假设抽取了一些球，并根据抽到的球的颜色分布，你想估计整个罐子里球的颜色分布。由于你无法观察到所有的球，你只能根据抽到的球开近似估计原始分布。这个估计就是近似后验。在veloVI模型中，我们使用变分推断来找到一个近似后验分布，使其尽可能接近真实的后验分布，这个近似后验分布可以帮助我们推断细胞的隐藏状态，如转录动力学和时间信息等。
通过优化近似后验分布与真实后验分布之间的差异（通常通过KL散度来度量），我们可以不断的改进我们对细胞隐藏状态的估计。这个过程就像不断根据抽到的球来调整对罐子里球颜色的分布的估计，直到估计与实际分布很相似。

#### 变分推断的贝叶斯建模：

假设你在一个房间里寻找一件隐藏的宝藏。这个房间有许多可能藏宝藏的地方,每个地方都有不同的概率藏有宝藏。你的目标是找到最有可能藏有宝藏的地方。
在这个类比中:

- 房间代表了数据空间,即所有可能的数据点的集合。
- 宝藏代表了我们要推断的隐藏变量或参数,例如veloVI模型中的转录动力学参数和潜在时间。
- 每个地方藏有宝藏的概率代表了给定数据时,不同隐藏变量或参数值的概率分布,即后验分布。

贝叶斯建模:

在贝叶斯建模中,我们先假设一个先验分布,即在观察数据之前,我们对隐藏变量或参数的初始猜测。然后,我们根据观察到的数据来更新这个先验分布,得到后验分布。在寻宝游戏中,这就像你根据之前的经验,对每个地方藏有宝藏的概率有一个初始猜测,然后根据你观察到的线索(如房间的布局、装饰等)来更新你的猜测。

变分推断：

然而，在许多复杂的模型中，精确计算后验分布非常困难或者不行。这就像一个巨大的房间里寻宝，无法检查每一个可能的地方。变分推断通过引入一个近似后验分布来解决这个问题，这个近似分布通常有一个简单的形式，容易处理。在寻宝游戏中，这就像你制定一个策略，根据一些简单的规则（如always先检查最可疑的地方）来猜测宝藏的位置，而不是详尽地检查每个地方。

优化过程：

变分推断的目标是找到一个近似后验分布，使其尽可能接近真实的后验分布。这通常通过最大化一个称为"证据下界 (ELBO)" 的目标函数来实现。在寻宝游戏中，这就像你不断调整你的策略，直到你的猜测与宝藏的实际位置非常接近。

#### 内在不确定性：
假设你是一个射箭运动员，目标是射中靶心，内在不确定性就像你的手抖动的程度。即使你瞄准的方向是正确的，但由于你的手会抖动，箭可能偏离靶心。在veloVI模型中，内在不确定性反映了模型在估计单个细胞RNA速度时的不确定性，即使模型整体方向是正确的，单个细胞的速度估计差异很大。

#### 外在不确定性：
假设在一个风很大的日子射箭，外部因素为外在不确定性。在veloVI模型中,外在不确定性反映了细胞邻域内RNA速度估计的变化程度。即使单个细胞的速度估计很准确,但如果其邻居细胞的速度估计差异很大,那么外在不确定性就会很高。

#### 速度相干性 Velocity Correlation：
假设你是一个交警，需要车辆的速度和方向来判断车流的整体趋势。速度相干性得分就像你对个别车辆速度与整体车流方向一致性的评估。如果大部分车都按照车流的方向行驶，那么速度相干性得分就高；反之，如果许多车辆偏离车流方向，那么速度相干性得分就低。在veloVI模型中,速度相干性得分衡量了单个基因的RNA速度与整体细胞发育轨迹方向的一致性。

#### 置换得分 Permutation Score：
假设你是一名侦探，你需要判断一件证据对破案的重要性。你可以：将这个证据移除，看影响。影响大，置换得分高，在veloVI模型中,置换得分衡量了随机打乱某个基因的表达数据后,模型对RNA速度估计的影响。如果置换后RNA速度估计发生了很大变化,那么这个基因的置换得分就高,说明它对RNA速度估计很重要;反之,如果置换后RNA速度估计变化不大,那么这个基因的置换得分就低,说明它可能不太重要。