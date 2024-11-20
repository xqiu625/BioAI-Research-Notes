https://github.com/DawnEve/txtBlog/blob/3ddf465df0f608ea43497bce6985cbe24d57654f/data/scSeq/RNA_Velocity_note.txt#L437 

文章链接:
https://www.nature.com/articles/s41586-018-0414-6

doc链接：
https://velocyto.org/velocyto.py/index.html 


RNA velocity是一种从单细胞RNA测序数据推测细胞命运和状态变化的计算方法。它的主要原理可以通俗地总结如下:

1. 一个基因从被转录到成熟mRNA需要一些时间。在这个过程中,细胞内会同时存在未剪接的pre-mRNA和已剪接的成熟mRNA。
2. 如果一个基因的表达量正在升高,那么未剪接的mRNA相对已剪接的mRNA会更多一些;反之如果基因表达量在下降,已剪接mRNA会相对更多。
3. 通过比较每个基因未剪接和已剪接的mRNA比例,我们可以知道这个基因表达是在升高还是降低。把所有基因的变化趋势综合起来,就可以
测这个细胞的状态是在向什么方向改变。
4. 把每个细胞的状态变化方向展示在一起,我们就可以推测细胞群体的发育轨迹,判断一个细胞将来会分化成什么细胞类型。
RNA velocity利用了RNA剪接动力学的信息,相当于在静态的基因表达数据中加入了时间维度,可以用来研究细胞命运决定、分化等动态过程。

![Alt text](../../paper-figures/Pseudotime_expression_profiles.png)

Principle-curve pseudotime: 是一种用于分析单细胞RNA测序数据的计算方法，用于推断细胞在一个生物学过程中的进展程度或“时间”。原理如下：

1. 在一个发育或分化过程中，细胞会经历一系列的状态变化，但是不同细胞的进展速度可能不同。我们希望知道每个细胞在这个过程中的相对位置和“时间”；
2. 单细胞RNA测序数据提供了每个细胞基因标的的高纬度快照。我们可以假设，随着发育过程的进行，细胞会在这个高维表达空间中移动，形成一条轨迹；
3. Principal curve是一种数学方法，它可以在数据点形成的分布中找到一条“中心线”。如果我们把细胞在基因表达空间中的轨迹看作一条曲线，主曲线就近似这个轨迹；
4. 我们可以把每个细胞投影到主曲线上最近的点，然后以这个点在曲线上的位置（即曲线起点到这个点的距离）作为这个细胞的pseudotime；
5. pseudotime 反映了细胞在发育轨迹上的相对位置，可以用来推断细胞的发育阶段，寻找关键的转变点，分析不同基因表达的动态变化等。

原理：
u 代表 unspliced reads，s 代表 spliced reads, γ 代表 RNA降解和剪接速率的估计，虚线是拟合出来的平衡态。\
一般认为当 u> γs 时基因处于转录激活态，反之为抑制。这样，对于每个基因，拟合出 γ 后，就可以根据平衡态预测速率；再通过基因预测结果综合判断细胞的速率；最后计算细胞见速率相关性，最终完成轨迹推断。

# velocyto run 生成 loom 文件 
velocyto 命令行工具具有直接从 cellranger 输出目录运行的功能，只需要提供 .bam 文件，也可以在任何单细胞平台上使用。
1. 安装
依赖: numpy scipy cython numba matplotlib scikit-learn h5py click
$ pip3 install velocyto  #0.17.17

2. Tutorial
(1) 该工具包含2部分：
Velocyto consists of two main components:
1) 命令行工具
A command line interface (CLI), that is used to run the pipeline that generates spliced/unspliced expression matrices.
2) 一个函数库
A library including functions to estimate RNA velocity from the above mentioned data matrices.

3. 准备文件 
1)下载 GENCODE or Ensembl 的 gtf 文件。
如果是 10x 的数据，从这里下载: https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references

命令格式: cellranger mkgtf input.gtf output.gtf --attribute=key:allowable_value

cellranger mkgtf Homo_sapiens.GRCh38.ensembl.gtf Homo_sapiens.GRCh38.ensembl.filtered.gtf \
                   --attribute=gene_biotype:protein_coding \
                   --attribute=gene_biotype:lincRNA \
                   --attribute=gene_biotype:antisense \
                   --attribute=gene_biotype:IG_LV_gene \
                   --attribute=gene_biotype:IG_V_gene \
                   --attribute=gene_biotype:IG_V_pseudogene \
                   --attribute=gene_biotype:IG_D_gene \
                   --attribute=gene_biotype:IG_J_gene \
                   --attribute=gene_biotype:IG_J_pseudogene \
                   --attribute=gene_biotype:IG_C_gene \
                   --attribute=gene_biotype:IG_C_pseudogene \
                   --attribute=gene_biotype:TR_V_gene \
                   --attribute=gene_biotype:TR_V_pseudogene \
                   --attribute=gene_biotype:TR_D_gene \
                   --attribute=gene_biotype:TR_J_gene \
                   --attribute=gene_biotype:TR_J_pseudogene \
                   --attribute=gene_biotype:TR_C_gene

多物种
cellranger mkref --genome=GRCh38 --fasta=GRCh38.fa --genes=GRCh38-filtered-ensembl.gtf \
                 --genome=mm10 --fasta=mm10.fa --genes=mm10-filtered-ensembl.gtf

2) 下载 mask.gtf 文件
human 和 mice可以 在 UCSC https://genome.ucsc.edu/cgi-bin/hgTables?hgsid=611454127_NtvlaW6xBSIRYJEBI0iRDEWisITa&clade=mammal&org=Mouse&db=mm10&hgta_group=allTracks&hgta_track=rmsk&hgta_table=0&hgta_regionType=genome&position=chr12%3A56694976-56714605&hgta_outputType=primaryTable&hgta_outputType=gff&hgta_outFileName=mm10_rmsk.gtf

其他物种可以用RepeatMasker
module load RepeatMasker/4-1-5
RepeatMasker -species "Toxoplasma gondii"  -dir RepeatMasker_output fasta/genome.fa

repeat_msk.gtf 原理
repeat_msk.gtf file contains annotations of repetitive elements and masked genomic regions in relation to the original genome assembly.
Masking repetitive sequences helps improve the accuracy and quality of the genome assembly by removing low-complexity regions, simple repeats, transposable elements, and other repetitive sequences that can complicate assembly and annotation.
Properly accounting for masked repeats is important for obtaining accurate estimates of RNA velocity, as reads mapping to repetitive regions could introduce biases or noise in the analysis.

4. 运行 10x 数据，生成 loom 文件(推荐使用 run 子命令)
$ velocyto run10x -m repeat_msk.gtf mypath/sample01 somepath/refdata-cellranger-mm10-1.2.0/genes/genes.gtf
参数说明:
Where genes.gtf is the genome annotation file provided with the cellranger pipeline.  
repeat_msk.gtf is the repeat masker file described in the Preparation section above. 可选。
(就是重复区域不要。)

通常一个样品运行3个小时。

Execution time is ~3h for a typical sample but might vary significantly by sequencing depth and cpu power.

输出:

$ ls *
outs/
	cellsorted_possorted_genome_bam.bam
velocyto/
	cellranger.loom #后续使用R包分析，就是基于该文件。


## loom 中间文件的使用 

(1) A valid .loom file is simply an HDF5 file that contains specific groups representing the main matrix as well as row and column attributes. Because of this, .loom files can be created and read by any language that supports HDF5.

(2) Merging multiple samples/lanes in a single file
loompy.combine(files, output_filename, key="Accession")
或者:

files = ["file1.loom","file2.loom","file3.loom","file4.loom"]
# on the command line do: cp file1.loom merged.loom
ds = loompy.connect("merged.loom")
for fn in files[1:]:
    ds.add_loom(fn, batch_size=1000)

(3) HDF5 的连接就像数据库一样，不是完全载入，只是在必须的时刻载入需要的数据，也可以修改、写入。
ds = loompy.connect("filename.loom")
# do something with the connection object ds
ds.close()
