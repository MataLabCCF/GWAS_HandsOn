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

Step 3) This tutorial requires the open-source programming language R and the open-source whole genome association analysis toolset PLINK. If these programs are not already installed on your computer they can be downloaded from respectively: https://www.r-project.org/ and https://www.cog-genomics.org/plink2

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

#### Step 1
**Investigate missingness per individual and per SNP and make histograms**

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

**Generate plots to visualize the missingness results.**

Open the R script named hist_miss.R on Rstudion and run. To run all lines, you can select all lines and click on the buttom run or use the shotcurt ctrl+alt+r. This script will generate two plots, on for individual missing data (histimiss.pdf) and one for SNP mnissing data(histlmiss.pdf)

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img7.PNG)

**Remove the missing data**

Delete SNPs and individuals with high levels of missingness, explanation of this and all following steps can be found in box 1 and table 1 of the article mentioned in the comments of this script.
The following two QC commands will not remove any SNPs or individuals because there is any SNP or individual with missing data greater or equal to 0.2. However, it is good practice to start the QC with these non-stringent thresholds.  

Delete SNPs with missingness >0.2.
```
<path to plink.exe> --bfile HapMap_3_r3_1 --geno 0.2 --make-bed --out HapMap_3_r3_2
```
Delete SNPs with missingness >0.2.
```
<path to plink.exe> --bfile HapMap_3_r3_2 --geno 0.2 --make-bed --out HapMap_3_r3_3
```
Delete SNPs with missingness >0.02.
```
<path to plink.exe> --bfile HapMap_3_r3_3 --geno 0.2 --make-bed --out HapMap_3_r3_4
```
Delete individuals with missingness >0.02.
```
<path to plink.exe> --bfile HapMap_3_r3_4 --geno 0.2 --make-bed --out HapMap_3_r3_5
```
The previous commands on our example
```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_1 --geno 0.2 --make-bed --out HapMap_3_r3_2
C:\HandsOn\plink.exe --bfile HapMap_3_r3_2 --mind 0.2 --make-bed --out HapMap_3_r3_3
C:\HandsOn\plink.exe --bfile HapMap_3_r3_3 --geno 0.02 --make-bed --out HapMap_3_r3_4
C:\HandsOn\plink.exe --bfile HapMap_3_r3_4 --mind 0.02 --make-bed --out HapMap_3_r3_5
```
![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img8.PNG)
![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img9.PNG)

#### Step 2

**Check for sex discrepancy.**

Subjects who were a priori determined as females must have a F value of <0.2, and subjects who were a priori determined as males must have a F value >0.8. This F value is based on the X chromosome inbreeding (homozygosity) estimate.

Subjects who do not fulfil these requirements are flagged "PROBLEM" by PLINK.

```
<path to plink.exe> --bfile HapMap_3_r3_5 --check-sex 
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_5 --check-sex 
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img10.PNG)

**Generate plots to visualize the sex-check results.**

Open the script gender_check.R in R-Studio and run all lines as mentioned before. This script will create three PDF files, one for men (Men_check.pdf), one for women (Women_check.pdf) and one for all individuals (gender_check.pdf)

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img11.PNG)

These checks indicate that there is one woman with a sex discrepancy, F value of 0.99. (When using other datasets often a few discrepancies will be found). 

The following two scripts can be used to deal with individuals with a sex discrepancy.
Note, please use one of the two options below to generate the bfile hapmap_r3_6, this file we will use in the next step of this tutorial.

**1) Delete individuals with sex discrepancy.**

Open the scrpit extract_problem.R in R-Studio and run as mentioned above. This script will output a file (sex_discrepancy.txt) with two columns. The first column is the family ID and the second column is the individual ID. Using this file you can remove the individuals with problem using this command

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img12.PNG)

```
<path to plink.exe> --bfile HapMap_3_r3_5 --remove sex_discrepancy.txt --make-bed --out HapMap_3_r3_6 
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_5 --remove sex_discrepancy.txt --make-bed --out HapMap_3_r3_6  
```
This command removes the list of individuals with the status “PROBLEM”.

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img13.PNG)

**2) impute-sex.**

```
<path to plink.exe> --bfile HapMap_3_r3_5 --impute-sex --make-bed --out HapMap_3_r3_6
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_5 --impute-sex --make-bed --out HapMap_3_r3_6
```

This imputes the sex based on the genotype information into your data set.

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img14.PNG)

#### Step 3

**Generate a bfile with autosomal SNPs only and delete SNPs with a low minor allele frequency (MAF).**

```
<path to plink.exe> --bfile HapMap_3_r3_6 --chr 1-22 --make-bed --out HapMap_3_r3_7
```

In our example


```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_6 --chr 1-22 --make-bed --out HapMap_3_r3_7
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img15.PNG)

**Generate a plot of the MAF distribution.**

```
<path to plink.exe>  --bfile HapMap_3_r3_7 --freq --out MAF_check
```

In our example

```
C:\HandsOn\plink.exe  --bfile HapMap_3_r3_7 --freq --out MAF_check
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img16.PNG)

After this, open the script MAF_check.R and run the script as mentioned before and run. This script will generate a histogram with MAF distribution (MAF_distribution.pdf)

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img17.PNG)

Remove SNPs with a low MAF frequency. A conventional MAF threshold for a regular GWAS is between 0.01 or 0.05, depending on sample size.

```
<path to plink.exe>  --bfile HapMap_3_r3_7 --maf 0.05 --make-bed --out HapMap_3_r3_8
```

In our example

```
C:\HandsOn\plink.exe  --bfile HapMap_3_r3_7 --maf 0.05 --make-bed --out HapMap_3_r3_8
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img18.PNG)

#### Step 4

Delete SNPs which are not in Hardy-Weinberg equilibrium (HWE).
Check the distribution of HWE p-values of all SNPs.

```
<path to plink.exe> --bfile HapMap_3_r3_8 --hardy
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_8 --hardy
```

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img19.PNG)

**Open the script hwe.R in R-Studio and run as mentioned before**. This script will select the SNPs with p-value < 0.00001 (we will call these SNPs as 'strongly deviating SNPs'), plot the MAF distribution for all SNPs (histhwe.pdf) and for the strongly deviating SNPs (histhwe_below_theshold.pdf)

By default the --hwe option in plink only filters for controls.
Therefore, we use two steps, first we use a stringent HWE threshold for controls, followed by a less stringent threshold for the case data.

```
<path to plink.exe> --bfile HapMap_3_r3_8 --hwe 1e-6 --make-bed --out HapMap_hwe_filter_step1
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_3_r3_8 --hwe 1e-6 --make-bed --out HapMap_hwe_filter_step1
```

The HWE threshold for the cases filters out only SNPs which deviate extremely from HWE. 
This second HWE step only focusses on cases because in the controls all SNPs with a HWE p-value < hwe 1e-6 were already removed

```
<path to plink.exe> --bfile HapMap_hwe_filter_step1 --hwe 1e-10 --hwe-all --make-bed --out HapMap_3_r3_9
```

In our example

```
C:\HandsOn\plink.exe --bfile HapMap_hwe_filter_step1 --hwe 1e-10 --hwe-all --make-bed --out HapMap_3_r3_9
```

Theoretical background for this step is given in our accompanying article: https://www.ncbi.nlm.nih.gov/pubmed/29484742 .

![Alt text](https://github.com/MataLabCCF/GWAS_HandsOn/blob/main/ImagesHandsOn/Img20.PNG)


#### Step 5
