---
layout: post
title: Oly Genetics 104, NF GenePop Analysis
---

Tried to do the html to .md trick for this notebook, but it did not function. No biggie, since there are no pretty plots in this notebook. Original notebooks: [R markdown version, NF-GenePop-Analysis.Rmd](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Notebooks/NF-GenePop-Analysis.Rmd); 
[HTML version, NF-GenePop-Analysis.html](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Notebooks/NF-GenePop-Analysis.html)

### In this notebook I will use the GenePop R program to analyze the 2016/2017 Fidalgo Bay (NF) Ostrea lurida microsatellite data; the results from each analysis are housed in a series of .txt files. 

Prior to importing the data, prepared the 2016/2017 NF data in Excel and exported into GenePop format; resulting file is available on [2018-01-22-Preparing-for-Genepop.md](https://github.com/laurahspencer/O.lurida_genetics/blob/master/Notebooks/2018-01-22-Preparing-for-Genepop.md)

### First, install GenePop program; 

        install.packages("genepop")
        library(genepop)

### Pull basic information on allele and genotype frequencies per locus and per sample 
        
        basic_info(inputFile="Data/Oly2016NFH+2017NFW_Merged.txt", outputFile = "Analyses/NF-Basic-Info.txt", verbose=T)
        
Resulting file: ["NF-Basic-Info.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-Basic-Info.txt)
Hetero- and homozygosity info pasted here: 

### NF Wild   

| Loci                   | Olur10      | Olur11  | Olur12  | Olur13  | Olur15  | Olur19  | 
|------------------------|-------------|---------|---------|---------|---------|---------| 
| Expected Homozygotes   | 4.99        | 24.59   | 11.83   | 6.68    | 6.66    | 6.64    | 
| Observed Homozygotes   | 2           | 28      | 12      | 7       | 6       | 7       | 
| Expected Heterozygotes | 92.01       | 70.41   | 86.17   | 91.32   | 91.34   | 90.36   | 
| Observed Heterozygotes | 95          | 67      | 86      | 91      | 92      | 90      | 


### NF Hatchery      

| Loci                   | Olur10      | Olur11  | Olur12  | Olur13  | Olur15  | Olur19  | 
|------------------------|-------------|---------|---------|---------|---------|---------| 
| Expected Homozygotes   | 5.02        | 23.03   | 13.44   | 7.14    | 7.22    | 7.10    | 
| Observed Homozygotes   | 7           | 27      | 5       | 6       | 6       | 7       | 
| Expected Heterozygotes | 92.98       | 70.97   | 86.56   | 88.86   | 90.78   | 88.90   | 
| Observed Heterozygotes | 91          | 67      | 95      | 90      | 92      | 89      | 


### Assess whether loci are in Hardy-Weinberg Equilibrium

        test_HW(inputFile = "Data/Oly2016NFH+2017NFW_Merged.txt", which="Proba", outputFile = "Analyses/NF-HWE.txt", enumeration = FALSE, dememorization = 10000, batches = 500, iterations = 2000, verbose = interactive())

Resulting file: ["NF-HWE.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-HWE.txt)
All P-values across loci in each population are >>0.05, do not reject the null hypothesis that all loci are in HWE. 
        
    Pop : NFW-2017
    -----------------------------------------
                                 Fis estimates
                                ---------------
    locus       P-val   S.E.    W&C     R&H     Steps 
    ----------- ------- ------- ------- ------- ------
    Olur10      0.2885  0.0155  -0.0327 -0.0097  22774 switches
    Olur11      0.7817  0.0096   0.0487  0.0823  32358 switches
    Olur12      0.9079  0.0055   0.0020 -0.0093  77677 switches
    Olur13      0.3355  0.0108   0.0035  0.0086  83321 switches
    Olur15      0.3031  0.0111  -0.0073 -0.0015  68376 switches
    Olur19      0.3528  0.0109   0.0040  0.0092  82550 switches
    
    All (Fisher's method):
     Chi2 :    9.8276
     Df   :    12.0000
     Prob :    0.6311
    
    Pop : NFH-2016
    -----------------------------------------
                                 Fis estimates
                                ---------------
    locus       P-val   S.E.    W&C     R&H     Steps 
    ----------- ------- ------- ------- ------- ------
    Olur10      0.6622  0.0165   0.0214  0.0184  19734 switches
    Olur11      0.7706  0.0105   0.0563  0.0169  29904 switches
    Olur12      0.8312  0.0070  -0.0981 -0.0520  86460 switches
    Olur13      0.1129  0.0057  -0.0129 -0.0057 112034 switches
    Olur15      0.0795  0.0057  -0.0135 -0.0123  93919 switches
    Olur19      0.1101  0.0065  -0.0012  0.0012  89921 switches
    
    All (Fisher's method):
     Chi2 :    15.5530
     Df   :    12.0000
     Prob :    0.2126
    ==========================================
     All locus, all populations 
    ==========================================
    All (Fisher's method) :
     Chi2 :    25.3806
     Df   :    24.0000
     Prob :    0.3853

### Assess whether any loci are linked 
       
       test_LD(inputFile = "Data/Oly2016NFH+2017NFW_Merged.txt", outputFile = "Analyses/NF-LD.txt", dememorization = 10000, batches = 100, iterations = 1000, verbose = TRUE)

Resulting file: ["NF-LD.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-LD.txt)
Interesting, results from this Linkage Disequilibrium test indicate that there are, in fact, linked loci:

    Pop             Locus#1  Locus#2    P-Value      S.E.     Switches
    ----------      -------  -------    --------     -------- --------
    NFW-2017        Olur10   Olur11     0.32418      0.045606      216
    NFW-2017        Olur10   Olur12     0.372000     0.048411       65
    NFW-2017        Olur11   Olur12     1.000000     0.000000      435
    NFW-2017        Olur10   Olur13     1.000000     0.000000       42
    NFW-2017        Olur11   Olur13     0.990730     0.006526      279
    NFW-2017        Olur12   Olur13     0.144450     0.034977       90
    NFW-2017        Olur10   Olur15     1.000000     0.000000       33
    NFW-2017        Olur11   Olur15     0.975760     0.014058      232
    NFW-2017        Olur12   Olur15     1.000000     0.000000       96
    NFW-2017        Olur13   Olur15     0.065770     0.024413       35
    NFW-2017        Olur10   Olur19     1.000000     0.000000       35
    NFW-2017        Olur11   Olur19     1.000000     0.000000      229
    NFW-2017        Olur12   Olur19     1.000000     0.000000       91
    NFW-2017        Olur13   Olur19     0.060010     0.023868       16
    NFW-2017        Olur15   Olur19     0.000000     0.000000       28  <------ Wild 15 & 19 linked
    NFH-2016        Olur10   Olur11     1.000000     0.000000      154
    NFH-2016        Olur10   Olur12     0.208400     0.040527       72
    NFH-2016        Olur11   Olur12     0.923220     0.024857      520
    NFH-2016        Olur10   Olur13     1.000000     0.000000       45
    NFH-2016        Olur11   Olur13     0.726700     0.043512      284
    NFH-2016        Olur12   Olur13     0.000000     0.000000      151 <------ Hatchery 12 & 13 linked
    NFH-2016        Olur10   Olur15     1.000000     0.000000       36
    NFH-2016        Olur11   Olur15     0.715690     0.043815      301
    NFH-2016        Olur12   Olur15     0.049120     0.021532      165
    NFH-2016        Olur13   Olur15     0.000000     0.000000       68 <------ Hatchery 13 & 15 linked
    NFH-2016        Olur10   Olur19     1.000000     0.000000       40
    NFH-2016        Olur11   Olur19     0.716270     0.042213      282
    NFH-2016        Olur12   Olur19     0.000000     0.000000      149 <------ Hatchery 12 & 19 linked
    NFH-2016        Olur13   Olur19     0.000000     0.000000       54 <------ Hatchery 13 & 19 linked
    NFH-2016        Olur15   Olur19     0.000000     0.000000       44 <------ Hatchery 15 & 19 linked
    
    P-value for each locus pair across all populations
    (Fisher's method)
    -----------------------------------------------------
    Locus pair                    Chi2      df   P-Value
    --------------------          --------  ---  --------
    Olur10        & Olur11        2.252913  4    0.689355
    Olur10        & Olur12        5.114315  4    0.275768
    Olur11        & Olur12        0.159775  4    0.996974
    Olur10        & Olur13        0.000000  4    1.000000
    Olur11        & Olur13        0.657110  4    0.956511
    Olur12        & Olur13        >35.7544134    <0.000000 <------ 12 & 13 linked
    Olur10        & Olur15        0.000000  4    1.000000
    Olur11        & Olur15        0.718094  4    0.949079
    Olur12        & Olur15        6.026978  4    0.197143
    Olur13        & Olur15        >37.3279524    <0.000000 <------ 13 & 15 linked
    Olur10        & Olur19        0.000000  4    1.000000
    Olur11        & Olur19        0.667396  4    0.955288
    Olur12        & Olur19        >31.8847694    <0.000002 <------ 12 & 19 linked
    Olur13        & Olur19        >37.5112584    <0.000000 <------ 13 & 19 linked
    Olur15        & Olur19        >63.7695394    <0.000000 <------ 15 & 19 linked


### Assess for null alleles 
        
        nulls(inputFile = "Data/Oly2016NFH+2017NFW_Merged.txt", outputFile = "Analyses/NF-null.txt", nullAlleleMethod = "B96", CIcoverage = 0.95, verbose = TRUE)

Resulting file: ["NF-null.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-null.txt)
Null allele frequences low; will compare with results from MicroChecker to confirm. 
    
    (Locus by population) table of estimated null allele frequencies
    ================================================================
    Locus:     Populations (! names truncated to 6 characters):
               NFW-20 NFH-20 
               -----------------------------------------------------
    Olur10     0.0000 0.0079 
    Olur11     0.0125 0.0159 
    Olur12     0.0000 0.0000 
    Olur13     0.0000 0.0000 
    Olur15     0.0000 0.0000 
    Olur19     0.0000 0.0000 
    ================================================================
    
    
    Confidence intervals for null allele frequencies
    =================================================
                           Frequency   0.0250   0.9750 
    Locus      Population   estimate   bound    bound
    -------------------------------------------------
    Olur10     NFW-2017    0.0000     (No info for CI)
               NFH-2016    0.0079     0.0000  0.0422  
    Olur11     NFW-2017    0.0125     0.0000  0.0694  
               NFH-2016    0.0159     0.0000  0.0724  
    Olur12     NFW-2017    0.0000     (No info for CI)
               NFH-2016    0.0000     (No info for CI)
    Olur13     NFW-2017    0.0000     (No info for CI)
               NFH-2016    0.0000     (No info for CI)
    Olur15     NFW-2017    0.0000     (No info for CI)
               NFH-2016    0.0000     (No info for CI)
    Olur19     NFW-2017    0.0000     (No info for CI)
               NFH-2016    0.0000     (No info for CI)
    =================================================


### Exact conditional contingency-table test or genotypic differentiation. 
Assesses the distribution of diploid genotypes in the various populations. The null hypothesis tested is Ho: "genotypes are drawn from the same distribution in all populations"

        test_diff(inputFile = "Data/Oly2016NFH+2017NFW_Merged.txt", outputFile = "Analyses/NF-Diff.txt", genic=FALSE, pairs=TRUE, dememorization = 10000, batches = 100, iterations = 1000, verbose = TRUE)

Resulting file: ["NF-Diff.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-Diff.txt)
Results indicate that genotypes are drawn from the same distribution, as P>>.01 for all loci. 

    Locus        Population pair        P-Value  S.E.     Switches
    -----------  ---------------------  -------  -------  --------
    Olur10       NFH-2016  & NFW-2017   0.32784  0.01552     27018
    Olur11       NFH-2016  & NFW-2017   0.54191  0.01155     36148
    Olur12       NFH-2016  & NFW-2017   0.67662  0.01140     38491
    Olur13       NFH-2016  & NFW-2017   0.08560  0.00745     32174
    Olur15       NFH-2016  & NFW-2017   0.06505  0.00713     32414
    Olur19       NFH-2016  & NFW-2017   0.15165  0.01146     32502
    
    
    P-value for each population pair across all loci
    (Fisher's method)
    -----------------------------------------------------
    Population pair               Chi2      df   P-Value
    --------------------          --------  ---  --------
    NFW-2017      & NFH-2016      18.39076  12   0.104331

### Calculate Fst for each population
Fst is a measure of genetic structure (developed by Sewall Wright, 1969, 1978), and is related to statistical analysis of variance  (ANOVA). FST is the proportion of the total genetic variance contained in a subpopulation (the S subscript) relative to the total genetic variance (the T subscript). Values can range from 0 to 1. High FST implies a considerable degree of differentiation among populations. 
        
        Fst(inputFile = "Data/Oly2016NFH+2017NFW_Merged.txt", outputFile = "Analyses/NF-Fst.txt", sizes=F, pairs=TRUE, dataType="Diploid", verbose = TRUE)

Resulting file: ["NF-Fst.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-Fst.txt)
Values are very close to zero, which indicates that there is little genetic differentiation among wild and hatchery populations.

    Indices for populations:
    ----     -------------
    1        NFW-2017
    2        NFH-2016
    ----------------------
    
    Estimates for each locus:
    ========================
      Locus: Olur10
    ---------------------------------
    pop      1       
    2     -0.0007 
    
      Locus: Olur11
    ---------------------------------
    pop      1       
    2     -0.0027 
    
      Locus: Olur12
    ---------------------------------
    pop      1       
    2     -0.0020 
    
      Locus: Olur13
    ---------------------------------
    pop      1       
    2      0.0030 
    
      Locus: Olur15
    ---------------------------------
    pop      1       
    2      0.0036 
    
      Locus: Olur19
    ---------------------------------
    pop      1       
    2      0.0027 
    
    Estimates for all loci (diploid):
    =========================
    pop      1       
    2      0.0008 

### Generate stats on allelic diversity. Interpretation TBD. 
       
       genedivFis(inputFile="Data/Oly2016NFH+2017NFW_Merged.txt", sizes=FALSE, outputFile = "Analyses/NF-DivFis.txt", dataType = "Diploid", verbose=interactive())

Resulting file: ["NF-DivFis.txt"](https://raw.githubusercontent.com/laurahspencer/O.lurida_genetics/master/Analyses/NF-DivFis.txt)
