---
layout: post
title: QTL analysis with tensorqtl
--- 

I'm exploring ways to run QTL analyses (quantitative trait loci). The goal is to identify alleles/SNPs/genotypes that are associated with traits in the juvenile Pacific cod. I have genotypes pulled from liver RNASeq data, genotype likelihoods from lcWGS (depth=3x), and several traits (growth rates, body and liver condition, liver lipid content). I also have a "composite performance index" that I generated from scaled versions of those traits. I am exploring whether genotype likelihoods (GLs) can be used, too. I got the thumbs up from the tensorqtl folks, indicating that if I format them correctly it shouldn't be a problem.  I don't have a ton of RNASeq-derived SNPs, and many sites have missing data, but it's worthwhile to use it for pipeline development and possibly to identify a few genes that contain variants that may influence traits. QTL studies typically have loads of individuals (hundreds to thousands). Here we just have genotypes for 20 individuals per temperature treatment from RNASEq data, and likelihoods from ~40/temperature treatment. 

I'm trying out the program [tensorqtl](https://github.com/broadinstitute/tensorqtl). 

Giles installed it for me in a mamba environment on Sedna, which I can access using `mamba activate tensorqtl

I'm also interested in using neural network models/approaches to identify Pacific cod sex markers. This tutorial is perhaps a good place to start: https://github.com/miguelperezenciso/DLpipeline

Testing out tensorqtl with RNA-seq derived genotypes
``` 
mamba activate tensorqtl-1.0.10

#NOTE: Giles said, "One note, make sure to set R_LIBS_USER to blank, don't have it pointing at your own R library #folder when you go to run this, it will break things." That variable is indeed set to my R library: 

echo $R_LIBS_USER
/home/lspencer/R/library

#So before running tensorqtl I did this
R_LIBS_USER=

# Import things 
import pandas as pd
import tensorqtl
from tensorqtl import genotypeio, cis, trans

# Make bed from RNASeq-derived VCF with genotypes
plink --make-bed --keep-allele-order --allow-extra-chr --vcf /home/lspencer/pcod-juv-temp/variants/pcod_rnaseq_genotypes-filtered-true.vcf.gz --out /home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes

PLINK v1.90b6.23 64-bit (28 May 2021)          www.cog-genomics.org/plink/1.9/
(C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
Logging to /home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes.log.
Options in effect:
  --allow-extra-chr
  --keep-allele-order
  --make-bed
  --out /home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes
  --vcf /home/lspencer/pcod-juv-temp/variants/pcod_rnaseq_genotypes-filtered-true.vcf.gz

95104 MB RAM detected; reserving 47552 MB for main workspace.
--vcf: 143k variants complete.
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes-temporary.bed +
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes-temporary.bim +
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes-temporary.fam
written.
143597 variants loaded from .bim file.
80 people (0 males, 0 females, 80 ambiguous) loaded from .fam.
Ambiguous sex IDs written to
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes.nosex .
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 80 founders and 0 nonfounders present.
Calculating allele frequencies... done.
Total genotyping rate is 0.140819.
143597 variants and 80 people pass filters and QC.
Note: No phenotypes present.
--make-bed to /home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes.bed +
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes.bim +
/home/lspencer/pcod-juv-temp/variants/pcod-rnaseq_genotypes.fam ... done.

