## Annotating single cell types using the SingleR module in BETSY  
betsy_run.py --num_cores 40 \\  
--input SignalFile --input_file counts.txt \\  
--mattr keep_genes_expressed_in_perc_samples=5 \\  
--dattr SignalFile.preprocess=counts \\  
--output SingleRResults --output_file out-singler_annot.besty.txt \\  
--run  

## Annotating immune single cell types using the ImmClassifier module in BETSY  
betsy_run.py --num_cores 40 \
--input SignalFile --input_file counts.txt \
--dattr SignalFile.preprocess=counts \
--output SingleRResults --output_file out-ImmClass_annot.betsy.txt \\  
--run  



## Annotating single cell types using the [SingleR](https://bioconductor.org/packages/devel/bioc/html/SingleR.html) package  
❗❗❗ __This is the legacy version, please use betsy instead__ ❗❗❗  
### writeups:  
The input "counts_matrix.txt" file looks like so:  

  |      | Cell1 | Cell2 | Cell3 |  
  | ---- | ----- | ----- | ----- |  
  | a    | 1     | 0     | 3     |  
  | b    | 1     | 1     | 20    |  
  | c    | 1     | 0     | 0     |  
  | d    | 0     | 5     | 4     |  
  
For more details please refer to the [:book:manual](https://bioconductor.org/packages/3.11/bioc/manuals/SingleR/man/SingleR.pdf) :))  

--------
### Scripts:  

```r
set.seed(41)

# install packages
# library(devtools)
# devtools::install_github('dviraran/SingleR')

# load pkgs
library(dplyr)
library(SingleR)

# run SingleR 
singler <-   
  CreateSinglerSeuratObject(  
  'counts_matrix.txt', annot = NULL, "ProjectName",  
  min.genes = 10, technology = "10X", species = "Human", citation = "",  
  ref.list = list(), normalize.gene.length = F, variable.genes = "de",  
  fine.tune = T, reduce.file.size = T, do.signatures = T, min.cells = 2,  
  npca = 10, regress.out = "nUMI", do.main.types = T,  
  reduce.seurat.object = T, numCores = 29  
  )  
  
# save the singler object  
save(singler, file = 'singler_object.RData')

# extract singler annotations as a dataframe
singler_annot <- singler$singler[[1]]$SingleR.single.main$labels %>% as.data.frame %>% `colnames<-`(c('singler.annot')) %>% mutate(Cell = rownames(.))
```
