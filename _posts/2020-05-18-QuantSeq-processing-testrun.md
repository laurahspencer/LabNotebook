---
layout: post
title: Oly RNA - Data processing test-run
---

Things to note about the QuantSeq libraries:
- I used the [QuantSeq 3' mRNA-Seq Library Prep Kit FWD for Illumina](https://www.lexogen.com/quantseq-3mrna-sequencing/#quantseqdescription)  
- Reads are therefore stranded - generated from forward (FWD) strand  
- The kit does not enrich for poly(A) or ribosomal RNA (rRNA) before 1st strand synthesis  
- 1st strand synthesis occurs near the 3' (poly(a)) end of the strand via oligodT priming. Therefore reads are more likely to contain the 3' end of the sequence. Also, reads are  less likely to contain exon junction sites, and are not randomly distributed across the mRNA sequence. Thus, data does not likely contain isoform information (does not represent the whole gene), and is more likely to appear as "duplicated" since reads are more likely to be pulled from the same start location.  
- Library insert lengths averaged ~250bp, but sequencing length was 100bp. Since adapter sequences & poly-a tails can be up to ~25 total, all raw reads contain ~75-85bp of actual mRNA sequence information.  
- Sequencing was performed on a NovaSeq, and was single-end.  

## Data processing test-run on 8 samples 
I'm developing my pipeline based on 8 of the total 144 samples. Of the 8 test samples, 2 are from adult gill, 2 from juvenile whole-body, and 4 are from larval samples (sample numbers are 141, 159, 302m, 331, 43, 441, 483, and 563). Here is my bioinformatics pipeline.

NOTE: for each of STAR, Bowtie2, and Salmon I tested various combinations of settings to try to optimize alignment rate. More details on that later. 

![Bioinformatics-process1](https://user-images.githubusercontent.com/17264765/82283420-32142780-994b-11ea-81d8-69e08f1c2009.png)
![Bioinformatics-process2](https://user-images.githubusercontent.com/17264765/82283426-350f1800-994b-11ea-8668-d79c660cdf27.png)
![Bioinformatics-process3](https://user-images.githubusercontent.com/17264765/82283760-22e1a980-994c-11ea-9e5e-b55ba8a66920.png)
![Bioinformatics-process4](https://user-images.githubusercontent.com/17264765/82283763-24ab6d00-994c-11ea-8346-50550b371eec.png)
![Bioinformatics-process5](https://user-images.githubusercontent.com/17264765/82283455-48ba7e80-994b-11ea-9545-9b52ecaa5595.png)

## Some notes on mapping QC   
  - % mapped reads - higher when mapping to genome. E.g. for human samples, 70-90% of reads typically map to human genome (lower mapping rate expected for transcriptomes).  
  - k-mer and GC content may reveal PCR biases. k-mer and GC content varies by species/experiment, BUT should be homogenous across samples in the same experiment    
  - Could check if rRNA and small RNAs are present - should not be. R packages NOISeq or EDASeq provide useful plots for QC of count data.  
  - Could try Picard for mapping quality control. Try using the `CollectMultipleMetrics` function. This does the following: 
  
_Collect multiple classes of metrics. This 'meta-metrics' tool runs one or more of the metrics collection modules at the same time to cut down on the time spent reading in data from input files. Available modules include CollectAlignmentSummaryMetrics, CollectInsertSizeMetrics, QualityScoreDistribution, MeanQualityByCycle, CollectBaseDistributionByCycle, CollectGcBiasMetrics, RnaSeqMetrics, CollectSequencingArtifactMetrics, and CollectQualityYieldMetrics._  

 

