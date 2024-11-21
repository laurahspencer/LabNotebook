---
layout: post
title: Refining growth/condition marker discovery in Pacific cod, GWAS Day 4
--- 

I am interested to see whether I can use the putative growth/condition markers detected in experimental Pacific cod to predict "performance" in our reference/wild caught cod from different marine regions (Northern Bering Sea, Aleutians, Eastern Bering Sea / western Gulf of Alaska, eastern Gulf of Alaska). I have a beagle file containing genotype likelihoods for sites detected in both experimental and reference fish. I filtered that beagle file for only our putative growth/performance markers, then ran PCAngsd to generate a covariance matrix, then PCA in R. My thought is that I could potentially use a growth/condition gradient along a PC axis from experimental fish to identify reference fish populations/regions that lie on the high growth/condition end (i.e. potential high performers). 

Here is the PCA of those ~270 putative growth/condition markers with growth/condition index of experimental fish indicated by the gradient, and all reference fish in gray. Second figure is a regression of growth/condition index ~ PC1 scores for experimental fish only, which I could possibly use to potentially predict performance in reference fish - the more negative PC1 score the better?. The boxplot shows PC1 scores for reference fish, potentially indicating eGOA fish have the most negative PC1 score / best performance indicator and tagged/NBS fish have the highest PC1 scor. 

![image](https://github.com/user-attachments/assets/e1a235ec-4f58-48e3-840f-035b96edf113)

![image](https://github.com/user-attachments/assets/619d5915-c367-4444-8938-4d2abab914d1)

![image](https://github.com/user-attachments/assets/2d8adf08-fb3c-453a-ae2a-723d0c20a6bc)

#### What's happening here
This is strange, though. When I plot PC1 scores for experimental fish alongside reference fish, obviously the variability is much higher in experimental fish. BUT also - there are differences among temperature treatments, which isn't a good sign. I believe this indicates that the marker discovery is being influenced by growth/condition differences among treatments. This suggests to me that I need to run my GWAS analyses separately for each temperature and look for 1) common markers among treatments that could indicate general growth/condition performance, and 2) unique markers among treatments that could reflect temperature-specific markers.  

![image](https://github.com/user-attachments/assets/7cc69e5c-470f-4df3-a0c5-bc865fb47475)

To Do: 
- Confirm genomtype imputation method - _do I need reference panel for this?_
- Re-run GWAS separately for each temperature group  
- Re-assess via PCA for warm, optimal, and cold. 

### Genome imputation
I previously performed genome imputation without any reference panel/map. I did some reading, and it looks like I could greatly improve imputation accuracy if I provide phased haplotype reference panel, built from other Pacific cod WGS data. I happen to have a merged beagle file containing genotype probabilities from all reference fish (depth ~3x) AND the big/little fish from 2021 (depth ~14x), **/home/lspencer/BLcod/wgsassign/refs-BLcod-merged.beagle.gz**. I'm going to see if I can use that to build the phased reference panel. 

_NOTE: phased reference panel: genetic variants are assigned to their respective chromosomes, distinguishing which alleles came from one parent versus the other._

ENCOUNTERING AN ISSUE - looks like I might need a VCF as input for the phased reference panel. I might need to build that from "scratch" using angsd's doVCF option 


Here I use Beagle to produce produce a phased reference panel: 
```
java -jar beagle.jar gt=/home/lspencer/BLcod/wgsassign/refs-BLcod-merged.beagle.gz ref=ref_genome.fa out=phased_reference
This will produce:

phased_reference.vcf.gz: A VCF file with phased haplotypes.
phased_reference.log: Log file.
Check Phasing Quality:

Assess the phasing quality using tools like SHAPEIT's quality checks or Beagle's output metrics.

/home/lspencer/BLcod/wgsassign/refs-BLcod-merged.beagle.gz 
