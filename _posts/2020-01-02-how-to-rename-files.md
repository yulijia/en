---
published: ture
layout: post
title: "How to rename files"
author: Yu
categories: HowTo
tags:
- rename
- Linux
- Bash
---

There are usually five purposes for rename a file (multiple files):

1. Replace (delete/remove) the spaces in a filename
2. Rename the suffix of a file
3. Add date in a filename
4. Uppercase/lowercase first letter(all letters) in a filename
5. Replace a specific part of a filename


Here, I will show you some script to do all jobs.

### 1. Replace (delete/remove) the spaces in a filename

```bash
## generate some files with spaces in the filenames
touch "A filename with space.txt"
touch "B filename with space.txt"

## Replace the space with underline
for filename in *\ *; 
do
  mv "$filename" "${filename// /_}"
done

## Delete the space in a filename
for filename in *\ *; 
do
  mv "$filename" "${filename// /}"
done
```

### 2. Rename the suffix of a file

```bash
## generate some files
touch "A.txt"
touch "B.txt"

for filename in *.txt;
do
  mv "$filename" "${filename//.txt/.TXT}"
done
```

### 3. Add date in a filename

```bash
## generate some files
touch "A.txt"
touch "B.txt"

DATE=`date +%F` 
for filename in *.txt;
do 
  mv "$filename" "$DATE-$filename"
done
```


### 4. Uppercase/lowercase first letter(all letters) in a filename

```bash
## generate some files
touch "aaaapple.txt"
touch "bbbbanana.txt"

## Uppercase first letter
for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename"  ${fn^}.${filename#*.}
done

## Lowercase first letter
for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename"  ${fn,}.${filename#*.}
done

## From lowercase to uppercase
for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename" ${fn^^}.${filename#*.}
done

## From uppercase to lowercase
for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename"  ${fn,,}.${filename#*.}
done

## Highlight character p in the filename
for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename"  ${fn^^p}.${filename#*.}
done
```

### 5. Replace a specific part of a filename

```bash
## generate some files
touch "abc.txt"
touch "bcd.txt"

for filename in *.txt;
do 
  fn="${filename%.*}"
  mv "$filename"  ${fn/bc/ZZZ}.${filename#*.}
done
```

Q.E.D.
