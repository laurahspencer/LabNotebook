---
layout: post
title: Finding P. cod sex markers with GWAS (first attempt)
--- 

It would be great to identify genetic markers that predict Pacific cod sex (if possible). This would allow us to include sex as a covariate in genetic studies, look for sex biases among groups, and see if sex influences phenotypes in our experiments. We are considering doing some sequencing of tissue from sexed Pcod in this next round, since it could add tons of value/insight in the other aspects of our study. But, we already have lcWGS data from quite a few sexed fish from reference spawning popualtions from Sara Schaal / Ingrid's work. So, here I explore possible P. cod sex markers using that data. 

## Reference fish with known sex 

We have 103 females and 63 males total, here's how many are from three major marine regions/spawning aggregations 

| marine_region | sex    | n  |
|---------------|--------|----|
| Aleutians     | female | 54 |
| Aleutians     | male   | 18 |
| eGOA          | female | 23 |
| eGOA          | male   | 24 |
| wGOA          | female | 26 |
| wGOA          | male   | 21 |

## GWAS in ANGSD 

**I'm working in this directory on Sedna: /home/lspencer/pcod-sex/**  
I ran association tests (GWAS, genome wide association study) in ANGSD using genotype probabilities from all reference fish with known sex. 

### Step 1. Subset genotype likelihood file (beagle file) for only fish that are sexed
I luckily have code and files ready to go for this. I wrote it when I developed my WGSassign pipeline. It relies on a slightly edited beagle file, with sample IDs in the header (instead of "Ind1, Ind2" etc.), which I also have ready to go (called "rehead_beagle1.gz") from my previous join_beagle.sh code, which I used to merge reference and experimental fish for PCA and WGSassign. I did need to create a new text file, "id_file", that lists sample IDs for sexed fish in a single column (no header), which I did in R. 

Here is the slurm script: [subset-ref-beagle.sh](https://raw.githubusercontent.com/laurahspencer/pcod-sex/refs/heads/main/subset-ref-beagle.sh)

### Step 2. Perform imputation (i.e. fill in missing genotypes) 
Imputation fills in missing genotypes by inference, see [this write-up](https://baylab.github.io/MarineGenomicsSemester/week-10-genome-wide-association-study-gwas.html). I'm interested to see how my results differ when I use imputed genotype probabilities, and the original probabilities without imputation. To perform the imputation, I Downloaded beagle.jar from [http://faculty.washington.edu/browning/beagle/beagle.html](http://faculty.washington.edu/browning/beagle/beagle.html). NOTE: I downloaded and used an old version of Beagle ([vs3](http://faculty.washington.edu/browning/beagle/beagle.jar)); later versions of Beagle (e.g. 4.1, 5.4) do not perform imputation on genotype probability beagle files.  

_NOTE: imputation is performed in the same script as GWAS, see next step_ 

### Step 3. Perform GWAS 

Finally, I ran the GWAS slurm script. Inputs are:  
-  Beagle files (original or imputed), must have sample IDs without suffixes (i.e. three identical column names in a row, e.g. "ABLG1968  ABLG1968  ABLG1968", not "ABLG1968_AA     ABLG1968_AB     ABLG1968_BB"   
-  fish-sex.txt: file listing sex as a binary variable (1=female, 0=male) in a single column in the same sample order as they appear in the beagle file  
-  fish-region.txt: covariate file listing marine_region as numeric (1, 2, or 3) in the same order as they appear in the beagle file   
-  GCF_031168955.1_ASM3116895v1_genomic.fna.fai: our reference genome fasta index  

Here's the [pcod-sex-gwas.sh](https://raw.githubusercontent.com/laurahspencer/pcod-sex/refs/heads/main/pcod-sex-gwas.sh) slurm script. 

### Step 4: Examine GWAS results 
GWAS using both the imputed and original genotype probabilities found several sex-associated markers on Chromosome 11 - see manhattan plots below of log10-pvalues. GWAS using imputed genotype probabilities found similar results, but wtih a few more potentially sex-associated markers (not shown here). 

![image](https://github.com/user-attachments/assets/00041d5d-bf9c-4f3e-a5a0-7b9c05de17cc)
![image](https://github.com/user-attachments/assets/6beb0fe1-fd5f-4409-a814-5d006659acf3)

Atlantic cod also have sex-associated region on chromosome 11 (they refer to it as linkage group 11). From [Kirubakaran et al. 2019](https://doi.org/10.1038/s41598-018-36748-8) - "Here we characterize a male-specific region of 9 kb on linkage group 11 in Atlantic cod (Gadus morhua) harboring a single gene named zkY for zinc knuckle on the Y chromosome." They identified one specific gene of interest _zkY_ that is male-specific. Here is Figure 1  from that paper showing zkY and neighboring genes: 

![image](https://github.com/user-attachments/assets/f039ea00-e55c-46de-b0fe-d40cc5857ed1)

What's cool is that the region on the Pacific cod genome that contains the high-likelihood sex markers, which spans from ~13.8M-14.1M bases along Chrom. 11, contains similar genes compared to those on the sex-associated region in Atlantic cod. Here is a link to the region on Pacific cod Chrom. 11 that contains most of the high-likelihood sites: [https://www.ncbi.nlm.nih.gov/gdv/browser/genome/?cfg=NCID_1_7662302_130.14.22.10_9146_1731883376_3690893620](https://www.ncbi.nlm.nih.gov/gdv/browser/genome/?cfg=NCID_1_7662302_130.14.22.10_9146_1731883376_3690893620) (note, after 90 days of this post the link will be deleted, so you can search for region NC_082392.1:13835180-14120700 in the [G. macrocephalus assembly](https://www.ncbi.nlm.nih.gov/gdv/browser/genome/?id=GCF_031168955.1)). Below is a screenshot of a closer look at the site with the highest likelihood on the Pacific cod genome, and you'll see it's in the coding region of the gene "ptpn2a", with neighboring genes (cep76, cep192,seh1l) similar to the Atlantic cod region of interest (see figure above). I believe the "zkY " gene, if also present in Pacific cod, would be downstream (right) of the below region only in males (but I need to read that Kirubakaran paper more thoroughly and look for more recent lit. to make sure this is accurate). 

![image](https://github.com/user-attachments/assets/02c4d4cd-0cf0-4154-b506-f26a65be0e3b)

### Step 5: Use putative sex markers to look at sex-associated structure in reference fish 
I pulled genotype likeihoods for only these sex-associated loci and used PCAngsd to generate a covariance matrix, then performed PCA in R. Here's the slurm script, [**pca-wgsassign-sex.sh**](https://raw.githubusercontent.com/laurahspencer/pcod-sex/refs/heads/main/pca-wgsassign-sex.sh). 

Using R I performed PCA. Principal componenet 1 explains the majority of variation in these putative sex markers (~66%). **R script where I explore GWAS results and whatnot: [GWAS.Rmd](https://raw.githubusercontent.com/laurahspencer/pcod-sex/refs/heads/main/GWAS.Rmd)**. And hey, look at that! Reference samples segregate nicely by sex. These ~50 putative sex markers, most of which are on Chromosome 11, do a pretty good job separating fish into males and females along PC1. 

![image](https://github.com/user-attachments/assets/eed6fde3-799c-44bd-8ceb-ccb58a943510)
![image](https://github.com/user-attachments/assets/3635cf3b-8ba6-430e-a7d2-177f48b0f526)
![image](https://github.com/user-attachments/assets/53e0679d-c5ef-4975-852a-bce84611e235)

### EXPERIMENT: Can we identify sex in Big/Little fish from 2021 Kodiak collection? 
I won't be able to confidently answer this question, BUT I'm pretty excited about this result! I pulled genotype likelihoods (from beagles) for only putative sex markers from the Big/Little fish collected in 2021 off Kodiak and performed PCA. There are 47 of those putative sex markers in that dataset, which seemingly do a very good job segregating fish into two groups along PC1, which explains 57% of variation. 

![image](https://github.com/user-attachments/assets/198b1b96-bc23-4e09-b9ca-33178f8166ca)

### EXPERIMENT: Can we identify sex in experimental juveniles? 
The pattern is much less clear, probably because this data is low coverage (3x), but there is quite a big of variance explained by PC1. 

![image](https://github.com/user-attachments/assets/1334eede-9d81-4289-acf7-275e1ff79eb5)


### Are putative sex markers associated with specific genes or processes? 
Here are the putative sex markers, many of which are inside (n=38) or within 2kb upstream (n=3) of annotated genes (upstream = potentially involved in transcription regulation). 

| Chromosome  | Position | Major | Minor | LRT  | SE     | pvalue   | gene_id          | protein_names                                                                                                                                                                                                   |
|-------------|----------|-------|-------|------|--------|----------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NC_082392.1 | 13871084 | T     | G     | 110  | 2      | 7.83e-26 | ptpn2a           | Tyrosine-protein phosphatase non-receptor type 2 (EC 3.1.3.48) (T-cell protein-tyrosine phosphatase) (TCPTP)                                                                                                    |
| NC_082392.1 | 13864340 | A     | G     | 69.3 | 0.78   | 8.51e-17 | seh1l            | Nucleoporin SEH1 (GATOR2 complex protein SEH1) (Nup107-160 subcomplex subunit seh1)                                                                                                                             |
| NC_082392.1 | 13886325 | C     | T     | 59.1 | 0.664  | 1.47e-14 | cep76            | Centrosomal protein of 76 kDa (Cep76)                                                                                                                                                                           |
| NC_082392.1 | 13883774 | C     | A     | 57.4 | 0.658  | 3.5e-14  | cep76            | Centrosomal protein of 76 kDa (Cep76)                                                                                                                                                                           |
| NC_082392.1 | 13860311 | C     | T     | 47.3 | 0.866  | 6.19e-12 | cep192           | Centrosomal protein of 192 kDa (Cep192) (Cep192/SPD-2)                                                                                                                                                          |
| NC_082392.1 | 13863929 | T     | C     | 44.1 | 0.768  | 3.04e-11 | seh1l            | Nucleoporin SEH1 (GATOR2 complex protein SEH1) (Nup107-160 subcomplex subunit seh1)                                                                                                                             |
| NC_082392.1 | 14003292 | G     | A     | 43.3 | 0.423  | 4.76e-11 | LOC132468235     | Neurocalcin-delta A                                                                                                                                                                                             |
| NC_082392.1 | 14004520 | G     | C     | 38   | 0.438  | 6.99e-10 | LOC132468235     | Neurocalcin-delta A                                                                                                                                                                                             |
| NC_082392.1 | 13835195 | G     | T     | 34.5 | 0.528  | 4.21e-09 | cep192           | Centrosomal protein of 192 kDa (Cep192) (Cep192/SPD-2)                                                                                                                                                          |
| NC_082383.1 | 10569439 | G     | A     | 29.4 | 0.582  | 5.85e-08 | ppp1r1b          | Protein phosphatase 1 regulatory subunit 1B (DARPP-32) (Dopamine- and cAMP-regulated neuronal phosphoprotein)                                                                                                   |
| NC_082392.1 | 14104515 | G     | A     | 29   | 0.413  | 7.41e-08 | LOC132468224     | E3 ubiquitin-protein ligase RNF19A (EC 2.3.2.31) (Double ring-finger protein) (Dorfin) (RING finger protein 19A)                                                                                                |
| NC_082392.1 | 14016733 | G     | T     | 28.3 | 0.378  | 1.04e-07 | LOC132468235     | Neurocalcin-delta A                                                                                                                                                                                             |
| NC_082392.1 | 14025607 | A     | G     | 25.9 | 0.389  | 3.63e-07 | LOC132468235     | Neurocalcin-delta A                                                                                                                                                                                             |
| NC_082392.1 | 14025607 | A     | G     | 25.9 | 0.389  | 3.63e-07 | LOC132468241     |                                                                                                                                                                                                                 |
| NC_082397.1 | 24728203 | C     | T     | 22.5 | 228000 | 2.07e-06 | farsb            | Phenylalanine--tRNA ligase beta subunit (EC 6.1.1.20) (Phenylalanyl-tRNA synthetase beta subunit) (PheRS)                                                                                                       |
| NC_082392.1 | 14115398 | T     | C     | 21.5 | 0.373  | 3.53e-06 | LOC132467531     | Sperm-associated antigen 1 (HSD-3.8) (Infertility-related sperm protein Spag-1)                                                                                                                                 |
| NC_082394.1 | 14763732 | C     | T     | 20.7 | 0.654  | 5.42e-06 | syn2b            | Synapsin-2 (Synapsin II)                                                                                                                                                                                        |
| NC_082392.1 | 13846732 | A     | T     | 20.2 | 0.47   | 6.97e-06 | cep192           | Centrosomal protein of 192 kDa (Cep192) (Cep192/SPD-2)                                                                                                                                                          |
| NC_082383.1 | 5392090  | A     | G     | 19.9 | 0.499  | 7.96e-06 | elac2            | Zinc phosphodiesterase ELAC protein 2 (EC 3.1.26.11) (ElaC homolog protein 2) (Ribonuclease Z 2) (RNase Z 2) (tRNA 3 endonuclease 2) (tRNase Z 2)                                                               |
| NC_082395.1 | 6672772  | G     | A     | 19.2 | 0.815  | 1.15e-05 | urb2             | Unhealthy ribosome biogenesis protein 2 homolog                                                                                                                                                                 |
| NC_082384.1 | 8088796  | C     | T     | 19.1 | 0.332  | 1.27e-05 | LOC132453903     |                                                                                                                                                                                                                 |
| NC_082387.1 | 10957453 | A     | T     | 19   | 0.513  | 1.29e-05 | LOC132459091     | Netrin receptor UNC5B-b (Protein unc-5 homolog-b) (Unc-5-like protein B)                                                                                                                                        |
| NC_082401.1 | 14981947 | C     | T     | 18.6 | 0.335  | 1.6e-05  | myo10l3          | Unconventional myosin-X (Unconventional myosin-10)                                                                                                                                                              |
| NC_082388.1 | 1735871  | A     | C     | 18.4 | 208000 | 1.79e-05 | LOC132460630     | NACHT, LRR and PYD domains-containing protein 12 (Monarch-1) (PYRIN-containing APAF1-like protein 7) (Regulated by nitric oxide)                                                                                |
| NC_082388.1 | 1735871  | A     | C     | 18.4 | 208000 | 1.79e-05 | LOC132460633     | NLR family CARD domain-containing protein 3 (CARD15-like protein) (Caterpiller protein 16.2) (CLR16.2) (NACHT, LRR and CARD domains-containing protein 3) (Nucleotide-binding oligomerization domain protein 3) |
| NC_082383.1 | 23141235 | T     | C     | 18.4 | 0.431  | 1.82e-05 | smg1             | Serine/threonine-protein kinase SMG1 (SMG-1) (EC 2.7.11.1) (Lambda/iota protein kinase C-interacting protein) (Lambda-interacting protein) (Nonsense mediated mRNA decay-associated PI3K-related kinase SMG1)   |
| NC_082399.1 | 10612688 | G     | A     | 18   | 0.402  | 2.2e-05  | pvalb9           | Parvalbumin, thymic CPV3 (Parvalbumin-3)                                                                                                                                                                        |
| NC_082404.1 | 13542723 | G     | T     | 17.9 | 0.343  | 2.28e-05 | ppp1r16a         | Protein phosphatase 1 regulatory subunit 16A (Myosin phosphatase targeting subunit 3)                                                                                                                           |
| NC_082398.1 | 4576240  | A     | G     | 17.6 | 0.506  | 2.68e-05 | si:ch211-63p21.1 |                                                                                                                                                                                                                 |
| NC_082382.1 | 23290981 | A     | G     | 17.6 | 162000 | 2.71e-05 | LOC132462987     | NACHT, LRR and PYD domains-containing protein 12 (Monarch-1) (PYRIN-containing APAF1-like protein 7) (Regulated by nitric oxide)                                                                                |
| NC_082389.1 | 4330430  | C     | G     | 17.5 | 0.335  | 2.82e-05 | LOC132462677     | Voltage-dependent R-type calcium channel subunit alpha-1E (Brain calcium channel II) (BII) (Calcium channel, L type, alpha-1 polypeptide, isoform 6) (Voltage-gated calcium channel subunit alpha Cav2.3)       |
| NC_082404.1 | 13542018 | T     | C     | 17.5 | 0.4    | 2.86e-05 | ppp1r16a         | Protein phosphatase 1 regulatory subunit 16A (Myosin phosphatase targeting subunit 3)                                                                                                                           |
| NC_082397.1 | 14542883 | G     | C     | 17.5 | 47400  | 2.9e-05  | si:ch211-235m3.5 |                                                                                                                                                                                                                 |
| NC_082384.1 | 14096248 | C     | G     | 17.3 | 0.691  | 3.24e-05 | LOC132454224     | Synaptogyrin-1 (p29)                                                                                                                                                                                            |
| NC_082389.1 | 8393309  | G     | A     | 17.2 | 1.12   | 3.28e-05 | dcaf8            | DDB1- and CUL4-associated factor 8 (WD repeat-containing protein 42A)                                                                                                                                           |
| NC_082395.1 | 1168995  | C     | A     | 17   | 0.525  | 3.8e-05  | arl14ep          | ARL14 effector protein (ARF7 effector protein)                                                                                                                                                                  |
| NC_082401.1 | 20889576 | T     | C     | 16.9 | 0.313  | 4e-05    | pofut2           | GDP-fucose protein O-fucosyltransferase 2 (EC 2.4.1.221) (Peptide-O-fucosyltransferase 2) (O-FucT-2)                                                                                                            |
| NC_082395.1 | 20362496 | G     | T     | 16.7 | 1.04   | 4.3e-05  | LOC132471491     |                                                                                                                                                                                                                 |
| NC_082387.1 | 26174979 | T     | A     | 16.4 | 0.649  | 5.04e-05 | LOC132460192     | FH1/FH2 domain-containing protein 3 (Formactin-2) (Formin homolog overexpressed in spleen 2) (hFHOS2)                                                                                                           |
| NC_082390.1 | 16771212 | T     | A     | 16.4 | 1.05   | 5.2e-05  | LOC132464682     | Carbohydrate sulfotransferase 8 (EC 2.8.2.-) (GalNAc-4-O-sulfotransferase 1) (GalNAc-4-ST1) (GalNAc4ST-1) (N-acetylgalactosamine-4-O-sulfotransferase 1)                                                        |
| NC_082388.1 | 12602684 | G     | A     | 16.2 | 0.345  | 5.72e-05 | LOC132460874     | Trafficking protein particle complex subunit 14 (Microtubule-associated protein 11)                                                                                                                             |
| NC_082388.1 | 12602684 | G     | A     | 16.2 | 0.345  | 5.72e-05 | LOC132460870     |                                                                                                                                                                                                                 |
| NC_082388.1 | 12602684 | G     | A     | 16.2 | 0.345  | 5.72e-05 | LOC132460873     |                                                                                                                                                                                                                 |
| NC_082385.1 | 33364181 | C     | T     | 16   | 0.452  | 6.29e-05 | LOC132455445     |                                                                                                                                                                                                                 |
| NC_082385.1 | 33364181 | C     | T     | 16   | 0.452  | 6.29e-05 | LOC132455345     | NLR family CARD domain-containing protein 3 (CARD15-like protein) (Caterpiller protein 16.2) (CLR16.2) (NACHT, LRR and CARD domains-containing protein 3) (Nucleotide-binding oligomerization domain protein 3) |
| NC_082400.1 | 2788962  | T     | C     | 16   | 0.542  | 6.37e-05 | nup188           | Nucleoporin NUP188 (hNup188)                                                                                                                                                                                    |
| NC_082400.1 | 5998453  | T     | G     | 15.9 | 24900  | 6.53e-05 | LOC132447881     | Guanine nucleotide exchange factor VAV2 (VAV-2)                                                                                                                                                                 |
| NC_082392.1 | 16779539 | C     | T     | 15.9 | 0.583  | 6.75e-05 | adcy3a           | Adenylate cyclase type 3 (EC 4.6.1.1) (ATP pyrophosphate-lyase 3) (Adenylate cyclase type III) (AC-III) (Adenylate cyclase, olfactive type) (Adenylyl cyclase 3) (AC3)                                          |
| NC_082399.1 | 6838934  | T     | G     | 15.7 | 0.452  | 7.28e-05 | sncgb            | Alpha-synuclein                                                                                                                                                                                                 |
| NC_082391.1 | 938064   | T     | A     | 15.7 | 0.597  | 7.35e-05 | elp1             | Elongator complex protein 1 (ELP1) (IkappaB kinase complex-associated protein) (IKK complex-associated protein) (p150)                                                                                          |
| NC_082391.1 | 21166930 | C     | T     | 15.6 | 0.393  | 7.65e-05 | slc26a2          | Sulfate transporter (Solute carrier family 26 member 2)                                                                                                                                                         |
| NC_082384.1 | 8086290  | A     | C     | 15.6 | 0.321  | 7.74e-05 | LOC132453903     |                                                                                                                                                                                                                 |
| NC_082401.1 | 10135616 | C     | G     | 15.6 | 0.385  | 7.95e-05 | mycbp2           | E3 ubiquitin-protein ligase MYCBP2 (EC 2.3.2.33) (Myc-binding protein 2) (Pam/highwire/rpm-1 protein) (Protein Magellan) (Protein associated with Myc)                                                          |
| NC_082400.1 | 17784330 | C     | T     | 15.4 | 0.316  | 8.56e-05 | chchd3a          | MICOS complex subunit Mic19 (Coiled-coil-helix-coiled-coil-helix domain-containing protein 3)                                                                                                                   |
| NC_082386.1 | 15007185 | G     | A     | 15.3 | 0.336  | 9.39e-05 | fermt2           | Fermitin family homolog 2 (Kindlin-2)                                                                                                                                                                           |
| NC_082404.1 | 3984286  | T     | C     | 15.2 | 0.574  | 9.44e-05 | rbbp8            | DNA endonuclease RBBP8 (EC 3.1.-.-)                                                                                                                                                                             |
| NC_082403.1 | 1138637  | C     | T     | 15.2 | 0.526  | 9.82e-05 | LOC132451362     | NLR family CARD domain-containing protein 3 (CARD15-like protein) (Caterpiller protein 16.2) (CLR16.2) (NACHT, LRR and CARD domains-containing protein 3) (Nucleotide-binding oligomerization domain protein 3) |
| NC_082403.1 | 1138637  | C     | T     | 15.2 | 0.526  | 9.82e-05 | LOC132451514     |                                                                                                                                                                                                                 |
### Here are the top 5 sex-associated markers as per GWAS 
Here are the top 5 sex-associated sites. X-axes are whether fish are female or male. Y-axes represent whether the fish is homozygous for major allele (1.0), heterogyzous (0.5), or homozygous for minor allele (0). Points have been jittered slightly for readability. There are separate plots for each marine region. 

![image](https://github.com/user-attachments/assets/5d1fcfda-0b55-4daa-b88d-f7bb92ba50f3)
![image](https://github.com/user-attachments/assets/89054db2-5cc2-420f-842f-8f6e4944cda6)
![image](https://github.com/user-attachments/assets/feccb945-9514-4744-8286-f2fcb3d69351)
![image](https://github.com/user-attachments/assets/dd335b8d-033b-453b-a169-72bb1e426cff)
![image](https://github.com/user-attachments/assets/513e29e8-10eb-4e63-91d7-d527f648a50a)



### NEXT STEP: Perform more rigorous analysis with training/test stes and leave-one-out
Here I included ALL sexed fish. Next step should include separate training and test sets so that I can get error/accuracy rates and whatnot, and GWAS re-run with random sex assignments to make sure these loci are indeed associated with sex. 

