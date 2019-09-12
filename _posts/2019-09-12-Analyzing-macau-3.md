---
layout: post
title: Analyzing MACAU results, take 3
---

New and improved with the following: 
- Included a False Discovery Rate correction as per this paper [doi:10.3390/genes10050356](10.3390/genes10050356)   
- 2 heat maps created with % methylation:  
 1) excluding loci for individual samples where coverage <5x (retained for other samples), and  
 2) excluding loci for all samples if any had <5x coverage  
- Barplot of lengths in same order as 2nd heat map  

See new RMarkdown notebook: [006-analyzing-MACAU-results-rev1](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/06-analyzing-MACAU-results-rev1.html) 

_Preview of new plots_

![image](https://user-images.githubusercontent.com/17264765/64827023-a35b3980-d577-11e9-9838-5896477db188.png)
