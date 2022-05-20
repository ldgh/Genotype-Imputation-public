### Summary


- [Introduction](#introduction)
- [Imputation](#imputation)
- [Requirements](#requirements)
- [Scripts](#scripts)
- [Mandatory parameters](#mandatory-parameters)
- [Mandatory input files](#mandatory-input-files)
- [Optional parameters](#optional-parameters)
- [Example of the execution of the command line](#example-of-the-execution-of-the-command-line)
- [Citation](#citation)

## Introduction 
 Genotype imputation is the statistic inference of genotypes that were not obtained experimentaly in a target panel, based on patterns of linkage disequilibrium, using a high density genotype panel as reference. The imputation today is almost a requirement in modern GWAS, because it enhances the statistical power by increasing the number of "genotyped" snps, and by allowing a better match in merging studies results that uses different genotyping platforms. An important result is the use of a reference panel that is most likely to represent the study population, or target panel, to gain accuracy in the imputation. We upgraded our analysis in Latin American populations by using a combination of the 1000 Genomes Project dataset with the recent sequenced brazilian SABE cohort represented by 1171 brazilian whole genomes.

 The following **Figure 1** by Marchini & Howie (2010) is a demonstration of the imputation process in the genomes of unrelated individuals. The target samples comprise a set of genotyped SNPs with a large number of non-genotyped SNPs, each line is an individual and each column is a snp **(a)**. Performing an association test with only these SNPs may not lead to a significant association **(b)**. The aim of the imputation process is to predict those missing genotypes. Although many software have been developed for imputation, some basic steps must be followed and the first one is strand alignment between data sets and phasing each individual in the study at the typed SNPs. Three phased individuals are exhibited **(c)**. Then, target samples haplotypes are compared to a dense set of haplotypes from a reference panel **(d)**. The figure shows target individuals coloured according to the match with the reference panel haplotypes. Then, missing genotypes from the target sample are predicted using the match between data sets **(e)**. This can increase both the power to detect association signals and the signal resolution near a causal or associated variant **(f)**.

![0](https://user-images.githubusercontent.com/73356412/169386169-38ec0c4e-d84c-4ada-acd0-488940120644.png)

 The current imputation script is a congregation of all the metodological steps deliniated in the **Figure 2** of Magalhães et al. (2018), which are separated in the following processes:
* (A) Imputation Overview
* (B) Reference Panel Creation
* (C) Merge Reference Panels
* (D) Pre-Phasing

![imagem_2022-05-19_185824468 (cópia)](https://user-images.githubusercontent.com/73356412/169386287-ed0f3215-1a91-4171-8b91-8faccb8b883e.png)
 
 Overview of the complete imputation process **(A)**.

![imagem_2022-05-19_185936498 (cópia)](https://user-images.githubusercontent.com/73356412/169386358-e090711e-3452-4487-bc95-930a2f5c6893.png)

 Requirements of the imputation process is the creation **(B)** or merge of the reference panels, in this case the 1KGP and SABE cohorts, but other cohorts can also be used. In this step, the unphased genotypes are converted into a reference panel, producing the SABE panel of haplotypes from the brazilian SABE cohort data set.

![imagem_2022-05-19_185951421 (cópia)](https://user-images.githubusercontent.com/73356412/169386407-2d6f60e6-a076-4537-a26d-c7e0673f4774.png)

 The other requirement previous to the imputation is the merge reference panel step **(C)** which uses the IMPUTE2 or IMPUTE4 software to produce the combination of two distinct panels.

![imagem_2022-05-19_190004335 (cópia)](https://user-images.githubusercontent.com/73356412/169386411-896ab0b3-ff19-4e4c-8fec-a0e5976cd913.png)

The next step is the process itself **(D)**, composed by three main tasks: pre-phasing, haplotype phase inference, and imputation. The pre-phasing step performs strand alignment between target and reference panel using SHAPEIT2 or SHAPEIT4, PLINK, and the scripting language AWK. Using the metodology implemented in the SHAPEIT software to infere the haplotype phase of the target data set, producing .haps and .sample files.

![image](https://user-images.githubusercontent.com/51415738/169545061-6e86a35c-96ff-48bc-b57c-0f039132c978.png)


## Imputation


 The newest imputation script developed by the LDGH crew to impute genotypes by either Shapeit2 or Shapeit4 and Impute2 or Impute4. This script can be also used to independently perform the tasks of haplotyping and/or imputation, alongside with converting the final data to vcf or bgen file formats, and run the snp-stats flag from qctool.

 
## Requirements


Imputation.py and modificaLegend.py scripts were implemented using python language. Use the following programs in this execution:

* [Plink](https://www.cog-genomics.org/plink/)
* Bcftools
* Shapeit2 and/or Shapeit4 (program of your choosing)
* Vcftools
* Impute2 and/or Impute4 (program of your choosing)
* QCtool
* Python3

## Scripts


**ModificaLegend.py**

> This script modifies the reference panel file format .hap e .legend, and creates the file .samples in the intended design e formats required for conversion for the VCF format by Bcftools

**main.py**

> This script haplotypes and/or imputes, implemented in python3 language using the programs Shapeit2 and/or Shapeit4 and Impute2 and/or Impute4. There are mandatory files, mandatory parameters and optional parameters:


## Mandatory parameters:


* ***--analysis or -a***		
> File with the description of analysis to be performed (Haplotype, Imputate, Chunk)

* ***--programs or -p***	
> File with the path to the programs used

* ***--reference or -r***	
 > File with the path of reference files

* ***--database or -d***	
> File with the path of databases files

* ***--thread or -t***		
> Number of threads to be used in haplotyping and/or imputation

* ***--folder or -f***		
> Folder to store the intermediary files and output files


## Mandatory input files:

##### Analysis


> File with the description of analysis to be performed (Haplotype, Imputate, Chunk). This file must be separated by \t.

Example:
````
%Haplotype
Database1
Database2	Reference

%Impute
Database1	Reference
Database2	Reference

%Chunk	7000000
````

##### Database

> File with the path of databases files and other important information, such as: 
First and Last Chromosome
Type of Data
If the data is Gzipped
If the data is Phased

Example:

````
#POP	Path	FirstChr	LastChr	DataType	Gzipped?	Phased
Database1	/media/imputation/database1_chr*.vcf.gz	1	22	VCF	Yes	No
Database2	/media/imputation/database2_chr*.vcf.gz	1	22	VCF	Yes	No
````

##### Reference


> File with the path of reference files. This file must have two columns (Name of reference dataset and Path to the reference file) separated by \t. There must be at least three type of files:
MAPS - the genetic map to the required chromosome to be haplotyped and/or imputed
VCF - the gzipped vcf file of the reference dataset
HAP - the .hap .sam .legend files of the reference dataset

Example:

```
MAPS	/media/imputation/chr*.b38.gmap.gz
Reference	VCF	/media/imputation/Reference_panel_chr*.vcf.gz
Reference	HAP	/media/imputation/Reference_panel_hg38_chr*
```

##### Programs

> File with the path to the programs used. This file must have two columns (Name of the program and Path to the program) separated by \t.

Example:
```
plink	/media/imputation/plink
bcftools	/media/imputation/bcftools
shapeit4	/media/imputation/shapeit4
vcftools	/media/imputation/vcftools
impute4	/media/imputation/impute4.1.2_r300.1
qctool	/media/imputation/qctool
shapeit2	/media/imputation/shapeit2
impute2	/media/imputation/impute2
```

## Optional parameters:
* ***--check or -c***		
> Perform shapeit2 check process

* ***--vcfout or -v***		
> Set the output as a VCF file
 
* ***--bgenout or -b***		
> Set the output as a BGEN file
 
* ***--snpstats or -s***	
> Run the snp-stats function from qctool
 
* ***--onlyprint or -o***	
> Print the command line, do not run

## Example of the execution of the command line:
````
python3 main.py -a analysis.txt -d database.txt -r reference.txt -p programs -t 10 -f /home/imputation/
````
## Citation
###### [Click to return to summary](#summary)

>This imputation pipeline and panel were developed as a product for future colaborations in Latin American cohorts to use a properly schematics in order to improve their analysis and discoveries of novel associations that could be overlooked in a poorly diverse reference panel.
* MARCHINI, J.; HOWIE, B. Genotype imputation for genome-wide association studies. Nature Reviews Genetics, v. 11, n. 7, p. 499–511, 2010.
* Magalhães, W., Araujo, N. M., Leal, T. P., Araujo, G. S., Viriato, P., Kehdy, F. S., Costa, G. N., Barreto, M. L., Horta, B. L., Lima-Costa, M. F., Pereira, A. C., Tarazona-Santos, E., Rodrigues, M. R., & Brazilian EPIGEN Consortium (2018). EPIGEN-Brazil Initiative resources: a Latin American imputation panel and the Scientific Workflow. Genome research, 28(7), 1090–1095. https://doi.org/10.1101/gr.225458.117

