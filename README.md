# Imputation

The newest imputation script developed by the LDGH crew to impute genotypes by either Shapeit2 or Shapeit4 and Impute2 or Impute4. This script can be also used to independently perform the tasks of haplotyping and/or imputation, alongside with converting the final data to vcf or bgen file formats, and run the snp-stats flag from qctool.

## Requirements

Imputation.py and modificaLegend.py scripts were implemented using python language. Use the following programs in this execution:

* Plink
* Bcftools
* Shapeit2 and/or Shapeit4 (program of your choosing)
* Vcftools
* Impute2 and/or Impute4 (program of your choosing)
* QCtool
* Python3

## Scripts

**ModificaLegend.py**

> this script modifies the reference panel file format .hap e .legend, and creates the file .samples in the intended design e formats required for conversion for the VCF format by Bcftools

**main.py**

> this script haplotypes and/or imputes, implemented in python3 language using the programs Shapeit2 and/or Shapeit4 and Impute2 and/or Impute4. There are mandatory files, mandatory parameters and optional parameters:

## Mandatory input files:

***analysis.txt***    

> File with the description of analysis to be performed (Haplotype, Imputate, Chunk)
```

Exemplo Ricardo

```
***database.txt*** 

> File with the path of databases files
```

Exemplo Ricardo

```
***reference.txt***   

> File with the path of reference files
```

Exemplo Ricardo

```
***programs.txt***    

> File with the path to the programs used
```
plink /media/imputation/plink
bcftools  /media/imputation/bcftools
shapeit4  /media/imputation/shapeit4
vcftools  /media/imputation/vcftools
impute4 /media/imputation/impute4.1.2_r300.1
qctool  /media/imputation/qctool
shapeit2  /media/imputation/shapeit2
impute2 /media/imputation/impute2
```

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
## Analysis

File with the description of analysis to be performed (Haplotype, Imputate, Chunk). This file must be separated by \t.
Example:
````
%Haplotype
Database1
Database2	Reference

%Impute
Database1	Reference
Database2	Reference

%Chunk	7000000

