

文章链接
https://www.nature.com/articles/s41587-020-0591-3 

doc链接
https://scvelo.readthedocs.io/en/stable/

中文操作笔记 
https://www.jianshu.com/p/fb1cf5806912

完整操作笔记 
https://github.com/basilkhuder/Seurat-to-RNA-Velocity?tab=readme-ov-file 

```python

import scvelo as scv
import scanpy as sc
#import cellrank as cr
import numpy as np
import pandas as pd
import anndata as ad

import matplotlib.pyplot as plt

scv.settings.verbosity = 3
scv.settings.set_figure_params('scvelo', facecolor='white', dpi=100, 
                               fontsize=6, color_map='viridis',
                               frameon=False)

#cr.settings.verbosity = 2
#adata = sc.read_h5ad('my_data.h5ad')
#adata=sc.read_loom(f'out/{type}.my_data.loom')

#adata.obsm['X_pca'] = pca.to_numpy()
#adata.obsm['X_umap'] = np.vstack((adata.obs['UMAP_1'].to_numpy(), adata.obs['UMAP_2'].to_numpy())).T

#adata

#这里展示了多个样本的处理方式。

#load loom files for spliced/unspliced matrices for each sample:
sample_loom_1="./velocyto/WT1/possorted_genome_bam_HA9DP.loom"
ldata1 = scv.read( sample_loom_1, cache=True)
ldata1

ldata1.obs.index[0:2]
#WT_1 -1_1
barcodes = [bc.split(':')[1] for bc in ldata1.obs.index.tolist()]
barcodes = [bc[0:len(bc)-1] + '-1_1' for bc in barcodes]
ldata1.obs.index = barcodes
ldata1.obs.index[0:5]

#WT2 -1_2
barcodes = [bc.split(':')[1] for bc in ldata2.obs.index.tolist()]
barcodes = [bc[0:len(bc)-1] + '-1_2' for bc in barcodes]
ldata2.obs.index = barcodes
ldata2.obs.index[0:5]

ldata1.var.head()


ldata1.var_names_make_unique()
ldata2.var_names_make_unique()

#merge
ldata = ldata1.concatenate([ldata2])
ldata

#merge matrices into the original adata object
adata = scv.utils.merge(adata, ldata)
adata

保存数据
adata.write_h5ad(f'data/{type}.adata_ldata.h5ad')


scVelo 预处理
adata = sc.read(f'data/{type}.adata_ldata.h5ad')
adata

#scv.pp.filter_and_normalize(adata, min_shared_counts=5, min_shared_cells=3, log=True)
scv.pp.filter_and_normalize(adata)

可选步骤:

##clean some genes
import re
flag = [not bool(re.match('^Rp[ls]', i)) for i in adata.var_names]
adata = adata[:,flag]
adata = adata[:,adata.var_names != "Malat1"]
adata

scVelo
scv.pp.moments(adata, n_neighbors=30, n_pcs=30)
#this step will take a long while
import gc
gc.collect()

temp_pre= f"{type}_nue.in_process2"
if False==os.path.exists(f"{temp_pre}.velo.gz.h5ad"):
    scv.tl.recover_dynamics(adata, var_names='all', n_jobs=64)
    scv.tl.velocity(adata, mode='dynamical')
    adata.write(f"{temp_pre}.velo.gz.h5ad", compression='gzip')
    print(">>Write to file")
else:
    adata = sc.read(f"{temp_pre}.velo.gz.h5ad", compression='gzip', ext="h5ad")
    print(">>read from file")

scv.tl.velocity_graph(adata)
scv.tl.velocity_embedding(adata, basis="umap")

可视化
##add colSet
adata.obs["cluster_names"] = adata.obs["cluster_names"].astype('category')

color = pd.read_csv(f"../merge/data/mergeR3.neutrophil.color.txt", #compression="gzip", 
            sep=" ", header=0, index_col=0)
#color.index=color.index.astype("str")
color.index=color["cluster_names"]

#对数据 按分类因子 排序
adata.obs["cluster_names"].cat.reorder_categories(color["cluster_names"], inplace=True)

#获取颜色列
color_used = list(color.loc[adata.obs["cluster_names"].cat.categories,"color"])
adata.uns['cluster_names_colors'] = color_used
adata

```

![Resolving subpopulation kinetics and identifying dynamical genes in neurogenesis.](image.png)

牙龈神经发生过程中使用动态模型和稳态模型来估计RNA速率的比较。

1. 动态模型更准确：在UMAP图上，动态模型准确地显示了神经母细胞发育为颗粒细胞的主要流向，并正确识别了其他细胞类型的状态，如终末状态的CR细胞和从OPC分化为OL的过程。
2. 稳态模型的局限性：相比之下，稳态模型错误地为CR细胞分配了高速率，并错误地指示OPC远离OL，这表明它在捕捉细胞状态变化时不如动态模型准确。
3. 基因层面的洞察：动态模型还能在基因层面提供更准确的信息。例如，它正确地识别了Tmsb10基因在神经母细胞分化为颗粒细胞过程中的关键作用，并准确地描述了Fam155a基因在CR细胞中的表达状态。
4. 识别驱动基因：动态模型的一个独特优势是能够系统地识别可能的驱动基因，这些基因表现出明显的动态行为，而不是由噪音或不存在的瞬时状态所主导。

# Generalizing RNA velocity to transient cell states through dynamical modeling

RNA速率是一种创新的技术，用于通过测量未成熟和成熟的mRNA水平来预测细胞的未来状态。随着时间的推移，这项技术不断发展，以更好第捕捉细胞发育过程中的动态变化。
1. 稳态/确定性模型（最简单）：这是最早的RNA速率模型，假设细胞处于稳定状态，就像一条平稳流动的河流。它认为，如果你观察河流足够长的时间，你就能预测水流的速度和方向。同样，这个模型假设细胞的基因表达已经达到平衡，通过观察现有的mRNA水平，可以估计转录和剪接的速率。但是，就像河流可能突然变快或改变方向一样，细胞也可能快速改变状态。这时这个简单的模型就可能出错。
2. 随机模型（稍微复杂）：这个模型可以理解为看起来平静的河流，水流速度和方向也会有小的变化。同样，即使细胞似乎处于稳定状态，基因表达也会有自然的波动。随机模型不仅看平均水平（河流的平均速度），还看波动的程度（水流速度的变化）。这有助于更好地估计RNA速率，但仍然假设细胞处于相对稳定的状态。
3. 动态模型（最复杂但最准确）：想象一条河流在山区，有急流、瀑布和平缓的部分。这条河流的的动态非常复杂，每个部分都有独特的流动方式。类似地，细胞发育是一个动态的过程，每个基因都有独特的转录和剪接模式，特别是在细胞改变状态（如从干细胞到特定类型的细胞）时。动态模型是专门设计来处理这种复杂性的。它不假设所有基因都以相同的速率被剪接，也不假设细胞处于稳定状态。相反，它仔细研究每个基因的独特性为，就像研究河流的每个部分一样。它还考虑到细胞可能处于过度状态，就像水流从一个部分移到另一个部分。通过这种方式，动态模型可以更准确地预测细胞的未来状态，即使是在快速变化的情况下。更重要的是，通过详细研究每个基因的行为，这个模型可以找出哪些基因在驱动变化。就像识别哪些急流或瀑布导致河流的整体流动模式一样，它可以识别哪些基因正在推动细胞从一种状态转变到另一种状态。
