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
## Algorithm Framework
![alt text](../Figures/CellRank%202.png)


## Baseline Model, Evaluation Metrics, and Datasets
## Computing Language, Tools, Packages, and Resources