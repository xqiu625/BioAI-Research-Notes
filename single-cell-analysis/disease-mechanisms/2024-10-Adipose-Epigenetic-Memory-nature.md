# Adipose tissue retains an epigenetic memory of obesity after weight loss

## 📊 Paper Metadata
- **Title:** Adipose tissue retains an epigenetic memory of obesity after weight loss
- **Authors:** Laura C. Hinte, Daniel Castellano-Castillo, et al.
- **Publication:** Nature (2024)
- **Institution:** ETH Zurich (Lead)
- **Code/Data:** Data available at GEO (GSE236580)
- **Tags:** #obesity #epigenetics #adipose-tissue #weight-loss #cellular-memory

## 🎯 Core Contributions
1. Demonstrated that adipose tissue retains transcriptional changes after significant weight loss in both humans and mice
2. Identified persistent obesity-induced epigenetic alterations in mouse adipocytes that affect function and metabolic responses
3. Showed that epigenetic memory primes cells for pathological responses when re-exposed to obesogenic environments
4. Established a mechanistic link between epigenetic memory and the "yo-yo" effect in weight regain

## 📋 Paper Structure

### 1. Introduction
- Background: Weight loss maintenance is a major challenge in obesity treatment
- Problem: Molecular mechanisms behind weight regain tendencies are poorly understood
- Innovation: First comprehensive study of epigenetic memory in adipose tissue

### 2. Methods/Results
- Human Studies:
  - Analyzed adipose tissue samples before and after bariatric surgery
  - Used single-nucleus RNA sequencing to track cellular changes
  
- Mouse Studies:
  - Developed diet-induced obesity and weight loss model
  - Performed detailed epigenetic analysis using multiple techniques
  - Conducted functional studies of adipocyte behavior

### 3. Discussion
- Identified stable epigenetic changes that persist after weight loss
- Demonstrated functional consequences of epigenetic memory
- Suggested potential therapeutic implications for weight management

### 1. Adipocytes (Most Critical)
```markdown
Key Findings:
- Retained epigenetic memory after weight loss
- Showed persistent transcriptional changes
- Key altered pathways:
  * Downregulated: Adipocyte metabolism and function
  * Upregulated: Inflammatory and fibrosis pathways

Specific Changes:
- Altered genes: IGF1, LPIN1, IDH1, PDE3A (in omAT)
- Changed genes: IGF1, DUSP1, GPX3, GLUL (in scAT)
```

### 2. Macrophages (Major Immune Component)
```markdown
Subtypes involved:
1. Lipid-Associated Macrophages (LAMs)
   - Increased during obesity
   - Remained elevated after weight loss
   - Key indicator of tissue inflammation

2. Non-perivascular Macrophages
   - Showed persistent population changes

3. Perivascular Macrophages
   - Altered distribution in tissue

Changes:
- Increased infiltration in obese state
- Persistent presence after weight loss
- Altered inflammatory signaling
```

### 3. Adipocyte Progenitor Cells (APCs)
```markdown
Significance:
- Showed strong transcriptional deregulation
- Impaired adipogenesis capacity after weight loss
- Contributed to tissue dysfunction

Changes:
- Altered differentiation potential
- Modified response to metabolic stimuli
```

### 4. Endothelial Cells
```markdown
Relevance:
- Strong retention of transcriptional differences
- Involved in tissue remodeling

Subtypes affected:
- Arteriolar Endothelial Cells
- Stalk Endothelial Cells
- Venular Endothelial Cells
```

### 5. Immune Cell Populations
```markdown
Key players:
1. T cells
2. B cells
3. Dendritic cells
4. Mast cells

Role:
- Contributed to inflammatory state
- Showed persistent changes after weight loss
```

### Disease Mechanism Summary
```markdown
1. Primary Mechanisms:
   - Epigenetic memory in adipocytes
   - Persistent inflammation
   - Altered tissue remodeling
   - Modified metabolic responses

2. Cell Type Interactions:
   - Adipocyte-macrophage crosstalk
   - Immune cell infiltration
   - Vascular remodeling

3. Functional Consequences:
   - Accelerated weight regain
   - Impaired metabolic function
   - Enhanced inflammatory response
```

### Clinical Relevance
```markdown
Most Critical Cell Types for Intervention:
1. Adipocytes: Primary therapeutic target
2. LAMs: Inflammatory mediators
3. APCs: Tissue regeneration potential

Potential Therapeutic Approaches:
- Targeting epigenetic modifications
- Modulating immune response
- Improving adipocyte function
```


## 🔬 Technical Details

### Key Experimental Approaches
1. Single-nucleus RNA sequencing
2. TRAP-seq for translatome analysis
3. CUT&Tag for histone modifications
4. ATAC-seq for chromatin accessibility
5. Integration of multi-modal data using MOFA

### Implementation Details
- Computing Resources:
  - R packages: Seurat, EdgeR, chromVAR
  - Bioinformatics tools: Cell Ranger, ChromHMM
  - Statistical analysis frameworks

Here are the key bioinformatic analyses performed in this study:

### 1. Single-nucleus RNA Sequencing (snRNA-seq) Analysis
```markdown
- Pipeline: 10x Genomics Cell Ranger (v6.1.2 and v7.2.0)
- Quality Control:
  * Used scDblFinder to remove doublets
  * Filtered nuclei based on feature counts and UMI counts
  * Removed highly expressed genes (mitochondrial, pseudogenes, Malat1)
  * Used SoupX for ambient RNA contamination estimation

- Data Processing:
  * Seurat v4.1.0 for data integration and analysis
  * Normalization using sctransform
  * Integration using CCA (canonical correlation analysis)
  * Clustering using Louvain algorithm
  * Cell type annotation based on known markers
  * Reference mapping against published datasets

- Differential Expression Analysis:
  * Used Wilcoxon rank-sum test
  * Criteria: |log2FC| > 0.5 and adjusted P < 0.01
  * Validated using MAST and likelihood-ratio tests
```

### 2. Multi-modal Data Integration
```markdown
- MOFA (Multi-Omics Factor Analysis):
  * Used for unsupervised integration of multi-omic datasets
  * Included all data modalities across conditions
  * Selected top 3,000 variable features per modality
  * Identified five latent factors explaining data variability
```

### 3. Epigenomic Analyses
```markdown
A. CUT&Tag Analysis:
  * Peak calling using SEACR v1.3
  * Peak annotation using ChIPSeeker
  * Filtered peaks overlapping blacklist regions
  * Differential analysis using EdgeR

B. ATAC-seq Analysis:
  * Quality control and bedgraph generation
  * Peak calling using SEACR
  * Integration with other modalities
  * Chromatin accessibility analysis

C. ChromHMM Analysis:
  * Used for chromatin state identification
  * Combined all histone modifications and ATAC-seq data
  * Eight chromatin states model
  * Enhancer state identification
```

### 4. TRAP-seq Analysis (Translatome)
```markdown
- Quality control using FastQC
- Trimming using TrimGalore
- Alignment using HISAT2 to mm10 reference
- Quantification using featureCounts
- Differential expression using EdgeR
  * Criteria: |log2FC| ≥ 1 and nominal P < 0.01
```

### 5. Integration and Annotation Analyses
```markdown
- Gene Set Enrichment Analysis (GSEA):
  * Used enrichR package
  * Multiple databases including WikiPathways
  * Adjusted P-value < 0.01 threshold

- SNP-based Demultiplexing:
  * Used cellsnp-lite for SNP calling
  * Vireo for demultiplexing pooled data
  * 1000 Genomes-based reference for hg38
```

### 6. Visualization Tools
```markdown
- R packages:
  * SCpubr for snRNA-seq visualization
  * Seqmonk for visual QC
  * GraphPad Prism
  * Basic R plotting

- Other Tools:
  * Affinity Designer
  * Affinity Publisher
```

### 7. Statistical Analysis
```markdown
- Multiple testing correction:
  * Bonferroni method
  * Benjamini-Hochberg method
- Various statistical tests:
  * Wilcoxon rank-sum test
  * Mann-Whitney tests
  * t-tests with multiple testing corrections
```

### 8. Computing Resources and Platforms
```markdown
- Languages: R v4.2
- Key R Packages:
  * Seurat
  * EdgeR
  * chromVAR
  * ChIPSeeker
  * enrichR
- Sequencing Platforms:
  * NovaSeq 6000
  * NovaSeqX
```



## 📊 Evaluation
### Datasets
1. Human Studies:
   - MTSS cohort
   - LTSS cohort
   - NEFA trial samples

2. Mouse Studies:
   - Diet-induced obesity model
   - Weight loss intervention groups

## 💭 Critical Analysis

### Strengths
1. Comprehensive multi-modal analysis
2. Validation in both human and mouse models
3. Clear mechanistic insights
4. Clinical relevance

### Limitations
1. Human samples limited to bariatric surgery patients
2. Focus primarily on adipose tissue
3. Long-term effects not fully explored

## 📌 Key Takeaways
1. Obesity induces stable epigenetic changes that persist after weight loss
2. These changes affect cellular function and response to metabolic stimuli
3. Epigenetic memory may contribute to weight regain difficulties
4. Findings suggest new therapeutic approaches for weight management

## 💡 Personal Notes
## 1. How inflammation was characterized?

### 1. Cellular Level Indicators
```markdown
1. Immune Cell Infiltration:
- Increased macrophage populations, especially:
  * Lipid-Associated Macrophages (LAMs)
  * Non-perivascular macrophages
- Changes in immune cell composition:
  * T cells
  * B cells
  * Dendritic cells
  * Mast cells

2. Tissue Changes:
- Apical fibrosis in adipose tissue
- Collagen deposition (shown by Masson's trichrome staining)
```

### 2. Molecular Signatures
```markdown
1. Upregulated Inflammatory Pathways:
- TGFβ signaling
- Chemokine signaling
- IL-3 signaling pathway
- IL-6 signaling pathway
- TYROBP causal network

2. Key Inflammatory Genes:
- ICAM1 
- Lyz2
- Tyrobp
- CD74
- Ctss
```

### 3. Epigenetic Changes
```markdown
1. Active Marks (H3K4me3/H3K27ac) on:
- Inflammatory pathway genes
- Extracellular matrix remodeling genes

2. Persistent Enhancer Activation:
- Related to inflammatory signaling
- Associated with lysosome activity
```

### 4. Gene Set Enrichment Analysis
```markdown
Enriched Pathways:
1. In Weight Loss State:
- Chemokine signaling
- Inflammatory processes
- Fibrosis-related pathways

2. Retained After Weight Loss:
- Inflammatory pathways
- Lysosome activity
- Matrix remodeling
```


