---
published: true
layout: post
title: "Fold changes"
author: Yu
categories: [Bioinformatics]
tags:
- Fold-change
---

There are many fold-change calculation formulars. 
Since I read the paper [A comparison of fold-change and the t-statistic for microarray data analysis](http://statweb.stanford.edu/~tibs/ftp/FCTComparison.pdf), 
I think I need to summaries each mathods and make sure I understand them.

Because fold-change is widely used in RNA research. 
The background of the fold-change calculation in this post is based on how find the differentially-expressed genes in a experiment.

### The original method

The standard definition of the fold-change is

$$foldchange = \frac{control}{treatment}$$

Note: `control` and `treatment` are the <q>raw expression levels</q>.

### An simple method

$$foldchange = control - treatment$$

In some papers, I found they use deviation of control and treatment as the fold-change result.

If the difference of control and treatment data are not to large, I think we should use the **simple method**.

Some differentially expressed genes have large differences (B-A) but small ratios (A/B), this is another point why using **simple methold** instead of the **original method**.

### log fold-changes

If the difference or ratios of control and treatment is dynamic between genes, we need to scale the range of fold-change result.

Here we need the log fold-changes.

$$foldchange = log_2(\frac{control}{treatment})= log_2(control)-log_2(treatment)$$

### delta-delta-Ct method

$$foldchange = \frac{2^{treatment}}{2^{control}} = 2^{treatment - control}$$

This method is used in qPCR experiment.


DCt: Target Ct - Housekeeping Ct

DDCt: Sample DCt - Calibrator DCt (Calibrator is your group of comparison)

Fold calculus: 2^-DDCt

For more detail, please see:

- [How can I calculate the expression of gene using 2-ddct method?](https://www.researchgate.net/post/How_can_I_calculate_the_expression_of_gene_using_2-ddct_method)
- [What is a Ct value?](http://bitesizebio.com/24581/what-is-a-ct-value/)

If we need the direction of the fold-change trend, we can use the [sign function](https://en.wikipedia.org/wiki/Sign_function).

$$foldchange = SIGN(treatment - control)*2^{\left|treatment - control\right|}$$


