##### other notes
###### Seurat read h5seurat object
```r
library(Seurat)
library(SeuratData)
library(SeuratDisk)

GetAssayData.Seurat <- function(object, layer = "data", slot, ...) {
  if (!missing(slot)) layer <- slot
  return(SeuratObject:::GetAssayData.Seurat(object, layer = layer, ...))
}
registerS3method("GetAssayData", "Seurat", GetAssayData.Seurat, envir = asNamespace("SeuratObject"))

GetAssayData.Assay <- function(object, layer = "data", slot, ...) {
  if (!missing(slot)) layer <- slot
  return(SeuratObject:::GetAssayData.Assay(object, layer = layer, ...))
}
registerS3method("GetAssayData", "Assay", GetAssayData.Assay, envir = asNamespace("SeuratObject"))

SetAssayData.Assay <- function(object, layer, new.data, slot, ...) {
  if (!missing(slot)) layer <- slot
  return(SeuratObject:::SetAssayData.Assay(object, layer = layer, new.data = new.data, ...))
}
registerS3method("SetAssayData", "Assay", SetAssayData.Assay, envir = asNamespace("SeuratObject"))

obj <- LoadH5Seurat("./ENS_log_counts.h5seurat", 
                    assays = "RNA", 
                    meta.data = TRUE)
```