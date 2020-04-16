---
published: true
layout: post
title: "Bam/FASTQ file mapping statistics"
author: Yu
categories: Bioinformatics
tags:
- Bam
- mapping
- samtools
- bedtools
- FASTQ
---

This article aim to help me to remember mapping statistics method.

#### 1.cleaned reads number

~~~
samtools view -c aligned_reads.bam
~~~

cleaned reads base = cleaned reads number * reads length

#### 2.mapped reads number

~~~
samtools view -F 0x04 -c aligned_reads.bam
~~~

count of unmapped reads number = cleaned reads number - mapped reads number

#### 3.unmapped reads number

~~~
samtools view -f4 -c aligned_reads.bam
~~~

#### 4.Sequenced exon/gene number

~~~
samtools bedcov exon_region.bed/gene_region.bed aligned_reads.bam 
~~~
<!--
#### 5.coverage region and reads depth

bedtools coverage  (aka coverageBed)

~~~
#coverageBed -abam aligned_reads.bam -b region.bed -hist > bam.cov
bedtools coverage -abam aligned_reads.bam -b region.bed -hist > bam.cov
~~~

```bash
cat bam.cov | grep all | awk '{SUM += $2*$3;all=$4} END {print SUM/all}' > reads.depth

cat bam.cov | grep all | head -n 1 | awk '{print 1-$5}' > reads.1Xcoverage
```
-->


#### 5.read depth

Updated on Jan 13, 2020

```bash
samtools depth  *bamfile*  |  awk '{sum+=$3} END { print "Average = ",sum/NR}'
```

#### 6.bam tags 

|Tag|Meaning|
|:----:|:----:|
|NM|Edit distance|
|MD|Mismatching positions/bases|
|AS|Alignment score|
|BC|Barcode sequence|
|X0|Number of best hits|
|X1|Number of suboptimal hits found by BWA|
|XN|Number of ambiguous bases in the reference|
|XM|Number of mismatches in the alignment|
|XO|Number of gap opens|
|XG|Number of gap extentions|
|XT|Type: Unique/Repeat/N/Mate-sw|
|XA|Alternative hits; format: (chr,pos,CIGAR,NM;)*|
|XS|Suboptimal alignment score|
|XF|Support from forward/reverse alignment|
|XE|Number of supporting seeds|


#### 7. Counting Number Of Bases In A Fastq File

```bash
zcat data.clean.fq.gz | paste - - - - | cut -f 2 | tr -d '\n' | wc -c 
```


#### Reference

- [SAM and BAM filtering oneliners](https://gist.github.com/davfre/8596159)
