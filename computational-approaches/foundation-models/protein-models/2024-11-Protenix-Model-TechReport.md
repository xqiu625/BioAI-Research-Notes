## 📊 Paper Metadata
- **Title**: Modeling Cellular Perturbations with the Sparse Additive Mechanism Shift Variational Autoencoder (Protenix)
- **Authors**: ByteDance AML AI4Science Team
- **Publication**: Technical Report (November 9, 2024)
- **Institution**: ByteDance
- **Code**: https://github.com/bytedance/Protenix

## 🎯 Core Contributions
1. Open-source reproduction of AlphaFold 3 (AF3) with comparable performance
2. Complete training pipeline and code release for research community
3. Corrections and clarifications to AF3's technical details
4. Comprehensive evaluation framework for protein-ligand/protein/nucleic acid structure prediction

## 📋 Paper Structure

### 1. Introduction
- Background: Need for accessible protein structure prediction tools
- Problem: Limited accessibility of AF3 and lack of complete implementation details
- Innovation: Full open-source reproduction with clarified methodology

### 2. Results/Methods
- Model Architecture
  - Reproduces AF3's key components
  - Implements sparse additive mechanism shifts
  - Incorporates corrected algorithms and training procedures
- Performance validation on multiple datasets:
  - PoseBusters V2 for protein-ligand interactions
  - Low Homology Recent PDB Set for protein interfaces
  - CASP15 for RNA structure prediction

### 3. Discussion
- Achieves comparable or better performance than AF3 in many cases
- Identifies and corrects several implementation details
- Provides insights into model behavior and limitations

## 🔬 Technical Details

### Model Components
1. Data Pipeline:
   - MSA generation using MMSEQS2 and ColabFold
   - Custom preprocessing and featurization
   - Improved cropping strategies

2. Training Framework:
   - Multi-stage training approach
   - BF16 mixed precision optimization
   - Custom CUDA kernels for efficiency

3. Key Improvements:
   - Corrected algorithms from AF3 paper
   - Enhanced confidence head architecture
   - Optimized parameter initialization

## 📊 Evaluation

### Baseline Models
- AlphaFold 3 
- AlphaFold-Multimer 2.3
- RoseTTAFold2NA

### Key Metrics
1. RMSD and PB-Valid scores for protein-ligand prediction
2. DockQ scores for protein interfaces
3. LDDT scores for nucleic acid structure prediction

### Datasets
1. PoseBusters V2 (308 structures)
2. Low Homology Recent PDB Set
3. CASP15 RNA targets

## 💭 Critical Analysis

### Strengths
1. Complete open-source implementation
2. Strong performance across different molecular types
3. Detailed technical clarifications and corrections
4. Comprehensive evaluation framework

### Limitations
1. Requires significant computational resources
2. Some performance gaps in specific cases
3. Limited confidence head training

### Future Directions
1. Continuous model improvements
2. Additional feature development
3. Enhanced evaluation tools

## 📌 Key Takeaways
1. First complete open-source reproduction of AF3
2. Achieves competitive performance across different tasks
3. Provides valuable insights into implementation details
4. Makes advanced protein structure prediction more accessible

## 💡 Personal Notes
A. evaluation workflow for assessing predictions
  1. Inference and Sampling
    - Generate 25 samples using 5 model seeds
    - Export each sample in two formats:
      * CIF file for structure
      * JSON file containing confidence ranking scores
  - Use different prediction targets based on dataset:
      * PoseBusters V2: predict asymmetric unit
      * Other datasets: predict first bioassembly
  - Apply 10 recycles generally (matches AF3's configuration)

  2. Chain and Atom Permutation
    - Perform chain mapping to establish one-to-one correspondence between:
      * Predicted entities
      * True entities
    - After identifying equivalent chains:
      * Permute the chains
      * Permute the atoms
      * Align prediction with true structure

  3. Metrics Computation
    - Use specific methods for different interaction types:
        * For proteins: DockQ Python package for DockQ scores
        * For PoseBusters V2 predictions:
          a. Export structures:
               - Ligand of interest → SDF file
               - Chains within 5Å of predicted ligand → PDB file
          b. Analysis:
               - Use PoseBusters Python package v.0.2.7
               - Calculate ligand RMSD
               - Evaluate structural violations
