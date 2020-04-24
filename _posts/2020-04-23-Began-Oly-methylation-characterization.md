---
layout: post
title: April 23, 2020 - Oly methylation characterization 
---

Met with Katherine and Steven a couple weeks ago and updated them on my DML, DMG, and size-associated loci (SAL) analyses. I am testing out using RMarkdown to write my results, so check out this notebook entry for a summary of these activities: 

https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/10-Results.html

One thing I was missing was a general characterization of O. lurida methylation patterns. This is what I tackled the past couple days. To do so, I merged methylation data from all 18 samples (18 .bam files) into one .bam files, and ran that through MethylKit for some quick summary stats, then called methylation status using 50% threshold, filtered for 5x, and annotated using the O. lurida feature files. This is all posted in my RMarkdown notebook, [01b-General-Methylation-Patterns.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/01b-General-Methylation-Patterns.html). 

Here are barplots showing the % of methylated loci that overlap with genome features; note that I should also include a bar showing where all CpG loci are:  

![image](https://user-images.githubusercontent.com/17264765/80166254-5a01ac80-8592-11ea-9be1-36933338ba7e.png)

One interesting observation from this analysis is that our filtering is excluding most of the 0% methylated loci. Check out the below % methylation frequency plots: 
  1. When all samples are combined into one file, and we filter for only 2x coverage, we get a typical peak at 0% methylation. We also see a small peak at 50% methylation, probably b/c there are a lot of loci with 2x coverage, and 1 of the 2 reads was methylated.  
  2. When we filter for 5x coverage (again, with all samples merged), the 0% methylation peak dramatically reduces.  
  3. When filtered for 10x coverage, the 0% peak nearly disappears.  

This explains why we don't have much 0% methylation in our data - low coverage for un-methylated loci. This may mean that our analyses omits loci where methylation is substantially different between populations (i.e. 100% vs. 0% methylated). But, can we infer that low-coverage is a result of low methylation, or just poor coverage? Will dig into this further.  

![meth_coverage](https://user-images.githubusercontent.com/17264765/80166112-ff685080-8591-11ea-92bd-137b69d5204e.png)
