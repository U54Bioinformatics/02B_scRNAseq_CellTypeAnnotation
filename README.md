## Annotating single cell types using the [SingleR](https://bioconductor.org/packages/devel/bioc/html/SingleR.html) package
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
