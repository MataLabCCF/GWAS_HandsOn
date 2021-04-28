# GWAS Hands On

This tutorial is a modification of the tutorial at https://github.com/MareesAT/GWA_tutorial. We modified the commands so that they could be run on Windows computers. If you use Unix-based computers (MacOs and Linux) please use the commands and codes present in the original repository

On this topic, we will assume that you run all steps presented in 2_Population_stratification. 

## Preparation

You have to download the file present on https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/3_Association_GWAS/3_Association_GWAS.zip and uncompress at the analysis folder, in our example, C:\HandsOn\ . After that, copy the final files from the Population stratification to this folder:

```
copy <path of analysis folder>\2_Population_stratification\HapMap_3_r3_13.* <path of analysis folder>\3_Association_GWAS
copy <path of analysis folder>\2_Population_stratification\covar_mds.txt <path of analysis folder>\3_Association_GWAS
```

In our example

```
copy C:\HandsOn\2_Population_stratification\HapMap_3_r3_13.* C:\HandsOn\3_Association_GWAS
copy C:\HandsOn\2_Population_stratification\covar_mds.txt C:\HandsOn\3_Association_GWAS

```

After copy, you have to enter on the folder created for this topic:

```
cd <path of analysis folder>\3_Association_GWAS
```

In our example

```
cd C:\HandsOn\3_Association_GWAS
```

## Association analysis

For the association analyses we use the files generated in the previous tutorial (population stratification). 

### assoc
For binary traits we can use the "--assoc" flag on PLINK:

```
<path to plink.exe> --bfile HapMap_3_r3_13 --assoc --out assoc_results
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 --assoc --out assoc_results
```

Note, the "--assoc" option does not allow to correct covariates such as principal components (PC's)/ MDS components, which makes it less suited for association analyses.

### logistic
We will be using 10 principal components as covariates in this logistic analysis. We use the MDS components calculated from the previous tutorial: covar_mds.txt.

```
<path to plink.exe> --bfile HapMap_3_r3_13 --covar covar_mds.txt --logistic --hide-covar --out logistic_results
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 --covar covar_mds.txt --logistic --hide-covar --out logistic_results
```

Note, we use the option -–hide-covar to only show the additive results of the SNPs in the output file.
We have to remove NA values, those might give problems generating plots in later steps. To do this, run the script **removeNA.R**

The results obtained from these GWAS analyses will be visualized in the last step. This will also show if the data set contains any genome-wide significant SNPs.
Note, in case of a quantitative outcome measure the option --logistic should be replaced by --linear. The use of the --assoc option is also possible for quantitative outcome measures (as metioned previously, this option does not allow the use of covariates).

## Multiple testing

There are various way to deal with multiple testing outside of the conventional genome-wide significance threshold of 5.0E-8, below we present a couple. 

### Adjust
```
<path to plink.exe> --bfile HapMap_3_r3_13 -assoc --adjust --out adjusted_assoc_results
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 -assoc --adjust --out adjusted_assoc_results
```

This file gives a Bonferroni corrected p-value, along with FDR and others. 

### Permutation

This is a computational intensive step. Further pros and cons of this method, which can be used for association and dealing with multiple testing, are described in the article corresponding to this tutorial (https://www.ncbi.nlm.nih.gov/pubmed/29484742).
The reduce computational time we only perform this test on a subset of the SNPs from chromosome 22. To create the list of SNPs that we will run the permutation, please execute te R script called **makeSubset.R**.


```
<path to plink.exe> --bfile HapMap_3_r3_13 -extract subset_snp_chr_22.txt --make-bed --out HapMap_subset_for_perm
<path to plink.exe> --bfile HapMap_subset_for_perm --assoc --mperm 1000000 --out subset_1M_perm_result
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 -extract subset_snp_chr_22.txt --make-bed --out HapMap_subset_for_perm
C:\HandsOn\plink.exe --bfile HapMap_subset_for_perm --assoc --mperm 1000000 --out subset_1M_perm_result
```
The EMP2 collumn provides the for multiple testing corrected p-value.


## QQPLOT and Manhattan plot

These scripts assume R >= 3.0.0.
If you changed the name of the .assoc file or to the assoc.logistic file, please assign those names also to the Rscripts for the Manhattan and QQ plot, otherwise the scripts will not run.

The following Rscripts require the R package qqman, the scripts provided will automatically download this R package and install.

The first script to run is **Manhattan_plot.R**
After get the Manhattan plot, pease run the script **QQ_plot.R**
