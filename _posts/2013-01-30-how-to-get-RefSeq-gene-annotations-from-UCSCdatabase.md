--- 
published: true
title: How to get RefSeq gene annotations from UCSC database?
layout: post
categories:
- HowTo
- Bioinformatics
tags: 
- annotation
- RefSeq
- UCSC

---
I googled the question, the best answer is on this [page](https://lists.soe.ucsc.edu/pipermail/genome/2012-April/029059.html "[Genome] how could I get RefSeq gene annotations from UCSC	database?")

>You can do this by using our Table Browser. If you're unfamiliar with the Table Browser, please see the User's Guide at
http://genome.ucsc.edu/goldenPath/help/hgTablesHelp.html.   
1. From http://genome.ucsc.edu, select "Tables" from the blue navigation bar at the top of the screen.  
2. Select the following options:    
Clade: Mammal   
Genome: Human   
Assembly: Feb. 2009 (GRCh37/hg19)   
Group: Genes and Gene Prediction Tracks  
Track: RefSeq Genes     
Table: refGene  
3. If you have a list of RefSeq IDs that you want annotated, on the "identifiers" line, click the "paste list" or "upload list" button to use your list of identifiers. If you want the annotation for all genes in a specific region, on the "region" line, you can click "genome" for the entire genome, "position" to define a single specific region or "define regions" to define multiple regions.    
4. Set "output" to "all fields from selected table" to list all fields in your output or to "selected fields from primary or related tables" to select only certain fields in your output   
5. Click the "get output" button (if you selected "selected fields from primary or related tables" in step 4, the next screen will allow you to select the fields you want in your output)  


