### Summary

- [Introduction](#introduction)
- [Imputation](#imputation)
- [Requirements](#requirements)
- [Scripts](#scripts)
- [Mandatory parameters](#mandatory-parameters)
- [Mandatory input files](#mandatory-input-files)
- [Optional parameters](#optional-parameters)
- [Example of the execution of the command line](#example-of-the-execution-of-the-command-line)
- [References](#references)

## Introduction
![0](https://user-images.githubusercontent.com/73356412/169386169-38ec0c4e-d84c-4ada-acd0-488940120644.png)

![imagem_2022-05-19_185824468 (cópia)](https://user-images.githubusercontent.com/73356412/169386287-ed0f3215-1a91-4171-8b91-8faccb8b883e.png)

![imagem_2022-05-19_185936498 (cópia)](https://user-images.githubusercontent.com/73356412/169386358-e090711e-3452-4487-bc95-930a2f5c6893.png)

![imagem_2022-05-19_185951421 (cópia)](https://user-images.githubusercontent.com/73356412/169386407-2d6f60e6-a076-4537-a26d-c7e0673f4774.png)

![imagem_2022-05-19_190004335 (cópia)](https://user-images.githubusercontent.com/73356412/169386411-896ab0b3-ff19-4e4c-8fec-a0e5976cd913.png)

![imagem_2022-05-19_190312050 (cópia)](https://user-images.githubusercontent.com/73356412/169386424-b3433b62-9c66-4de1-bb18-b36a5b8952b5.png)

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
* ***analysis or -a***		
> File with the description of analysis to be performed (Haplotype, Imputate, Chunk)

* ***programs or -p***	
> File with the path to the programs used

* ***reference or -r***	
 > File with the path of reference files

* ***database or -d***	
> File with the path of databases files

* ***thread or -t***		
> Number of threads to be used in haplotyping and/or imputation

* ***folder or -f***		
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
* ***check or -c***		
> Perform shapeit2 check process

* ***vcfout or -v***		
> Set the output as a VCF file
 
* ***bgenout or -b***		
> Set the output as a BGEN file
 
* ***snpstats or -s***	
> Run the snp-stats function from qctool
 
* ***onlyprint or -o***	
> Print the command line, do not run

## Example of the execution of the command line:
````
python3 main.py -a analysis.txt -d database.txt -r reference.txt -p programs -t 10 -f /home/imputation/
````
## References
* (colocar aqui)
* (colocar aqui)
* (colocar aqui)
* (colocar aqui)
* (colocar aqui)
* (colocar aqui)
* (colocar aqui)
