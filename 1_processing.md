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