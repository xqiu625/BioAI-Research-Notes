# Generative modeling of single-cell time series with PRESCIENT enables prediction of cell trajectories with interventions

Nature communications (2021 May), MIT

paper link:
https://www.nature.com/articles/s41467-021-23518-w

github link:
https://github.com/gifford-lab/prescient 

## Summary
## Structure
## Workflow
## Algorithm framework 
![alt text](image-5.png)
a. 现有的单细胞发育模型可以描述为在伪时间或实际时间（x轴）中运行，并根据它们对基础分化过程的建模程度（y轴）进行分类。PRESCIENT用红色突出显示。

b. 人口水平时间序列数据的观测值用于一个生成框架，该框架在物理时间中建模基础动态过程。细胞状态的演变由漂移项和噪声项控制。漂移（实线箭头表示）定义为势函数的负梯度（背景中的颜色梯度表示）。虚线表示噪声。模型使用人口水平时间序列数据的观测值（实心圆表示）进行拟合。细胞状态的模拟（虚线圆表示）。

c. 模型拟合过程的示意图。参数化基础漂移函数μ的神经网络以观察到的时间点的基因表达数据的PCA投影作为输入（同样用实线表示）。然后通过一阶时间离散化模拟随机过程，以生成下一个时间步的群体，依此类推。直到下一个观察到的时间点，此时最小化模拟和预测群体之间的损失。模型通过两项任务进行验证。

d. 留出恢复，模型被要求预测留出时间点的边际分布。

e–f. 命运预测，模型被要求预测给定祖细胞的命运分布结果。命运预测可以应用于数据集中观察到的细胞（e）或通过计算机模拟施加了一些扰动的细胞状态（f）。如图所示，扰动导致命运分布结果的显著变化。

## Baseline models, Evaluation metrics, and Datasets
## Computing language，tools, packages and resources
