### **Transcript quantification**
#### <u>plate-based protocols (full length)</u>
an even coverage of transcripts is not always achieved with full-length protocols and therefore specific regions across the gene body may still be biased. full-length protocols is that they allow for the detection of splice variants.

#### <u>Drop/Tag-based</u>
it can mitigate the bias of transcript amplification. It can not align reads to the distinguishing transcripts. In microfluidic based protocols only about 10% of the transcripts of the cell are recovered, but it is sufficient to identify the cell types. In the microfluidic device-based methods (Drop-seq, Indrop, 10x), reads originate from valid barcodes was 75% for 10x, while the others are less than 30%. But 10X genomics favored the capture and amplification of shorter genes and genes with higher GC content.

### **scRNA vs snRNA**
snRNA-seq could not always capture all cell types because of the vulnerable cell types such as glutamatergic neurons in the brain. Besides, the snRNA-seq rely on the fresh tissue. snRNA-seq does not need the cell dissociation and are more resistant to mechanical force, and safe to the frozen tissue. It should be noticed that the cell dissociation has a strong effect on the potentially observable cell types.

Mapping to the genome
Because cell ranger use the entire genome for reference, the reads mapping outside the annotated transcripts axons can still be accounted for.


### **scRNA-seq data and quality control**
Sequencing depth affects the UMI count and the number of genes detected in the cells. It should be cautious when comparing the expression levels between samples.


#### <u> What can be done? (scRNA-seq data normalization) </u>
1. The **NormalizedData** in seurat will correct the sequencing depth in cells (UMI divided by the total count of UMI in each cell -> log2 scaled -> multiply by the common size factor) <br>
2. After using the **SCTransform** in the seurat, the PrepSCTFindMarkers could be used to identified the markers based on the normalized expression data (the SCT will remove the sequencing batch effect using the minimum median total UMI count to normalise the data), tutorial reference.<br>
3. Use the tool **SCnorm** to determine size factors in different groups/batches. 
4. Cellranger aggre normalization (seldom used in the study)
5. The CPM (not far for the lower-expression genes) and LSF (Lun Sum Factors) and the normalized data in scater. 

**Actually, few studies will correct the sequencing depth between samples.**

QC metrics 
Quality control will define mitochondrial, ribosomal and hemoglobin gene.
As for the mitochondrial genes,they associated with cell degradation. But since number of counts and gene per cell is sufficiently high and percentage of mitochondrial reads is for most cells below 20 % we can still process the data. The threshold for quality control can be determined by the MAD(median absolute deviations), like lower than the mean+3MAD
UMI: over 500, the average value in the Seurat PBMC data is around 2500
nFeatures: in PBMC tutorial is around 1000, the filter condition is 2500 > nFeature > 200