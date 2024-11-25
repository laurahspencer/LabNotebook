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
I previously performed genome imputation without any reference panel/map. I did some reading, and it looks like I could greatly improve imputation accuracy if I provide phased haplotype reference panel, built from other Pacific cod WGS data. I happen to have a lcWGS data from  ~600 reference fish (depth ~3x) AND the big/little fish from 2021 (depth ~14x). I'm going to see if I can use those datasets to build the phased reference panel. 

_NOTE: phased reference panel: genetic variants are assigned to their respective chromosomes, distinguishing which alleles came from one parent versus the other._

NOTE: I tried to use a beagle file (merged to have both ref & big/little data) as a reference panel, but no dice. I need a VCF file, so I'll build that from "scratch" using angsd's doVCF option 

### Run ANGSD to generate VCF with genotype probabilities from reference population and Big/Little 2021 cod 

I'm working in this repon on Sedna:  /home/lspencer/pcod-general/imputation-ref-panel, and ran the below code in the script **angsd-impute-ref-panel.sh.** It produced BCF files for each chromosome. 

```
#!/bin/bash

#SBATCH --cpus-per-task=10
#SBATCH --time=0-20:00:00
#SBATCH --job-name=angsd
#SBATCH --output=/home/lspencer/pcod-general/imputation-ref-panel/angsd-ref-panel_%A-%a.out
#SBATCH --mail-type=ALL
#SBATCH --mail-user=lspencer@noaa.gov
#SBATCH --array=1-24%24

module unload bio/angsd/0.940
module load bio/angsd/0.940

JOBS_FILE=/home/lspencer/pcod-general/imputation-ref-panel/angsdARRAY_input.txt
IDS=$(cat ${JOBS_FILE})

for sample_line in ${IDS}
do
        job_index=$(echo ${sample_line} | awk -F ":" '{print $1}')
        contig=$(echo ${sample_line} | awk -F ":" '{print $2}')
        if [[ ${SLURM_ARRAY_TASK_ID} == ${job_index} ]]; then
                break
        fi
done

angsd -b /home/lspencer/pcod-general/imputation-ref-panel/bamslist-ref-panel.txt -ref /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fa \
-r ${contig}: -out /home/lspencer/pcod-general/imputation-ref-panel/panel_${contig}_polymorphic \
-nThreads 10 -uniqueOnly 1 -doCounts 1 -setMinDepth 700 -remove_bads 1 -trim 0 -C 50 -minMapQ 35 -minQ 30 \
-dobcf 1 -doGlf 2 -GL 1 -doMaf 1 -doMajorMinor 1 -dopost 1 -dogeno 32 \
-minMaf 0.05 -SNP_pval 1e-10 -only_proper_pairs 1
(base) [lspencer@node28 imputation-ref-panel]$ pwd
/home/lspencer/pcod-general/imputation-ref-panel
```
I then concatenated the BCF files into one using `bcftools concat`, then converted to VCF, filtered for minor allele frequency > 5% and max missingness 15%. This produced the file **whole-genome_miss15.vcf.gz**.  There are still some missing genotypes, so here I try to impute those using BEAGLE v5.4:  

```
java -jar /home/lspencer/programs/beagle.v5.4.jar gt=/home/lspencer/pcod-general/imputation-ref-panel/whole-genome_miss15.vcf.gz gp=true impute=true out=/home/lspencer/pcod-general/imputed

beagle.29Oct24.c8e.jar (version 5.4)
Copyright (C) 2014-2022 Brian L. Browning
Enter "java -jar beagle.29Oct24.c8e.jar" to list command line argument
Start time: 11:17 AM PST on 25 Nov 2024

Command line: java -Xmx1136m -jar beagle.29Oct24.c8e.jar
  gt=/home/lspencer/pcod-general/imputation-ref-panel/whole-genome_miss15.vcf.gz
  gp=true
  impute=true
  out=/home/lspencer/pcod-general/imputed
  nthreads=1

No genetic map is specified: using 1 cM = 1 Mb

Reference samples:                    0
Study     samples:                  708

Window 1 [NC_036931.1:47-16349]
Study     markers:                   84

Burnin  iteration 1:           5 seconds
Burnin  iteration 2:           1 second
Burnin  iteration 3:           1 second

Estimated ne:                  5216323
Estimated err:                 6.4e-03

Phasing iteration 1:           0 seconds
Phasing iteration 2:           0 seconds
Phasing iteration 3:           0 seconds
Phasing iteration 4:           0 seconds
Phasing iteration 5:           0 seconds
Phasing iteration 6:           0 seconds
Phasing iteration 7:           0 seconds
Phasing iteration 8:           0 seconds
Phasing iteration 9:           0 seconds
Phasing iteration 10:          0 seconds
Phasing iteration 11:          0 seconds
Phasing iteration 12:          0 seconds

Window 2 [NC_082382.1:8368-26260017]
Study     markers:               17,298

...
```

yada yada yada 


This should produce a VCF file with phased haplotypes, and a log file. I should next assess the phasing quality using tools like SHAPEIT's quality checks or Beagle's output metrics.
