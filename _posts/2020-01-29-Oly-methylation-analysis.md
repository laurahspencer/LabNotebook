---
layout: post
title: Oly methylation analysis, Jan. 29th 2020 
--- 

### From the last meeting with Katherine and Steven, these were my tasks: 

* Re-do MACAU with pre-filtered count files <--- DONE
* Reannontate new MACAU results <--- DONE 
* Reannonate DMLs 
* DMG analysis 
* Figure out something comparable to Fst on DMLs and MACAU to do a correlation analysis 
* GO_MWU analysis redo with genes: https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/GO_MWU.ipynb
* New methylation distance matrix to include just loci with new 75% threshold 
* Get manhattan distance for just DMLs 
* Get methods down on paper ASAP 

---

### I did successfully re-run MACAU twice using two different filter settings (10x across 75% of samples, and 5x across 75% of samples). 

First I added code to the [04-raw-count-files.Rmd](https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/04-raw-count-files.Rmd) notebook to save new filtered count files. 

    # Filter count dataframes for loci where at least 75% of samples have 10x and 5x coverage or greater 

    # Total counts 
    counts.tot.destrand.10x75 <- counts.tot.destrand[rowSums(counts.tot.destrand[,-1] > 9) > 13,] # 10x or greater in 14 out of the 18 samples 
    counts.tot.destrand.5x75 <- counts.tot.destrand[rowSums(counts.tot.destrand[,-1] > 4) > 13,]  # 5x or greater in 14 out of the 18 samples 

    # Get methylation counts for loci identified in the above 10x75 total counts files 
    counts.meth.destrand.10x75 <- subset(counts.meth.destrand, siteID %in% counts.tot.destrand.10x75$siteID) # subset 
    counts.meth.destrand.10x75 <- counts.meth.destrand.10x75[order(match(counts.meth.destrand.10x75$siteID, counts.tot.destrand.10x75$siteID)), ] #reorder to match total count df
    sum(counts.tot.destrand.10x75$siteID != counts.meth.destrand.10x75$siteID) #confirm order is same 

    # Get methylation counts for loci identified in the above 5x75 total counts files 
    counts.meth.destrand.5x75 <- subset(counts.meth.destrand, siteID %in% counts.tot.destrand.5x75$siteID)
    counts.meth.destrand.5x75 <- counts.meth.destrand.5x75[order(match(counts.meth.destrand.5x75$siteID, counts.tot.destrand.5x75$siteID)), ]
    sum(counts.tot.destrand.5x75$siteID != counts.meth.destrand.5x75$siteID) #confirm order is same 

    nrow(counts.tot.destrand.10x75) == nrow(counts.meth.destrand.10x75) #confirm same # rows 
    nrow(counts.tot.destrand.5x75) == nrow(counts.meth.destrand.5x75)   #confirm same # rows 

---

### I then re-ran MACAU twice; see the bottom half of this Jupyter notebook, [05-MACAU.ipynb](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/05-MACAU.ipynb). Here's the code I used with the 5x coverage datasets: 

![image](https://user-images.githubusercontent.com/17264765/73411169-3d1e4b80-42b9-11ea-9277-093f114b5506.png)

I corrected for multiple comparisons using a false discovery rate of 10%, then calculated % methylation for resulting MACAU loci and created heat map figures. All told there were 71 MACAU-identified loci using the 10x coverage threshold, and 146 loci using the 5x coverage threshold. Please see my notebook for more details - [06-analyzing-MACAU-results-rev2](https://htmlpreview.github.io/?https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/06-analyzing-MACAU-results-rev2.html?raw=true) - but here is a heatmap showing loci assocated with shell length and ordered by shell length, using the 10x coverage. RED = Hood Canal, BLUE = South Sound.  [analyses/macau/macau-heatmap.10x75.length.pdf](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/analyses/macau/macau-heatmap.10x75.length.pdf): 

![image](https://user-images.githubusercontent.com/17264765/73413637-211ea800-42c1-11ea-8559-4da9a189f15a.png)

---

### I used BEDtools to annotate the MACAU loci and DMLs with the Oly gene tracks. 

- Gene tracks are located in this repo in the [genome-features/](https://github.com/sr320/paper-oly-mbdbs-gen/tree/master/genome-features) folder.  
- Annotation was performed in Jupyter, see the notebook: [07-DML-MACAU-annotation.ipynb](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/07-DML-MACAU-annotation.ipynb).  

**NOTE:** The DMLs were identified in the [01-methylkit.Rmd](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/01-methylkit.Rmd) notebook, and used the following MethylKit function and options:     

    `myDiff25p=getMethylDiff(myDiff,difference=25,qvalue=0.01)`

This selected for bases that have q-value<0.01 and percent methylation difference larger than 25%. Previously, data had been filtered such that we discarded bases with coverage below 10X and above 100x in each sample. We then destranded the data. **We should confirm that these are the criteria that we want.**    

**NOTE:** While reviewing the [01-methylkit.Rmd](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/01-methylkit.Rmd) notebook, I was reminded that the %CpG Methylation for our samples skews very high, which runs counter to our observation that inverts are hypomethylated. This raises the question - how has MethylKit processed our data? Does our MethylKit object, `myobj_18`, only retain loci that were methylated? If so, that's not a bad thing, it simply needs to be considered in our analysis / summary.  Here is a histogram of the %CpG methylation for one sample (#17): 

![image](https://user-images.githubusercontent.com/17264765/73415418-aeb0c680-42c6-11ea-95ae-5476abecdf7e.png)

--- 

### I then summarized and visualized genomic locations of MACAU loci and DMLs. 

See my RMarkdown notebook - [08-Annotations.Rmd](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/08-Annotations.html). 

#### All methylated loci are located here: 
- Genes: 127584 
- Coding sequences: 90224  
- Exons: 94869  
- mRNAs: 127584  
- Transposable elements: 15832  

#### DMLs are located here: 
- Genes: 32  
- Coding sequences: 25  
- Exons: 27  
- mRNAs: 32  
- Transposable elements: 3  

![image](https://user-images.githubusercontent.com/17264765/73415349-701b0c00-42c6-11ea-890a-21c0df244efb.png)

#### MACAU loci are located here: 
- Genes: 35  
- Coding sequences: 23  
- Exons: 34  
- mRNAs: 35  

![image](https://user-images.githubusercontent.com/17264765/73415377-84f79f80-42c6-11ea-9cd6-39b748d0f252.png)


