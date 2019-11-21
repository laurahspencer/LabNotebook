---
layout: post 
title: Oly methylation analysis, Nov 20, 2019
---

Revisiting Oly methylation data. We now have two lists of loci: 
 - 1) DMLs between two Olympia oyster populations, Hood Canal and South Sound, which were identified using MethylKit.   
 - 2) Loci where methylation status is associated with oyster shell length, filtered by a) loci have 10x coverage in all samples, and b) loci have 10x coverage in any sample.   
 
Today I re-plotted heatmaps using MACAU loci, based on feedback from Steven & Katherine: 
- Only use loci with 10x coverage  
- Add heatmaps where samples are NOT ordered by cluster analysis, but instead by 1) tree from MethylKit, and 2) shell length. See my notebook, [06-analyzing-MACAU-results-rev1.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/06-analyzing-MACAU-results-rev1.html), and here's one of the new heatmaps, with samples (columns) ordered by shell length, and a barplot of shell length below (red = Hood Canal oysters, green = South Sound oysters).   

<img align="right" src="https://user-images.githubusercontent.com/17264765/69304438-6e9ddb00-0bd5-11ea-950a-64b24e80fbf4.png"><img align="right" src="https://user-images.githubusercontent.com/17264765/69304447-78bfd980-0bd5-11ea-995a-149f7b6a6b83.png">

Then I used bedtools to see where DMLs and MACAU loci are located, see my notebook here:  
[07-DML-MACAU-annotation.ipynb](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/07-DML-MACAU-annotation.ipynb).  

Finally, I began annotating loci locations for DMLs and MACAU loci; see my notebook: [08-Annotations.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/08-Annotations.html). Here's a barplot showing which features overlap with the DMLs: 
![image](https://user-images.githubusercontent.com/17264765/69304650-3ba81700-0bd6-11ea-976c-0a13fadf11d1.png)

