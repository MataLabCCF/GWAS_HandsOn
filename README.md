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

Once you've created a directory in which you have downloaded and unzipped the folder: 1_QC_GWAS.zip, you are ready to start the first part of the actual tutorial. All steps of this tutorial will be excecuted using the commands from the main script: 1_Main_script_QC_GWAS.txt, the only thing necessary in completing the tutorial is copy-and-paste the commands from the main script at the prompt Windows device. Note, make sure you are in the directory containing all files, which is the directory after the last command of step 2. There is no need to open the other files manually. Using the command tree inside the folder we can see the list of the files inside the folder (and the files inside the folders inside)

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img5.PNG)

Different from the original material, we will use this GitHub readme as the tutorial file.

### Explanation of the main script

This tutorial uses freely available HapMap data: hapmap3_r3_b36_fwd.consensus.qc. The authors simulated a binary outcome measure (i.e., a binary phenotypic trait) and added this to the dataset. The outcome measure was only simulated for the founders in the HapMap data. This data set will be referred to as HapMap_3_r3_1. 

The HapMap data, without their simulated outcome measure, can also be obtained from http://hapmap.ncbi.nlm.nih.gov/downloads/genotypes/2010-05_phaseIII/plink_format/ 

It is essential for the execution of the tutorial that that all scripts belonging to this tutorial are in the same directory on your Windows workstation. **We want to remeber that this GitHub is a modification to perform this steps on Windows computer.**

Many scripts include comments which explain how these scripts work. Note, in order to complete the tutorial it is essential to execute all commands in this tutorial.

This script can also be used for your own data analysis, to use it as such, replace the name of the HapMap file with the name of your own data file. 

Furthermore, this script is based on a binary outcome measure, and is therefore not applicable for quantitative outcome measures (this would require some adaptations)

Note, most GWAS studies are performed on an ethnic homogenous population, in which population outliers are removed. The HapMap data, used for this tutorial, contains multiple distinct ethnic groups, which makes it problematic for analysis.

Therefore, the authors have selected only the EUR individuals of the complete HapMap sample for the tutorials 1-3. This selection is already performed in the HapMap_3_r3_1 file from the original GitHub page.

The Rscripts used in this tutorial are all executed from the Windows command line (cmd).

Therefore, this tutorial and the other tutorials from our GitHub page, can be completed simply by copy-and-pasting all commands from the ‘main scripts’ into the Windows terminal (cmd).

For a thorough theoretical explanation of all QC steps we refer to the article accompanying this tutorial entitled “A tutorial on conducting Genome-Wide-Association Studies: Quality control and statistical analysis” (https://www.ncbi.nlm.nih.gov/pubmed/29484742).

### Start Analysis

Change directory to a folder on your Windows device containing all files from ‘1_QC_GWAS.zip’.

```
cd <path to the folder 1_QC_GWAS>
```

In our example, we just type

```
cd C:\HandsOn\1_QC_GWAS
```

#### Step 1: Investigate missingness per individual and per SNP and make histograms

The command to this step is 

```
<path to plink.exe> --bfile HapMap_3_r3_1 --missing
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_1 --missing
```
Output: plink.imiss and plink.lmiss, these files show respectively the proportion of missing SNPs per individual and the proportion of missing individuals per SNP.

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img6.PNG)
