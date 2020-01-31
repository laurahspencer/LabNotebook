---
layout: post
title: Oly methylation analysis, Jan 31st, 2020 
--- 

Revisitiong my task list: 

#### New methylation distance matrix to include just loci with new 75% threshold <- not needed   

I reviewed Steven's MethylKit and he had already took necessary steps to select only loci with high coverage across samples. 

-- Upon calling methylation status for each locus (using sorted BAM files, function `processBismarkAln`) he set the "minimum read coverage to call a methylation status for a base" to 2.  
-- Then, he used the `filterByCoverage` function to filter out bases with coverage <10x or >100x. This was done on each locus within each sample, and did not discard loci that had <10x across all samples. 
-- However, he then used the `unite` function to "merge all samples to one object for base-pair locations that are covered in all samples ... The [resulting] methylBase object contains methylation information for regions/bases that are covered in all samples." This step in particular should control for genetic differences that could influence methylation status.  

#### Re-do MACAU <- DONE 
- With pre-filtered count files (5x and 10x across 75% of samples). NOTE: to generate my count files I had used the same filter settings as Steven (`low.count=10`, `hi.count=100`.  
- Reannontated new MACAU results 

#### Reannonate DMLs <- not needed 
Wasn't necessary, since the DMLs were already filtered.  

#### DMG analysis <- DONE 
- Figure out something comparable to Fst on DMLs and MACAU to do a correlation analysis
- GO_MWU analysis redo with genes: https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/GO_MWU.ipynb

#### Get manhattan distance for just DMLs <- DONE
- This is saved as a .csv file on github here: [paper-oly-mbdbs-gen/analyses/dist.manhat.DMLs.csv](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/analyses/dist.manhat.DMLs.csv)  
- Note: the number of DMLs is 
- Out of curiosity, I did a PCA on the DML percent methylation distance matrix with the package `FactoMineR::PCA()` 
![image](https://user-images.githubusercontent.com/17264765/73574877-ab831b00-442b-11ea-9fc1-7fa0a499930e.png)

## To do still  
- GO_MWU enrichment analysis redo with genes: https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/GO_MWU.ipynb  
- Figure out something comparable to Fst on DMLs and MACAU to do a correlation analysis   
- Get methods down on paper  

