# scVelo 分析教程与RNA Velocity动态建模

## 一、RNA Velocity建模方法演进

RNA velocity分析经历了三代模型的发展，每一代都在准确性和复杂性上有所提升：

### 1. 稳态模型（第一代）
- **特点**：假设细胞处于稳定状态
- **原理**：类似观察平稳流动的河流
- **局限**：无法准确捕捉快速状态转换
- **适用**：稳定状态的细胞分析

### 2. 随机模型（第二代）
- **特点**：考虑基因表达的自然波动
- **原理**：类似考虑河流速度的随机变化
- **优势**：比稳态模型更准确
- **局限**：仍然假设相对稳定状态

### 3. 动态模型（第三代）
- **特点**：
  - 考虑每个基因的独特转录和剪接模式
  - 可处理细胞状态快速转换
  - 能识别关键驱动基因
- **优势**：
  - 最准确的预测能力
  - 可捕捉瞬时状态
  - 提供基因层面的深入洞察
- **应用**：适合分析发育和分化过程

## 二、scVelo分析流程

### 1. 环境准备
```python
import scvelo as scv
import scanpy as sc
import numpy as np
import pandas as pd
import anndata as ad
import matplotlib.pyplot as plt

# 设置绘图参数
scv.settings.set_figure_params('scvelo', 
    facecolor='white',
    dpi=100, 
    fontsize=6,
    color_map='viridis',
    frameon=False)
```

### 2. 数据加载与预处理
```python
# 读取loom文件
ldata1 = scv.read("sample1.loom", cache=True)

# 处理样本标识符
barcodes = [bc.split(':')[1] for bc in ldata1.obs.index.tolist()]
barcodes = [bc[0:len(bc)-1] + '-1_1' for bc in barcodes]
ldata1.obs.index = barcodes

# 合并多个样本（如果需要）
ldata = ldata1.concatenate([ldata2])

# 数据过滤和标准化
scv.pp.filter_and_normalize(adata)

# 计算距离矩阵
scv.pp.moments(adata, n_neighbors=30, n_pcs=30)
```

### 3. 速率分析
```python
# 恢复动态信息
scv.tl.recover_dynamics(adata, var_names='all')

# 计算速率
scv.tl.velocity(adata, mode='dynamical')

# 构建速率图
scv.tl.velocity_graph(adata)
scv.tl.velocity_embedding(adata, basis="umap")
```

### 4. 可视化设置
```python
# 设置聚类类别
adata.obs["cluster_names"] = adata.obs["cluster_names"].astype('category')

# 设置颜色方案
color = pd.read_csv("color_scheme.txt", sep=" ", header=0, index_col=0)
color.index = color["cluster_names"]

# 按分类排序
adata.obs["cluster_names"].cat.reorder_categories(
    color["cluster_names"], 
    inplace=True
)

# 设置颜色
color_used = list(color.loc[adata.obs["cluster_names"].cat.categories,"color"])
adata.uns['cluster_names_colors'] = color_used
```

## 三、实际应用案例：神经发生过程分析

### 动态模型的优势
1. **准确的发育轨迹**：
   - 准确显示神经母细胞到颗粒细胞的分化路径
   - 正确识别CR细胞的终末状态
   - 准确描述OPC到OL的分化过程

2. **基因表达动态**：
   - 准确捕捉Tmsb10在神经元分化中的作用
   - 正确描述Fam155a在CR细胞中的表达模式

3. **驱动基因识别**：
   - 系统识别关键调控基因
   - 区分真实信号和噪音
   - 揭示细胞状态转换的分子机制

### 相比稳态模型的改进
- 避免了错误的速率分配
- 提供更准确的分化方向预测
- 能够捕捉复杂的细胞状态转换

## 建议
1. 对于复杂的分化过程，优先使用动态模型
2. 注意数据质量控制和预处理步骤
3. 结合生物学背景解释结果
4. 考虑使用多个时间点的数据验证预测
