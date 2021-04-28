On  this topic, we will assume that you run all steps presented in 1_QC_GWAS. With that, we will not present prints of how to run plink or R scripts, or unzip files.

This is the main script for second tutorial from our comprehensive tutorial on GWAS and PRS changed to run on Windows computer. The original tutorial is avaliable at https://github.com/MareesAT/GWA_tutorial/. This step requires a lot of computational capacity and time, so it is recommended that you run the commands on a server. So we will explain the purpose of the analysis and provide the pre-processed data for all students to follow.

You have to download the file present on https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/2_Population_stratification/2_Population_stratification.zip and uncompress at the analysis folder, in our example, C:\HandsOn\

## Preparation

The original tutorial requires the download of a file with 60 GB from 1KGP (1000 Genomes Project) and processing this file. For this type of analysis the ideal is that you perform these steps on a server. With the aim of allowing the analysis to be elucidated, we carry out the analyzes and provide an overview. If you have access to a server, we recomended you run all the steps of the original tutorial.

First of all, we downloaded the data and convert to Bed/Bim/Fam format using PLINK. After that we entered unique IDs for SNPs that did not have an ID, that is, they had a "." in the ID field. After that, we removed the missing data (geno and mind) as seen in 1_QC_GWAS.

After this, we extract the SNPs in common between the data downloaded from the 1KGP and our data that was generated in the last tutorial (HapMap_3_r3_12). After this we changed the 1KGP genome build (making both databases have the same genome version) and merge the data. After that, we use the file from linkage disequilibrium (LD) analysis performed on the previous tutorial (indepSNP.prune.in) to get a databank with SNPs without LD.

After make the QC in 1KGP data, merge the both databases and prune the SNPs in LD, we performed the Multidimensional scaling (MDS), that we use in order to measure the similarity between the individuals. Because we are using all populations in the 1KGP, it is possible to find out if our data are more similar to which continental populations. In this tutorial we want to know if our individuals are similar to European individuals and, if there is any individual similar to other populations, we should remove it to avoid spourious associations.

After creating a file with the Individual ID and the respective ancestry we can plot the MDS using the script **MDS_merged.R**. After running the script we can see that our individuals (named OWN on the plot) are grouped with European individuals.

The output file MDS.pdf demonstrates that our ‘own’ data falls within the European group of the 1000 genomes data. Therefore, we do not have to remove subjects.
For educational purposes however, we will filter out population stratification outliers. The R script **MDS_merged.R** generated a file named EUR_MDS_merge2, that have all individuals that fits on the interval MDS component 1 < -0.04 and MDS component 2 > 0.03. We will keep this individuals, using the PLINK on the data generated on 1_QC_GWAS
```
copy <path of analysis folder>\1_QC_GWAS\HapMap_3_r3_12.* <path of analysis folder>\2_Population_stratification
copy <path of analysis folder>\1_QC_GWAS\indepSNP.prune.in <path of analysis folder>\2_Population_stratification
cd <path of analysis folder>\2_Population_stratification
<path to plink.exe> --bfile HapMap_3_r3_12 --keep EUR_MDS_merge2 --make-bed --out HapMap_3_r3_13
```

In our example

```
copy C:\HandsOn\1_QC_GWAS\HapMap_3_r3_12.* C:\HandsOn\2_Population_stratification
copy C:\HandsOn\1_QC_GWAS\indepSNP.prune.in C:\HandsOn\2_Population_stratification
cd C:\HandsOn\2_Population_stratification
C:\HandsOn\plink.exe --bfile HapMap_3_r3_12 --keep EUR_MDS_merge2 --make-bed --out HapMap_3_r3_13
```

Now we will perform an MDS ONLY on HapMap data without ethnic outliers. The values of the 10 MDS dimensions are subsequently used as covariates in the association analysis in the third tutorial.

```
<path to plink.exe> --bfile HapMap_3_r3_13 --extract indepSNP.prune.in --genome --out HapMap_3_r3_13
<path to plink.exe> --bfile HapMap_3_r3_13 --read-genome HapMap_3_r3_13.genome --cluster --mds-plot 10 --out HapMap_3_r3_13_mds
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 --extract indepSNP.prune.in --genome --out HapMap_3_r3_13
C:\HandsOn\plink.exe --bfile HapMap_3_r3_13 --read-genome HapMap_3_r3_13.genome --cluster --mds-plot 10 --out HapMap_3_r3_13_mds
```

After run PLINK to get the MDS, you have to run the R script "parseMDS.R" to convert the MDS to a covariate file to be used on the next topic.

The values in covar_mds.txt will be used as covariates, to adjust for remaining population stratification, in the third tutorial where we will perform a genome-wide association analysis.

CONGRATULATIONS you have succesfully controlled your data for population stratification!

For the next tutorial you need the following files:
- HapMap_3_r3_13 (the bfile, i.e., HapMap_3_r3_13.bed,HapMap_3_r3_13.bim,and HapMap_3_r3_13.fam
- covar_mds.txt
