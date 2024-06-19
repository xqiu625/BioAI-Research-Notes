![alt text](image-6.png)

a Illustration of transcriptional process which involves transcription rate α, splicing rate β, and degradation rate γ. Green dotted line indicates that parameters are inferred reversely. b Paradigm of the model in a with time as independent variable, showing predicted changes of α with regards to measured expression profile. c RBF for modeling patterns of induction, repression, and transient dynamics of each gene, where τg represents the peak time. Latent time of this model is rescaled and truncated between 0 and 1. d Example of phase portraits (can be splitted to induction and repression stages from the middle, shown in black and red arrows respectively) of two dynamical genes modeled by both scVelo and UniTVelo, Tmsb10 and Ppp3ca, showing RBF kernel has a similar ability to recover gene’s dynamic information. Colors indicate various cell types. e The inference of UniTVelo which tries to recover genes’ dynamic process via two sets of parameters: (1) gene-specific parameters θg which define how transcriptome of each gene changes along time and the relationship between un/spliced mRNA in progression. (2) cell-specific time points tng. By iteratively updating the gene-specific parameters using gradient descent, cell time points are assigned by minimizing the euclidean distance to phase trajectory. Specifically, besides directly using gene-specific time matrix in optimization, UniTVelo also supports a unified-time assignment for each cell based on cell ordering. This enables the discrepancy between genes’ directionality to be minimized.


https://www.nature.com/articles/s41467-022-34188-7 


https://github.com/StatBiomed/UniTVelo

nature communications (2022 Nov), University of Hong Kong