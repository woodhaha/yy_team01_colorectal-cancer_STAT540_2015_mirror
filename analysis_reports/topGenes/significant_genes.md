# Significant Genes
Santina  
Sunday, April 05, 2015  

Here we will look for significant genes as identified in the top tables from limma anlaysis 

# Load data 

Load the top tables: 

```r
n_c <- read.delim("../04.1limma-normal_vs_cancer/normal_vs_cancer_santina.tsv", sep="\t")
n_a <- read.delim("../04.2limma-normal_vs_adenoma/normal_vs_adenoma_santina.tsv", sep="\t")
n_n <- read.delim("../04.1limma-normal-C_vs_normal-H/normalC_vs_normalH_santina.tsv", sep="\t")
a_c <- read.delim("../04.3limma-adenoma_vs_cancer/adenoma_vs_cancer_santina.tsv", sep="\t")
```

# Threshold 

We will use q value < 1e-6 as our threshold 


```r
getGenes <- function(topTable, threshold){
	genes  <- rownames(topTable[which(topTable$q.value <= threshold), ])
}

n_c_genes <- getGenes(n_c, 1e-4)
n_a_genes <- getGenes(n_a, 1e-4)
a_c_genes <- getGenes(a_c, 1e-4)
n_n_genes <- getGenes(n_n, 1e-4)
```

There are 8247 in `n_c_genes`, 
13276 in `n_a_genes`, 
8 in `n_n_genes`, and 
1011 in `a_c_genes`.  

# Venn Diagram

Of genes in normal-cancer, normal-adenoma, and adenoma-cancer.

```r
library(VennDiagram)
```

```
## Warning: package 'VennDiagram' was built under R version 3.1.3
```

```
## Loading required package: grid
```

```r
# Put all genes in the list where the names will identify each region of the plot
cool_genes <- list(N_C = n_c_genes, N_A = n_a_genes, A_C = a_c_genes)

# Start a new plot and save it 

plot.new()
```

![](significant_genes_files/figure-html/unnamed-chunk-3-1.png) 

```r
venn.plot <- venn.diagram(cool_genes, filename = "../../figures/venn_diagram_e4.tiff", height = 3000, width = 3000, resolution = 800, fill=c("red", "blue", "yellow"), alpha = 0.5, cex = 1)
```

Go see it in the file , or I can do this: 


```r
plot.new()
venn.plot <- venn.diagram(cool_genes, filename = NULL, fill=c("red", "blue", "yellow"), alpha = 0.5, cex = 1)
grid.draw(venn.plot)
```

![](significant_genes_files/figure-html/unnamed-chunk-4-1.png) 

# Save the list 

```r
cool_genes <- c(cool_genes, list(N_N = n_n_genes)) 
#save(cool_genes, file = "cool_genes_e5.RData")
```

You can view the list file like this: 
- `load("cool_genes_e5.RData")`
- `cool_genes["N_N"]` to view the list of probes of normalC versus normalH
- `cool_genes["N_C"]` to view the list of probes of normal(C+H) versus cancer 
- `cool_genes[["N_A"]]` to GET the list of probes of normal(C+H) versus adenoma
etc 

# Save the intersection genes

```r
nc_na <- intersect(n_c_genes, n_a_genes)
nc_ac <- intersect(n_c_genes, a_c_genes)
ac_na <- intersect(a_c_genes, n_a_genes)
all <- intersect(n_c_genes, intersect(n_a_genes, a_c_genes))

intersect_genes <- list(nc_na = nc_na, nc_ac = nc_ac, ac_na = ac_na, all = all)
save(intersect_genes, file = "intersect_genes_e4.RData")
```


