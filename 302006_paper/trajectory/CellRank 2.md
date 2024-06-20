# CellRank 2: unified fate mapping in multiview single-cell data

nature methods(2024 June), Helmholtz Munich

paper link:
https://www.nature.com/articles/s41592-024-02303-9

github link:
https://github.com/theislab/cellrank 


## Summary
1. 采用kernel-estimator设计将转移矩阵的推断和分析解耦。Kernels用于根据不同数据视角估计转移矩阵,estimators用于分析转移矩阵得到初始状态、中间状态、终末状态等。这种模块化结构使CellRank 2适用于多种数据形式。
2. 引入了新的kernels用于处理pseudotime(PseudotimeKernel)、发育潜能(CytoTRACEKernel)、时间序列数据(RealTimeKernel)以及代谢标记数据,拓展了CellRank的应用范围。
3. 通过加速主要estimator、重构代码以及可视化随机游走等改进,大幅提升了计算效率和可解释性。
4. 成功应用于人类造血、内胚层发育、咽内胚层发育等多个生物学系统,展示了CellRank 2在揭示细胞命运和识别driver基因等方面的性能优势。
5. 未来计划引入局部kernel组合、整合扰动数据等,进一步增强CellRank 2揭示分子机制驱动细胞命运决策的能力。

## Structure
1. 介绍
    - 单细胞测序技术背景
    - 现有轨迹推断方法的局限性
    - CellRank 2的主要特点和改进
2. 结果
    - CellRank 2的模块化设计与关键创新
        - Kernel-estimator设计
        - 引入新的kernels处理多种数据形式
        - 计算效率和可解释性的改进
    - 应用1:利用pseudotime overcome RNA velocity limitations in造血
    - 应用2:利用发育潜能分析内胚层发育
    - 应用3:利用时间序列数据分析咽内胚层发育
    - 应用4:利用代谢标记数据估计肠器官中的动力学参数和命运
3. 讨论
    - CellRank 2在快速整合新数据形式方面的优势
    - 与其他时间序列数据分析方法的比较
    - 利用代谢标记数据研究基因调控策略
    - 不同kernels的适用场景与选择指南
    - 未来拓展方向:整合空间信息、遗传谱系示踪、扰动数据等
4. 方法
    - CellRank 2的整体框架
    - 各个kernels的原理与实现
    - 性能改进的技术细节
    - 数据集来源与预处理

## Workflow
1. 数据预处理
    - 过滤低质量细胞和基因
    - 标准化、log转换、高变基因选择等
2. 选择合适的kernel计算转移矩阵
    - RNA速度数据:VelocityKernel
    - Pseudotime数据:PseudotimeKernel
    - 发育潜能数据:CytoTRACEKernel
    - 时间序列数据:RealTimeKernel
        - 利用Waddington Optimal Transport (WOT)计算时间点间转移
        - 利用相似性计算时间点内转移
        - 组合时间点间和时间点内转移矩阵
    - 代谢标记数据:
        - 脉冲追踪实验估计转录和降解速率
        - 基于估计的动力学参数构建速度场
        - 利用VelocityKernel转化为转移矩阵

![alt text](../Figures/CellRank 2_kernel choice.png)

3. 利用estimators分析转移矩阵
    - 应用广义Perron聚类分析(GPCCA)识别初始态、中间态和终末态
    - 计算每个细胞到终末态的吸收概率作为命运概率
    - 通过命运概率与基因表达的相关性鉴定命运调控基因
    - 拟合广义可加模型(GAM)刻画沿着拟时间的基因表达变化

#### Kernel-estimator 
Kernel-estimator设计是CellRank 2的核心创新,它将转移矩阵的推断(inference)和分析(analysis)解耦为两个独立的步骤,由kernels和estimators分别负责。这种模块化的设计带来了以下优势:
1. 灵活处理多种数据形式:
    - Kernels根据不同的数据视角或先验知识估计细胞之间的转移概率,生成转移矩阵。每个kernel专门处理一种特定的数据输入,如RNA速度、伪时间轨迹、时间序列、代谢标记等。
    - Estimators则以转移矩阵为输入,推断细胞状态、命运概率等下游信息。由于estimators与具体的数据形式无关,因此可以方便地组合不同的kernels,实现多视角数据的整合分析。
2. 便于扩展新的数据类型:
    - 当新的单细胞数据类型出现时,只需开发相应的kernel,而无需修改现有的estimators。这种解耦设计让CellRank 2能快速适应单细胞测序技术的发展,持续拓展其应用范围。
    - 同时,研究者也可以开发自己的kernels,将CellRank 2与其他算法(如用于整合空间信息的方法)结合,发挥协同作用。

#### 转移矩阵 ： 
在CellRank 2的背景下,转移矩阵是一种数学工具,用来描述细胞状态变化的概率。通俗地说,它就像是一张"细胞命运地图",告诉我们每个细胞可能变成什么样子。

- 想象一个城市有三个区域:住宅区、商业区和工业区。每天,人们会在这些区域之间移动。转移矩阵就像是一张表格,记录了人们从一个区域移动到另一个区域的概率:

```
        住宅区   商业区   工业区
住宅区     0.7     0.2     0.1
商业区     0.3     0.6     0.1
工业区     0.2     0.3     0.5
```
这个表格告诉我们:
    - 在住宅区的人有70%的概率留在住宅区,20%的概率去商业区,10%的概率去工业区。
    - 在商业区的人有30%的概率回住宅区,60%的概率留在商业区,10%的概率去工业区。
    - 在工业区的人有20%的概率去住宅区,30%的概率去商业区,50%的概率留在工业区。
在细胞生物学中,这个"城市"就是我们的数据集,而"区域"则代表不同的细胞状态或类型。转移矩阵描述了细胞从一种状态转变为另一种状态的概率。

这个"细胞命运地图"帮助我们:
    - 预测细胞的未来发展方向
    - 识别关键的细胞状态转换点
    - 理解细胞分化过程中的主要路径

通过分析这个矩阵,研究人员可以推断出细胞的初始状态、中间状态和终末状态,以及细胞沿着不同发展路径的概率,从而深入理解细胞命运决策的过程。

## Algorithm Framework
![alt text](../Figures/CellRank%202.png)


## Baseline Model, Evaluation Metrics, and Datasets
## Computing Language, Tools, Packages, and Resources



