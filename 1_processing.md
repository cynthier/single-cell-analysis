# Data processing

### **1. Sequencing depth**
 it affects the UMI count and the number of genes detected in the cells. It should be cautious when comparing the expression levels between samples.


#### <u> What can be done? (scRNA-seq data normalization) </u>
1. The **NormalizedData** in seurat will correct the sequencing depth in cells (UMI divided by the total count of UMI in each cell -> log2 scaled -> multiply by the common size factor) <br>
2. After using the **SCTransform** in the seurat, the PrepSCTFindMarkers could be used to identified the markers based on the normalized expression data (the SCT will remove the sequencing batch effect using the minimum median total UMI count to normalise the data), tutorial reference.<br>
3. Use the tool **SCnorm** to determine size factors in different groups/batches. 
4. **Cellranger aggre** normalization (seldom used in the study)
5. The CPM (not far for the lower-expression genes) and LSF (Lun Sum Factors) and the normalized data in scater. 

***Actually, few studies will correct the sequencing depth between samples.*** <br>
<br>


### **2. QC metrics** 
Quality control will define <u>mitochondrial, ribosomal and hemoglobin gene </u> and calculate the score for quality control. <br>

Mitochondrial genes associated with cell degradation. Since the number of counts and gene per cell is sufficiently high, when the percentage of mitochondrial reads for most cells is **below 20 %**, we can still process the data. <br>

The threshold for quality control can be determined by the MAD(median absolute deviations), like lower than the **mean+3MAD**. <br>
<u>UMI</u>: over 500, the average value in the Seurat PBMC data is around 2500 <br>
<u>nFeatures</u>: in PBMC tutorial is around 1000, the filter condition is 2500 > nFeature > 200 <br>


## **3. data normalization**
normalization is to adjust the raw counts in the datasets for variable sampling effects.

<u>1. shifted algorithm transformation</u><br>
utilizing the algorithm like log2 transformation, helpful for dimension reduction and DEGs identifications. <br>

<u>2. scran normalization (delta method)</u><br>
estimate the scaling factors using linear model similar to the log2 transformation for pool cells, but finally deconvolve the scaling factors to each cell. helpful for batch correction. <br>

<u>3. analytic approximation of Pearson residuals </u><br>
it can add the count depth as a covariate and remove the impact of sampling effects.

## **4. Feature selection**
Informative genes only account for a small fraction of genes identified. <br>

 Traditional approaches based on average expression or coefficient variation (**sd/u**) of genes to obtained 500~3000 highly variable genes for down-stream analysis. Actually, the normalization methods affect the gene expression variance -> selection of high variable genes. 
 
 Therefore,  Germain et al., 2020 propose **a deviance method (scry)** which works on raw count (the genes with constant expression are not informative). This method ranks the genes and obtains only the highly deviant genes.

1. 离散度（方差/平均值）(seurat)
2. 方差稳定变换 (seurat v3)
3. 基因皮尔森近似残差 (pearson_residuals)

## **5. Dimension reduction**
higher dimension data contained noise and redundancy even after features selection, therefore the dimensional algorithms are used to reduce the data complexity and for visualization, which still captures the underlying structures of the data.

The most popular dimensional algorithms are **UMAP, TSNE and PCA**.

<u>PCA(linear dimensionality reduction)</u>: the genes are ranked by the variance and comprise the PCs. Actually, the PCs is the linear combination of genes. Via an orthogonal transformations, PCA creates a new set of uncorrelated variables. it is highly interpretable and computationally efficient. 

scRNA data is highly non-linear, visualization with the linear dimensionality reduction technique PCA is not very appropriate. PCA is typically used to select the top 10-50 PCs which are used for downstream analysis tasks.


## **6. Clustering**
Before performing the cell identify annotation, the cells are structured into cluster to infer the identity of similar cells. In the PC-reduced expression space, the **KNN algorithm** is used to connect the cells where the distance between cells are Euclidean distance. Then the KNN reflects the topology of expression data. The dense regions of the KNN graph are detected by the community detection methods like Leiden and Louvain. (Leiden is the improved version of Louvain, which is no longer maintained). The resolution parameter determine the scale of partition cluster, which can also used to perform the sub-clustering.

<u>In the lower dimensions like UMAP 2D, distances are not necessarily captured well between all points. it is not recommendded to interpretate distances between clusters visualized on UMAP embeddings.</u>


**SCCAF**: auto clustering for the best classification using machine learning. (recommended) <br>
**ROC curve** can be used to assess the marker genes for the cell type classification

## **7. marker identification**
new method base on the max-min scale expression of selected marker genes. [ref](https://pubmed.ncbi.nlm.nih.gov/35705694/)



## **8. Annotation**
cell annotation is based on the known and unknown cellular phenotype. cell types are the cellular phenotype robust across datasets, which is identifiable based on the expression of markers (gene and proteins) and often linked to the specific functions. multiple cell types could be a single continuum where one cell type might transition or differentiate into another. When annotating based on markers, we should know that sometimes markers in one dataset do not turn out to work as well in other datasets, due to the sequencing depth or the variance of datasets and samples.

the markers are often sparsely expressed in the cell types, due to the nature of scRNA-seq data <u>(we only sequence a small subset of the total amount of RNA molecules in the cell and sometimes not sample the transcripts of the markers even when they were expressed)</u>. Therefore we do not annotate cells based on the threshold of marker gene expression. Instead, we perform clustering to group cells with high transcriptomic similarity.

### <u>methods for annotation</u>

1. manual annotation based on known markers after 

2. calculate DEGs and link them to the biology, Wilcoxon rank-sum test perform best for DEGs identification

3. employed MatchSCore2, CellTypist (based on several thousand genes) to annotate and classify cell types.

4. label transfer using SingleCellNet, scArches and the reference profile, which ignore the unseen cell types and stated in the new data.

5. pathways filtered markers, especially for the data contained unknown cell types or intermediate cell states.

6. module scoring using marker list

7. integration and transfer labels

8. GSEA (overcome the technical effect and it cross the species)

## **9. Integration**
1. **global model**: original from the bulk RNA data and model the batch effect across all cells, like ComBat.
2. **linear embedding models**: 
    1. use a singular value decomposition (SVD) to embed data → 
    2. identify the local neighbors of similar cells across batches → 
    3. correct batch effect in a locally adaptive(non-linear) manner → 
    4. project the data back into gene expression space using SVD loadings → 
    5. return corrected embedding. <br>
    **<u>(MNN (not perform dimension reductions), Seurat integration, Scanoama, FastMNN, and Harmony)</u>**
3. **Graph-based methods**: the approach will use a nearest-neighbor graph to represent the data from each batch → correcting batch by forcing connections between cells from different batches. BBKNN
4. **Deep learning models**：most packages are based on the autoencoder networks. scVI, scANVI, scGen

when select the features for annotation, the features should be batch-ware. That means the features should be selected from each batch. <u>scIB, batchbench can be used to evaluate the integration performance.</u>

### <u>10x genomic data and Smart-seq integration</u>
1. CCA.[ref](https://pubmed.ncbi.nlm.nih.gov/39574011/)
2. new method to integrate the sequencing data from different platforms.[ref](https://pubmed.ncbi.nlm.nih.gov/39574011/)
3. modified integration based on Harmony. [ref](https://pubmed.ncbi.nlm.nih.gov/31675496/)
4. scanorama integration

## <u>10. Differentiation and trajectories</u>
1. psupertime: supervised pseudotime approach based on a regression model, which explicitly uses time-series labels as input. It identifies genes that vary coherently along a time series

2. Cellrank

3. CellOracle

4. Slingshot

5. tradeSeq


## **Others**
### **1. Integration of modularity and species** 
1. Liger used iNMF (different modularity, species)

2. Seurat CCA integration

3. ComBat implemented in the “sva” R package, with default parameters on the log2(TP10K+1) expression matrix. They have used CCA and KNN before, but but did not obtain any enhancement in performance.

### **2. Compare the dataset or cell cluster using the Spearman correlation**
**1. Spearman correlation**: gene count matrixes → seurat integration identify the top 100 anchor feature→ scaled and centered the top 100 anchor features in the integrated object→ average the expression in each clusters<br>
**2. GSEA hierarchical clustering** (Majd et al. 2024): the average expression of cell type or samples -> caldogram<br>
**3. module scores**: idetify the DEGs in each cell types in reference data -> remove the genes that are not in the query data -> top 100 DEGs sorted by FDR -> calculated the module scores in the query datasets.



### **3. Potency score along the development**
**1. CCAT**: calculate the correlation. more potent, the hub gene in PPI expresses higher.<br>
**2. StemID**: calculate the connectivity among clusters. assume the progenitor will connect more clusters.<br>
**3. SLICE**: calculate the entropy. more mature cell,lower entropy.<br>

### **4. signatures / scores**
**1. proliferation rate**:Ki67 markers
StemID predict the stem cell identity <br> 
**2. contributions of gene list**:calculate the sum(log2(gene list UMI))/all((log2 UMI))-> validate the contributions of gene list for the whole transcriptome.<br>
**3. module score**： AUCell for the gene set <br>





