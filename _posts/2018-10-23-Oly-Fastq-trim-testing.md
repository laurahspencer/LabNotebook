---
layout: post
title: O. lurida RNAseq data fastqc & trim testing
---

Today I downloaded RNASeq data - four fastq files - from Olympia oyster pooled gonad. The gonad was from Fidalgo Bay and Oyster Bay oysters following a 2017 low pH exposure. I unzipped the files, then tested a couple methods of trimming and plotting quality scores for trimmed/untrimmed files. 

Jupyter notebook to download/trim files:  [Inspecting fastq files.ipynb](https://github.com/fish546-2018/laura-quantseq/blob/master/notebooks/Inspecting%20fastq%20files.ipynb)
 
RMarkdown notebook to run a program to extract and plot quality scores against bp for trimmed/untrimmed files: [RNASeq-screening.md](https://github.com/fish546-2018/laura-quantseq/blob/master/notebooks/RNASeq-screening.md)

I also tested out the Fastqc program to get another look at the read quality.  Here are screen shots from the pre-trimmed read quality: 

The 1st "read" from the lane denoted by 0343: 

![fastq 1](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_1.png?raw=true)
![fastq 2](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_2.png?raw=true)
![fastq 3](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_3.png?raw=true)
![fastq 4](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_4.png?raw=true)
![fastq 5](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_5.png?raw=true)
![fastq 6](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_6.png?raw=true)
![fastq 7](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_7.png?raw=true)

The 2nd "read" from the lane denoted by 0343:  
![fastq 8](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_8.png?raw=true)
![fastq 9](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_9.png?raw=true)
![fastq 10](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_10.png?raw=true)
![fastq 11](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_11.png?raw=true)
![fastq 12](https://github.com/fish546-2018/laura-quantseq/blob/master/data/fastqc/Snip20181023_12.png?raw=true)
