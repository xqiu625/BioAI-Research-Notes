# RNA Velocity 分析指南

## 原理介绍

RNA velocity 是一种从单细胞RNA测序数据推测细胞命运和状态变化的计算方法。其核心原理基于以下观察：

1. **RNA剪接过程**
   - 基因从转录到形成成熟mRNA需要经过剪接过程
   - 细胞内同时存在未剪接的pre-mRNA和已剪接的成熟mRNA
   
2. **表达量变化的判断**
   - 基因表达量上升时：未剪接mRNA比例较高
   - 基因表达量下降时：已剪接mRNA比例较高
   - 通过比较这两种mRNA的比例，可以预测基因表达的变化趋势

3. **细胞状态预测**
   - 整合所有基因的变化趋势
   - 预测细胞的状态变化方向
   - 推测细胞群体的发育轨迹
   - 预测细胞的未来分化方向

## 数学模型

RNA velocity 使用以下参数进行计算：
- u：未剪接reads数量（unspliced reads）
- s：已剪接reads数量（spliced reads）
- γ：RNA降解和剪接速率的估计值

判断标准：
- 当 u > γs 时：基因处于转录激活状态
- 当 u < γs 时：基因处于转录抑制状态

## 实际操作流程

### 1. 安装 velocyto

```bash
# 安装依赖
# 需要：numpy, scipy, cython, numba, matplotlib, scikit-learn, h5py, click
pip3 install velocyto
```

### 2. 准备必要文件

1. **GTF注释文件**
   - 从GENCODE或Ensembl下载
   - 对于10x数据，可以从10x官网下载
   - 使用cellranger mkgtf处理：
   ```bash
   cellranger mkgtf input.gtf output.filtered.gtf \
     --attribute=gene_biotype:protein_coding \
     --attribute=gene_biotype:lincRNA \
     # 其他基因类型...
   ```

2. **重复序列掩码文件（mask.gtf）**
   - 人类和小鼠：从UCSC下载
   - 其他物种：使用RepeatMasker生成
   ```bash
   RepeatMasker -species "物种名" -dir RepeatMasker_output genome.fa
   ```

### 3. 运行分析

```bash
velocyto run10x \
  -m repeat_msk.gtf \
  mypath/sample01 \
  refdata-cellranger-mm10-1.2.0/genes/genes.gtf
```

- 处理时间：约3小时（取决于测序深度和计算资源）
- 输出文件：`velocyto/cellranger.loom`

### 4. loom文件处理

loom文件是基于HDF5格式的数据文件，可以：
- 合并多个样本
- 按需读取数据
- 支持多种编程语言处理

示例代码：
```python
# 合并多个loom文件
import loompy
files = ["file1.loom", "file2.loom", "file3.loom"]
ds = loompy.connect("merged.loom")
for fn in files[1:]:
    ds.add_loom(fn, batch_size=1000)
ds.close()
```

## 后续分析

RNA velocity分析可以帮助我们：
1. 理解细胞分化过程
2. 预测细胞命运决定
3. 研究基因表达动态变化
4. 构建细胞发育轨迹

通过这种分析，我们可以在静态的基因表达数据中加入时间维度，更好地理解细胞发育和分化过程。
