# FLOW-MAP: a graph-based, force-directed layout algorithm for trajectory mapping in single-cell time course datasets

Nature Protocols (2020 Jan)
University of Virginia 

paper link:
https://www.nature.com/articles/s41596-019-0246-3

github link: 
https://github.com/zunderlab/FLOWMAP/

## Summary
-  FLOW-MAP is a GUI-based software tool that uses graph layout analysis with sequential time ordering to visualize cellular trajectories in high-dimensional single-cell datasets obtained from flow cytometry, mass cytometry or single-cell RNA sequencing experiments.
- The main steps are: 1) data preprocessing within FLOW-MAP (downsampling, transforming data), 2) graph building between nodes from adjacent time points, and 3) graph visualization after iterative force-directed layout.
- It accommodates multiple FLOW-MAP modes for analyzing a single sample, multiple conditions at one time point, one time course, or multiple time courses.
- Key parameters that affect the output include the ratio of cells subsampled to number of clusters, edge density parameters, and choice of markers used for clustering.
- The output is a 2D graph representation showing patterns of change over time across multiple markers. Node size indicates number of cells, color indicates marker expression or metadata.
- It enables visualization of cellular trajectories, branchpoints, and marker expression patterns in dynamic processes like cell differentiation, without directly tracing individual cells.
- It can be applied to mass cytometry and single-cell RNA-seq data, with examples shown for mouse embryonic stem cell differentiation and hematopoiesis.

## Structure
Introduction

- Overview of FLOW-MAP algorithm and applications
- Comparison to other single-cell analysis methods
- Limitations

Overview of FLOW-MAP algorithm

- Data preprocessing
- FLOW-MAP graph building
- FLOW-MAP graph layout and visualization

Materials

- Equipment
- Software

Equipment setup

- FLOWMAPR installation

Procedure

1. Setting up files and specifying parameters for FLOW-MAP analysis
2. Running FLOW-MAP
3. Visualizing FLOW-MAP results

Anticipated results

- Specifying different FLOW-MAP settings
- Comparison of FLOW-MAP with other single-cell analysis methods
- FLOW-MAP analysis of mESC differentiation time course
    - Combined analysis of all conditions
    - Individual condition analysis
- Using FLOW-MAP to analyze scRNA-seq data

## Algorithm framework
![alt text](image.png)
The FLOW-MAP algorithm has three major stages: data preprocessing, including optional subsampling or density-dependent downsampling and clustering (Steps 1–3); graph building between nodes from adjacent time points, allotting edges in a density-dependent manner (Step 4); and graph visualization after iterative force-directed layout and postprocessing (Steps 5–9). Workflow and example outputs are shown for the four available modes: a, single time point, single condition; b, single-time point, multiple conditions; c, multiple time points, single condition; and d, multiple-time points, multiple conditions. The default input for FLOW-MAP is an FCS file, but the tool can be applied to other formats. Example FLOW-MAPs are shown on synthetic 2D datasets.

1. Data preprocessing within FLOW-MAP

- This stage involves preparing the input data for graph building.
- If necessary, the data is transformed using an Arcsinh transform.
- Downsampling methods can be applied to reduce the dataset size, including density-dependent downsampling to preserve rare cell populations, random subsampling, or hierarchical clustering.
- An overclustering approach is recommended when downsampling/clustering.

### Arcsinh transform
Arcsinh变换将正态分布的数据转换为具有更高偏度和峰度的分布,即sinh-arcsinh分布。这种变换对于处理具有重尾和偏斜特征的数据非常有用。
（偏度(Skewness)
偏度衡量数据分布的不对称性,描述数据相对于平均值的偏斜程度。
偏度=0,表示数据分布呈对称分布,如正态分布。
偏度>0,表示数据右偏(正偏),分布在平均值右侧有较长尾部。
偏度<0,表示数据左偏(负偏),分布在平均值左侧有较长尾部。
偏度的绝对值越大,表明数据分布偏离对称性的程度越高。
峰度(Kurtosis)
峰度描述数据分布曲线顶端的尖锐或扁平程度,反映数据分布相对于正态分布的陡峭程度。
峰度=0,表示数据分布峰度与正态分布相同。
峰度>0,表示数据分布峰部较高尖锐,为尖顶峰。
峰度<0,表示数据分布峰部较低扁平,为平顶峰。
峰度的绝对值越大,表明数据分布相对于正态分布的陡缓程度差异越大。）

