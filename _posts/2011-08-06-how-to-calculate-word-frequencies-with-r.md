--- 
published: true
title: How to calculate word frequencies with R
layout: post
categories:
- HowTo
- R-Language
tags: 
- R
- word frequencies

---

I haven't check my code for 7 years ago, thanks to all the visitors who left a comment.

==================================


[How to do with R] is a category about use R to deal with problems. Maybe you can use web search find this, when you have the same problems.

I need to calculate "number of times" same word appear in some text documents. There are many ways to do it. One simple way is using R. To calculate word frequencies have three mainly steps.

- Read the text into R.
- Split sentence into words.
- Calculate word frequencies.

**R can read any text file using readLines() or scan().**

```r
readLines("Textname.txt",encoding="UTF-8")
scan("Textname.txt","character",sep="\n")
```
**Reference:** [Reading and writing text files](http://en.wikibooks.org/wiki/R_Programming/Text_Processing#Reading_and_writing_text_files "Reading and writing text files")

**Use *strsplit* to split sentence into words.**

```r
strsplit(string,"")
```
**Reference:** [?strsplit](http://stat.ethz.ch/R-manual/R-patched/library/base/html/strsplit.html "?strsplit")

**Use *table* to calculate word frequencies.**

```r
table(sentences)
```
**Reference:** [?table](http://stat.ethz.ch/R-manual/R-patched/library/base/html/table.html "?table")

## Hear is a Example 

Calculate word frequencies in TEXTname.txt

>TEXTname.txt: It was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair, we had everything before us, we had nothing before us.

```r
sentences<-scan("TEXTname.txt","character",sep="\n");
#Replace full stop and comma
sentences<-gsub("\\.","",sentences)
sentences<-gsub("\\,","",sentences)
#Split sentence
words<-strsplit(sentences," ")
#Calculate word frequencies
words.freq<-table(unlist(words));
```

The source code can be found in [github](https://gist.github.com/1857130 "calculate word frequencies with R")

```r
Result:
cbind(names(words.freq),as.integer(words.freq)) ## You might consider using cbind.data.frame instead of cbind

  [1,] "age" "2"
  [2,] "before" "2"
  [3,] "belief" "1"
  [4,] "Darkness" "1"
  [5,] "despair" "1"
  [6,] "epoch" "2"
  [7,] "everything" "1"
  [8,] "foolishness" "1"
  [9,] "had" "2"
  [10,] "hope" "1"
  [11,] "incredulity" "1"
  [12,] "it" "7"
  [13,] "It" "1"
  [14,] "Light" "1"
  [15,] "nothing" "1"
  [16,] "of" "8"
  [17,] "season" "2"
  [18,] "spring" "1"
  [19,] "the" "8"
  [20,] "us" "2"
  [21,] "was" "8"
  [22,] "we" "2"
  [23,] "winter" "1"
  [24,] "wisdom" "1"
```
