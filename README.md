# GWAS Hands On

This tutorial is a modification of the tutorial at https://github.com/MareesAT/GWA_tutorial. We modified the commands so that they could be run on Windows computers. If you use Unix-based computers (MacOs and Linux) please use the commands and codes present in the original repository

## Preparation

Step 1) Create the folder in which we will perform the steps. To do this, open your terminal (you can type cmd at search bar) and digit the command:

```
mkdir <path>
```
  
In our examples we will use C:\HandsOn. A recommendation is not to use non-alphanumeric characters and spaces in the folder path as it can cause errors.

```
mkdir C:\HandsOn
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img1.PNG)

Step 2) Download the first tutorial (https://github.com/MareesAT/GWA_tutorial/blob/master/1_QC_GWAS.zip?raw=true) and unzip the file at your analysis folder. In our tutorial we use winrar to uncompress the files

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img2.PNG)
![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img3.PNG)

Step 3) This tutorial requires the open-source programming language R and the open-source whole genome association analysis toolset PLINK. If these programs are not already installed on your computer they can be downloaded from respectively: https://www.r-project.org/ https://www.cog-genomics.org/plink2

We recommend using the newest versions. We also recommend install RStudio IDE (https://www.rstudio.com/products/rstudio/download/). 
You have to install the RStudio after install R. We recomend to move plink.exe to the folder created before.

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img4.PNG)
![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img3.PNG)


## Execution of Tutorial 1
