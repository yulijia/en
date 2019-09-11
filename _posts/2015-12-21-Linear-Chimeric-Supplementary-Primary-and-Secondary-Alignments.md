---
published: true
layout: post
title: "Linear, Chimeric, Supplementary, Primary and Secondary Alignments"
author: Yu
categories: Bioinformatics
tags:
- Linear Alignment
- Chimeric Alignment
- Supplementary Alignment
- Primary Alignment
- Secondary Alignment
- samtools
---

### What is the difference between the title mentaioned alignment reads?

**Linear Alignment:** An alignment of a read to a single reference sequence that may <q>include insertions,
deletions, skips and clipping</q>, <u>but may not include direction changes</u> (i.e. one portion of the alignment
on forward strand and another portion of alignment on reverse strand). [^1]

**Chimeric Alignment:** An alignment of a read that cannot be represented as a linear alignment.
Typically, one of the linear alignments in a chimeric alignment is considered the "representative" alignment, and the
others are called "supplementary" and are distinguished by the supplementary alignment flag. [^1]

Chimeric reads are indicative of structural variation in DNA-seq and it may indicate the presence of [chimeric genes](https://en.wikipedia.org/wiki/Chimeric_gene) in RNA-seq. [^2]

In short, chimeric reads can be split in to two or more parts, each part would be mapped to reference(it's not [hard-clipped](https://www.biostars.org/p/119537/)), the total length of the mapped part is longger than read length. [^3]

**Representative alignment:** A chimeric alignment that is represented as a set of linear alignments that do not have large overlaps typically has one linear alignment that is considered the representative alignment. [^4]

<del>I don't understand representative alignment with the word "representative" in my mother tongue and could not find more information(figure) about it. </del>
One read can align to multiple positions, we can find one alignmnet position which sequence do not have large overlaps, it called representative alighment, for other alignment positions, we called them supplementary alignment.

It seems that GATK can realignment those representative reads to the correctly position via <q>RealignerTargetCreator and IndelRealigner</q>. (WARNING: I am not quite sure if I understand this correctly. If someone could help me, please leave me a message below, thanks, thanks.)


**Supplementary Alignment:** A chimeric reads but not a representative reads.

**Primary Alignment and Secondary Alignment:** A read may map ambiguously to multiple locations, e.g. due to repeats. **Only one of the multiple read alignments is considered primary**,<u> and this decision may be arbitrary</u>. All other alignments have the secondary alignment flag. [^5]


### How to filter those reads?

I usually use samtools to filter those reads. [Picard tools website](http://broadinstitute.github.io/picard/explain-flags.html) provide a online SAM flags explaination tool.

Using `samtools view -f/-F` command can `filter`/`filter out` the reads with specific flags.




**Reference:**

[^1]: [Sequence Alignment/Map Format Specification - Samtools](https://samtools.github.io/hts-specs/SAMv1.pdf "SAMv1.pdf")

[^2]: [What are chimeric reads?](https://www.biostars.org/p/116201/#116203)

[^3]: [Not primary vs Supplementary alignment](http://seqanswers.com/forums/showthread.php?t=40239)

[^4]: [IGV: Filter supplementary alignments](https://software.broadinstitute.org/software/igv/Preferences)

[^5]: [IGV: Filter secondary alignments](https://software.broadinstitute.org/software/igv/Preferences)
