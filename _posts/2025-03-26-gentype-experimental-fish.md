---
layout: post
title: WGSassign Tool - Pcod gentype/ecotype
---

To expand on my previous post ... 

I've been working with Ben, Ingrid, and Laura Timm to analyze and write up several Pacific cod datasets.  One interesting development is that there appears to be multiple 'gentypes" in cod from the wGOA which _may_ be associated with temperature preference. More on that later. However, this raises a major question in my juvenile experimental data - do I have both gentypes? If so, do they respond differently to temperature? To explore this initially I joined genotype probability data (beagle files) from one of Ingrid's studies ("BIOGEO", adult cod) in which there is clear genetic differentiation, with data from my experimental fish and ran PCA. Based on this analysis, it does appear that I have both gentypes! And they are fairly equally distributed among temperature  treatments - Yay! 

![image](https://github.com/user-attachments/assets/081733dd-2c80-4e64-ae1b-3407c18aadfb)
![image](https://github.com/user-attachments/assets/d2c1e307-c771-476b-ae2b-84cb99252ad8)

--- 

Based on how my experimental fish are clustering with the BIOGEO gentypes I assigned them gentypes, then plotted all phenotypic data curves (below, medians) by gentype: 

![image](https://github.com/user-attachments/assets/65df40c5-0ab0-4699-bde8-4a74c19f39c5)

---
Next Steps: 

I would like to develop a WGSassign tool for assigning pcod to gentype 1 or gentype 2.  We can use this to look for gentypes in the EBS fish (from our broader reference set), and it will be useful for gentyping cod from other projects (e.g. temperature experiment). To do so I'll need:   
  -- Beagle and original bams from relevant datasets (BL, TAG, AAcod?)
  -- List identifying gentypes for each sample
  -- All my WGSassign training, testing, and assignment scripts 

Once I am confident that experimental fish are correctly assigned, I want to re-assess phenotypic data: 
  -- Model phenotypic traits by temperature separately for each genotype. Is gentype a significant factor? If so, define optimal temperatures for each trait for each gentype. 
  -- Look at my GWAS results - 
      -- Do the performance-associated sites segregate out by gentype? Plot GWAS boxplots with points by gentype. This would be most interesting if sites are associated with performance in specific temperatures (warm, cold) and have different alleles/heterozygosity in gentypes.
      -- Are any markers highly differentiated in ecotypes (high-fst snps in Ingrid's TAG/Age-0 data)? 
  -- If we DO find any phenotypic differences among gentypes (particularly in lipid or liver data), re-analyze expression data to identify genes/processes that differ among gentypes, either 1) in specific temperatures (e.g. warm, cold), or 2) model genes by temperature and look for those with significant gentype interaction effects.  
  -- Write it up.  Publish. Bam.  
