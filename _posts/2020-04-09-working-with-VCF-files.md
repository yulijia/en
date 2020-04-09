---
published: ture
layout: post
title: "working with VCF files"
author: Yu
categories: HowTo
tags:
- VCF
- sort
- index
- intersect
- extract
- merge
- concatenate
- combine
---

* table of content
{:toc}  


This is a note of working with VCF files.


### 1.sort VCF files

```bash
java -jar picard.jar SortVcf INPUT=in.vcf OUTPUT=out.vcf
java -jar picard.jar SortVcf INPUT=in.vcf.gz OUTPUT=out.vcf.gz
```
It will provide index file if you set the output as gz file.

### 2.index VCF files

```bash
bgzip genotypes.vcf && tabix -p vcf genotypes.vcf.gz
```

Ref:[Question: Generate vcf.gz file and its index file vcf.gz.tbi](https://www.biostars.org/p/59492/)

### 3.extract vcf from a bed region

Need have a vcf index file before extract from the vcf file.

```bash
tabix -R region.bed myvcf.gz > extract.vcf
```

Ref:[Question: Extract Sub-Set Of Regions From Vcf File](https://www.biostars.org/p/46331/)

### 4.intersect VCF files

```bash
bcftools isec  -p output_dir  A.vcf.gz B.vcf.gz
```

- `output_dir/0000.vcf.gz` would be variants unique to A.vcf.gz
- `output_dir/0001.vcf.gz` would be variants unique to B.vcf.gz
- `output_dir/0002.vcf.gz` would be variants shared by A.vcf.gz and B.vcf.gz as represented in A.vcf.gz
- `output_dir/0002.vcf.gz` would be variants shared by A.vcf.gz and B.vcf.gz as represented in B.vcf.gz

Ref: [Question: intersect VCF files](https://www.biostars.org/p/178146/)

### 5.merge VCF files

When we say **merge** VCF files, it means that all genotype of one snv/indel will be merged in single line.

A.vcf

```
 ##fileformat=VCFv4.1 
 ##FILTER=<ID=PASS,Description="Passed all filters">
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S1  S2  S3
 10  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 11  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1
 12  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
 13  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1
```

B.vcf

```
 ##fileformat=VCFv4.1 
 ##FILTER=<ID=PASS,Description="Passed all filters">
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S4  S5  S6
 10  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 11  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1  .  1/1
 12  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 1/1 
 13  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
```


merged.vcf

```
 ##fileformat=VCFv4.1 
 ##FILTER=<ID=PASS,Description="Passed all filters">
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S1  S2  S3 S4  S5  S6
 10  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1 0/1 0/0 0/1
 11  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1 0/1  .  1/1
 12  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0 0/0 0/0 1/1 
 13  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1 0/0 0/0 0/0
```



bcftools only accept zipped vcf files. `-Oz` is the output format of `.gz`

```bash
bcftools merge *vcf.gz -Oz -o Merged.vcf.gz
```

Ref:[Question: Best way to merge multiple VCF files](https://www.biostars.org/p/311621/)



### 6.Concatenate or combine or append VCF files

All source files must have the same sample columns appearing in the same order. The program can be used, for example, to concatenate chromosome VCFs into one VCF, or combine a SNP VCF and an indel VCF into one. The input files must be sorted by chr and position. 


A.vcf

```
 ##fileformat=VCFv4.1 
 ##FILTER=<ID=PASS,Description="Passed all filters">
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S1  S2  S3
 10  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 11  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1
 12  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
 13  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1
```


B.vcf

```
 ##fileformat=VCFv4.1
 ##FILTER=<ID=PASS,Description="Passed all filters">  
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S1  S2  S3
 14  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 15  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1
 16  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
 17  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1
```

concat.vcf

```
 ##fileformat=VCFv4.1
 ##FILTER=<ID=PASS,Description="Passed all filters">
 ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Read Depth">
 ##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
 #CHROM POS ID  REF ALT QUAL    FILTER  INFO    FORMAT  S1  S2  S3
 10  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 11  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1
 12  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
 13  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1
 14  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 0/0 0/1
 15  .   C   A   .   .   DP=3;CALLER=Samtools    GT  .   .   1/1
 16  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/0 0/0 0/0
 17  .   C   A   .   .   DP=3;CALLER=Samtools    GT  0/1 1/1 1/1
```


```bash
bcftools concat A.vcf.gz B.vcf.gz -Oz -o output.vcf.gz 
````


Ref:[Question: How to merge vcf files with different variants but same samples?](https://www.biostars.org/p/312024/)
