---
layout: post
title: Oly OA RNA isolation - sequencing
---

I'm done with my QuantSeq libraries!  After getting a few quotes for sequencing, we've decided have the UW Genome Sciences sequencing core do it (the [Sequencing Northwest Genomics Center](https://nwgc.gs.washington.edu/)). They also pooled my libraries for me for a small fee, but I needed to provide them with library concentrations (from QuBit). I also sent them my BioAnalyzer results (mean library length). 

On Friday, April 17 I prepped my samples and walked them over to Erica at the genome science center. Here's what needed to be done: 

### Quantified a few more libraries using Qubit. 

I had to quantify my juvenile O. lurida libraries, and a couple random libraries that I previously omitted. I used the [Qubit 1X dsDNA HS Assay](https://assets.thermofisher.com/TFS-Assets/LSG/manuals/MAN0017455_Qubit_1X_dsDNA_HS_Assay_Kit_UG.pdf). 

### Transerred libraries to new PCR plates

I submitted 8 uL of each library, which is ~half of my volumes (started with ~16 uL). I also wanted to make sure the sequencing facility would correctly pool my libraries in two batches -- 1) Ctenidia+Juvenile and 2) Larvae. This is because some of my index numbers were the same in Batch 1 as in Batch 2, so they needed to be run on separate lanes. To make this very clear, I prepared 2 PCR plates, one for each batch/lane. Also, because I re-prepped a few libraries, I needed to carefully identify which to transfer, and which to omit.  

![image](https://user-images.githubusercontent.com/17264765/80171146-74da1e00-859e-11ea-84a3-c5e81e2ce7da.png)

### Library details 

Below are details for the libraries I submitted, with information such as original RNA concentration used for library prep, # PCR cycles used to amplify libraries, library concentration (DNA, from Qubit), mean library length (for a few), index number. All this information is located in this [Sample-Submission-and-Info.xlsx](https://github.com/fish546-2018/laura-quantseq/blob/master/data/library-prep/Sample-Submission-and-Info.xlsx) spreadsheet. 

Here is the manifest that I sent to Erica: [Spencer,LH_Sample-Manifest-2020-04-17.xlsx](https://github.com/fish546-2018/laura-quantseq/blob/master/data/library-prep/Spencer%2CLH_Sample-Manifest-2020-04-17.xlsx) 

Here is the sample submission sheet: [Spencer,LH_Sample-Submission-Form-2020-04-17.pdf
](https://github.com/fish546-2018/laura-quantseq/blob/master/data/library-prep/Spencer%2CLH_Sample-Submission-Form-2020-04-17.pdf)

### A note on PhiX Spike-in: 
Erica asked whether I would like PhiX spike-in. Standard protocol for any Illumina platform is to do a 1% spike-in for quality control. The QuantSeq library prep manual does not specify whether it is recommended for my library type, although it does specify for QuantSeq libraries prepped using an add-on module which tack on UMIs to each library. I eailed my QuantSeq rep, and they replied quickly with the following: "For standard QuantSeq libraries without UMIs, 1% is fine. The reason libraries with UMIs need more is due to the spacer we use with the UMIs. It has a unique fingerprint that requires a more PhiX. That is the only reason. Even when labs combine non-UMI libraries with UMI libraries, they no longer need the higher percentage of PhiX. It is for UMI only libraries in a flow cell lane. It is rather specific." **So, in the end I had them use a 1% PhiX spike-in.**

### Adult O. lurida ctenidia samples, multiple populations and pCO2 exposures 

Plate/Batch/Lane   # | Well | Sample No. | [RNA] (ng/ul) | Cycles, round down (for most) | Tissue source | Tissue type | [DNA] (ng/uL) | Bioanalyzer mean bp | INDEX # | Index bp sequence | Population | Parental pCO2
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
1 | D06 | 291 | 158.0 | 15 | Adult | ctenidia | 3.54 | NT | 7035 | GTTACC | Dabob Bay | High
1 | C08 | 292 | 29.6 | 15 | Adult | ctenidia | 1.32 | NT | 7037 | TGGCGA | Dabob Bay | High
1 | A11 | 293 | 39.6 | 16 | Adult | ctenidia | 1.03 | NT | 7014 | AATCCG | Dabob Bay | High
1 | A07 | 294 | 110.0 | 16 | Adult | ctenidia | 1.95 | 245 | 7008 | TGTGCA | Dabob Bay | High
1 | D05 | 295 | 34.8 | 15 | Adult | ctenidia | 3.58 | NT | 7050 | TCGAGG | Dabob Bay | High
1 | D04 | 296 | 180.0 | 15 | Adult | ctenidia | 3.46 | NT | 7046 | CTCCAT | Dabob Bay | High
1 | C01 | 298 | 182.0 | 15 | Adult | ctenidia | 3.08 | NT | 7028 | GCTCGA | Dabob Bay | High
1 | B09 | 299 | 50.4 | 15 | Adult | ctenidia | 2.46 | NT | 7024 | CCGCAA | Dabob Bay | High
1 | B10 | 301 | 75.8 | 15 | Adult | ctenidia | 2.82 | NT | 7025 | TTTATG | Dabob Bay | Ambient
1 | B03 | 302 | 62.4 | 14 | Adult | ctenidia | 4.9 | NT | 7018 | GTCAGG | Dabob Bay | Ambient
1 | B05 | 303 | 95.2 | 14 | Adult | ctenidia | 2.4 | NT | 7020 | TATGTC | Dabob Bay | Ambient
1 | B12 | 304 | 200.0 | 15 | Adult | ctenidia | 2.8 | NT | 7027 | CAAGCA | Dabob Bay | Ambient
1 | A01 | 305 | 75.2 | 16 | Adult | ctenidia | 2.96 | 267 | 7002 | GATCAC | Dabob Bay | Ambient
1 | D08 | 306 | 136.0 | 15 | Adult | ctenidia | 4.14 | NT | 7051 | CACTAA | Dabob Bay | Ambient
1 | D09 | 307 | 89.4 | 15 | Adult | ctenidia | 3.32 | NT | 7053 | CGCCTG | Dabob Bay | Ambient
1 | A05 | 308 | 73.6 | 16 | Adult | ctenidia | 2.48 | 256 | 7006 | GTGTAG | Dabob Bay | Ambient
1 | B08 | 309 | 170.0 | 14 | Adult | ctenidia | 1.46 | NT | 7023 | CACACT | Dabob Bay | Ambient
1 | A02 | 311 | 158.0 | 16 | Adult | ctenidia | 3.76 | 262 | 7003 | ACCAGT | Oyster Bay C1 | High
1 | C04 | 312 | 90.6 | 15 | Adult | ctenidia | 1.58 | NT | 7032 | CGAAGG | Oyster Bay C1 | High
1 | C12 | 313 | 72.4 | 15 | Adult | ctenidia | 1.81 | NT | 7041 | CTCTCG | Oyster Bay C1 | High
1 | E01 | 314 | 42.2 | 17 | Adult | ctenidia | 4.32 | NT | 7049 | GTGCCA | Oyster Bay C1 | High
1 | C02 | 315 | 148.0 | 15 | Adult | ctenidia | 2.32 | NT | 7029 | GCGAAT | Oyster Bay C1 | High
1 | A09 | 316 | 146.0 | 16 | Adult | ctenidia | 3.74 | 252 | 7012 | ATGAAC | Oyster Bay C1 | High
1 | C09 | 317 | 158.0 | 15 | Adult | ctenidia | 2.14 | NT | 7038 | ACCGTG | Oyster Bay C1 | High
1 | A06 | 318 | 174.0 | 16 | Adult | ctenidia | 4.08 | 262 | 7007 | CTAGTC | Oyster Bay C1 | High
1 | E04 | 319 | 77.6 | 17 | Adult | ctenidia | 3.02 | NT | 7011 | TTAACT | Oyster Bay C1 | High
1 | C05 | 321 | 148.0 | 15 | Adult | ctenidia | 3.64 | NT | 7033 | AGATAG | Oyster Bay C1 | Ambient
1 | A08 | 322 | 44.6 | 16 | Adult | ctenidia | 2.7 | 269 | 7010 | CGGTTA | Oyster Bay C1 | Ambient
1 | D03 | 323 | 102.0 | 15 | Adult | ctenidia | 3.7 | NT | 7044 | ACAGAT | Oyster Bay C1 | Ambient
1 | D11 | 324 | 172.0 | 16 | Adult | ctenidia | 2.84 | NT | 7009 | TCAGGA | Oyster Bay C1 | Ambient
1 | B01 | 325 | 180.0 | 14 | Adult | ctenidia | 2.92 | NT | 7016 | TACCTT | Oyster Bay C1 | Ambient
1 | D02 | 326 | 130.0 | 15 | Adult | ctenidia | 4.04 | NT | 7043 | AAGACA | Oyster Bay C1 | Ambient
1 | D01 | 327 | 85.2 | 15 | Adult | ctenidia | 1.97 | NT | 7042 | TGACAC | Oyster Bay C1 | Ambient
1 | A12 | 328 | 156.0 | 14 | Adult | ctenidia | 2.12 | NT | 7015 | GGCTGC | Oyster Bay C1 | Ambient
1 | D10 | 329 | 162.0 | 16 | Adult | ctenidia | 5.1 | NT | 7045 | TAGGCT | Oyster Bay C1 | Ambient
1 | E05 | 331 | 42.2 | 19 | Adult | ctenidia | 9.6 | NT | 7001 | CAGCGT | Fidalgo Bay | High
1 | D12 | 332 | 65.8 | 16 | Adult | ctenidia | 2.14 | NT | 7047 | GCATGG | Fidalgo Bay | High
1 | C06 | 333 | 78.6 | 15 | Adult | ctenidia | 2.98 | NT | 7034 | TTGGTA | Fidalgo Bay | High
1 | E02 | 334 | 64.8 | 17 | Adult | ctenidia | 3.92 | NT | 7048 | AATAGC | Fidalgo Bay | High
1 | C07 | 335 | 180.0 | 15 | Adult | ctenidia | 2.58 | NT | 7036 | CGCAAC | Fidalgo Bay | High
1 | E03 | 336 | 94.8 | 17 | Adult | ctenidia | 4.6 | NT | 7052 | GGTATA | Fidalgo Bay | High
1 | C11 | 337 | 194.0 | 15 | Adult | ctenidia | 4.02 | NT | 7040 | GATTGT | Fidalgo Bay | High
1 | A04 | 338 | 81.6 | 16 | Adult | ctenidia | 2.74 | 269 | 7005 | ACATTA | Fidalgo Bay | High
1 | A10 | 339 | 77.2 | 16 | Adult | ctenidia | 1.91 | NT | 7013 | CCTAAG | Fidalgo Bay | High
1 | B07 | 341 | 89.6 | 14 | Adult | ctenidia | 1.31 | NT | 7022 | GGAGGT | Fidalgo Bay | Ambient
1 | B11 | 342 | 162.0 | 15 | Adult | ctenidia | 1.78 | NT | 7026 | AACGCC | Fidalgo Bay | Ambient
1 | B02 | 343 | 114.0 | 14 | Adult | ctenidia | 2.58 | NT | 7017 | TCTTAA | Fidalgo Bay | Ambient
1 | C03 | 344 | 25.0 | 15 | Adult | ctenidia | 1.58 | NT | 7030 | TGGATT | Fidalgo Bay | Ambient
1 | B04 | 345 | 190.0 | 14 | Adult | ctenidia | 3.5 | NT | 7019 | ATACTG | Fidalgo Bay | Ambient
1 | B06 | 346 | 43.6 | 14 | Adult | ctenidia | 1.36 | NT | 7021 | GAGTCC | Fidalgo Bay | Ambient
1 | D07 | 347 | 69.0 | 15 | Adult | ctenidia | 4.36 | NT | 7031 | ACCTAC | Fidalgo Bay | Ambient
1 | A03 | 348 | 54.4 | 16 | Adult | ctenidia | 3.06 | 265 | 7004 | TGCACG | Fidalgo Bay | Ambient
1 | C10 | 349 | 82.0 | 15 | Adult | ctenidia | 1.9 | NT | 7039 | CAACAG | Fidalgo Bay | Ambient

### Juvenile O. lurida whole-body samples from those deployed in Port Gamble, with two populations and two parental pCO2 exposures 

Plate/Batch/Lane   # | Well | Sample No. | [RNA] (ng/ul) | Cycles, round down (for most) | Tissue source | Tissue type | [DNA] (ng/uL) | Bioanalyzer mean bp | INDEX # | Index bp sequence | Population | Parental pCO2
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
1 | G02 | 137 | 99.2 | 15 | Juvenile | whole body, individual | 5.2 | NT | 7090 | CCTGCT | Dabob Bay | Ambient
1 | F01 | 139 | 136.0 | 14 | Juvenile | whole body, individual | 13.3 | 294 | 7081 | GCAGCC | Dabob Bay | Ambient
1 | G03 | 140 | 174.0 | 15 | Juvenile | whole body, individual | 7.62 | 314 | 7091 | GCGCTG | Dabob Bay | Ambient
1 | F08 | 141 | 190.0 | 15 | Juvenile | whole body, individual | 23.6 | NT | 7088 | AGACCA | Dabob Bay | Ambient
1 | G05 | 156 | 110.0 | 16 | Juvenile | whole body, individual | 4.32 | NT | 7093 | TTCGAG | Fidalgo Bay | Ambient
1 | G07 | 159 | 184.0 | 16 | Juvenile | whole body, individual | 23 | 283 | 7095 | AGGCAT | Fidalgo Bay | Ambient
1 | F04 | 161 | 168.0 | 14 | Juvenile | whole body, individual | 6.22 | 290 | 7084 | AAGTGG | Fidalgo Bay | Ambient
1 | G01 | 162 | 164 | 15 | Juvenile | whole body, individual | 6.74 | 289 | 7080 | AACAAG | Fidalgo Bay | Ambient
1 | G06 | 168 | 89.0 | 16 | Juvenile | whole body, individual | 15.9 | 305 | 7094 | AGAATC | Dabob Bay | High
1 | G04 | 169 | 95.6 | 16 | Juvenile | whole body, individual | 3.68 | 310 | 7092 | GAACCT | Dabob Bay | High
1 | F05 | 171 | 146.0 | 15 | Juvenile | whole body, individual | 3.22 | 305 | 7085 | CTCATA | Dabob Bay | High
1 | F06 | 172 | 116.0 | 15 | Juvenile | whole body, individual | 10.6 | NT | 7086 | CCGACC | Dabob Bay | High
1 | G08 | 181 | 138.0 | 16 | Juvenile | whole body, individual | 2.84 | 280 | 7096 | ACACGC | Fidalgo Bay | High
1 | F03 | 183 | 196.0 | 14 | Juvenile | whole body, individual | 5.88 | NT | 7083 | TGCTAT | Fidalgo Bay | High
1 | F02 | 184 | 96.4 | 14 | Juvenile | whole body, individual | 6.1 | NT | 7082 | ACTCTT | Fidalgo Bay | High
1 | F07 | 185 | 102.0 | 15 | Juvenile | whole body, individual | 15 | 296 | 7087 | GGCCAA | Fidalgo Bay | High

### Larval O. lurida, multiple cohorts, parental pCO2 and temperature exposures. Larvae are pooled by pulse/family.  

Plate/Batch/Lane   # | Well | Sample No. | [RNA] (ng/ul) | Vol RNA used | Cycles, round down (for most) | Tissue source | Tissue type | [DNA] (ng/uL) | Bioanalyzer mean bp | INDEX # | Index bp sequence | Population | Parental Temperature | Parental pCO2 | Larval sample #
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
2 | F08 | 34 | 104.0 | 3.37 | 19 | Larval | whole body, pooled | 3.8 | NT | 7073 | GACATC | Oyster Bay C1 | 6 | Ambient | 77-A
2 | F12 | 35 | 94.4 | 3.71 | 21 | Larval | whole body, pooled | 0.522 | NT | 7075 | CGTCGC | Oyster Bay C1 | 6 | Ambient | 10-A
2 | F10 | 37 | 74.6 | 4.69 | 21 | Larval | whole body, pooled | 5.46 | NT | 7076 | ATGGCG | Oyster Bay C1 | 6 | Ambient | 69-A
2 | E04 | 39 | 89.8 | 3.90 | 15 | Larval | whole body, pooled | 2.04 | NT | 7056 | ATATCC | Oyster Bay C1 | 6 | Ambient | 48-A
2 | F02 | 41 | 108.0 | 3.24 | 19 | Larval | whole body, pooled | 6.82 | NT | 7068 | CCAATT | Oyster Bay C1 | 10 | High | 06-A
2 | D10 | 43 | 156.0 | 2.24 | 15 | Larval | whole body, pooled | 2.48 | 261 | 7050 | TCGAGG | Oyster Bay C1 | 10 | High | 08-A
2 | F11 | 44 | 41.4 | 5.00 | 21 | Larval | whole body, pooled | 0.18 | NT | 7078 | GCCACA | Oyster Bay C1 | 10 | High | 32-A
2 | F03 | 45 | 25.1 | 5 | 19 | Larval | whole body, pooled | 4.8 | NT | 7077 | ATTGGT | Oyster Bay C1 | 10 | High | 79-A
2 | F06 | 46 | 98.6 | 3.55 | 19 | Larval | whole body, pooled | 4.52 | NT | 7074 | CGATCT | Oyster Bay C1 | 10 | High | 24-A
2 | E10 | 47 | 128.0 | 2.73 | 16 | Larval | whole body, pooled | 2.58 | NT | 7070 | AACCGA | Oyster Bay C1 | 10 | High | 26-A
2 | A10 | 401 | 93.4 | 3.75 | 16 | Larval | whole body, pooled | 1.24 | 325 | 7012 | ATGAAC | Dabob Bay | 10 | Ambient | 14-A
2 | A05 | 402 | 114.0 | 3.07 | 16 | Larval | whole body, pooled | 2.76 | 346 | 7006 | GTGTAG | Dabob Bay | 10 | Ambient | 31-A
2 | C06 | 403 | 136.0 | 2.57 | 14 | Larval | whole body, pooled | 4.16 | NT | 7032 | CGAAGG | Dabob Bay | 10 | Ambient | 75-A
2 | D06 | 404 | 112.0 | 3.13 | 15 | Larval | whole body, pooled | 2.92 | NT | 7046 | CTCCAT | Dabob Bay | 10 | Ambient | 80-A
2 | A09 | 411 | 72.6 | 4.82 | 16 | Larval | whole body, pooled | 3.08 | 288 | 7011 | TTAACT | Dabob Bay | 10 | High | 23-A
2 | G02 | 412 | 31.2 | 5.00 | 23 | Larval | whole body, pooled | 2.52 | NT | 7069 | AGTTGA | Dabob Bay | 10 | High | 27-A
2 | D02 | 413 | 130.0 | 2.69 | 15 | Larval | whole body, pooled | 1.97 | NT | 7041 | CTCTCG | Dabob Bay | 10 | High | 58-A
2 | E01 | 414 | 168.0 | 2.08 | 15 | Larval | whole body, pooled | 0.858 | NT | 7053 | CGCCTG | Dabob Bay | 10 | High | 60-A
2 | B10 | 421 | 57.6 | 5.00 | 14 | Larval | whole body, pooled | 1.63 | NT | 7024 | CCGCAA | Dabob Bay | 6 | Ambient | 59-A
2 | G03 | 432 | 74.0 | 4.73 | 23 | Larval | whole body, pooled | 6.28 | NT | 7065 | AAGCTC | Dabob Bay | 6 | High | 72-A
2 | E07 | 434 | 162.0 | 2.16 | 15 | Larval | whole body, pooled | 5.58 | NT | 7059 | GGTGAG | Dabob Bay | 6 | High | 74-A
2 | G01 | 441 | 16.2 | 5.00 | 23 | Larval | whole body, pooled | 0.866 | NT | 7061 | GAAGTG | Fidalgo Bay | 10 | Ambient | 20-A
2 | C12 | 443 | 60.2 | 5.00 | 15 | Larval | whole body, pooled | 1.36 | NT | 7039 | CAACAG | Fidalgo Bay | 10 | Ambient | 53-A
2 | C10 | 444 | 70.6 | 4.96 | 14 | Larval | whole body, pooled | 4.08 | NT | 7037 | TGGCGA | Fidalgo Bay | 10 | Ambient | 63-A
2 | D09 | 445 | 160.0 | 2.19 | 15 | Larval | whole body, pooled | 4 | NT | 7049 | GTGCCA | Fidalgo Bay | 10 | Ambient | 65-A
2 | C04 | 451 | 68.4 | 5.12 | 14 | Larval | whole body, pooled | 2.24 | NT | 7030 | TGGATT | Fidalgo Bay | 10 | High | 16-A
2 | A12 | 453 | 196.0 | 1.79 | 14 | Larval | whole body, pooled | 0.968 | NT | 7014 | AATCCG | Fidalgo Bay | 10 | High | 36-A
2 | B08 | 473 | 124.0 | 2.82 | 14 | Larval | whole body, pooled | 2.34 | NT | 7022 | GGAGGT | Fidalgo Bay | 6 | High | 46-A
2 | B02 | 474 | 77.2 | 4.53 | 14 | Larval | whole body, pooled | 3.02 | NT | 7016 | TACCTT | Fidalgo Bay | 6 | High | 47-A
2 | B04 | 475 | 27.4 | 5.00 | 14 | Larval | whole body, pooled | 2.44 | NT | 7018 | GTCAGG | Fidalgo Bay | 6 | High | 50-A
2 | B05 | 476 | 39.2 | 5.00 | 14 | Larval | whole body, pooled | 4.24 | NT | 7019 | ATACTG | Fidalgo Bay | 6 | High | 54-A
2 | D01 | 477 | 164.0 | 2.13 | 15 | Larval | whole body, pooled | 2.28 | NT | 7040 | GATTGT | Fidalgo Bay | 6 | High | 76-A
2 | E09 | 481 | 89.2 | 3.92 | 15 | Larval | whole body, pooled | 2.18 | NT | 7036 | CGCAAC | Oyster Bay C1 | 10 | Ambient | 02-A
2 | C01 | 482 | 22.2 | 5.00 | 14 | Larval | whole body, pooled | 1.42 | NT | 7027 | CAAGCA | Oyster Bay C1 | 10 | Ambient | 04-A
2 | A07 | 483 | 99.0 | 3.54 | 16 | Larval | whole body, pooled | 1.22 | 219 | 7009 | TCAGGA | Oyster Bay C1 | 10 | Ambient | 03-A
2 | D07 | 484 | 58.4 | 5.00 | 15 | Larval | whole body, pooled | 1.67 | NT | 7047 | GCATGG | Oyster Bay C1 | 10 | Ambient | 09-A
2 | B09 | 485 | 19.1 | 5.00 | 14 | Larval | whole body, pooled | 2.9 | NT | 7023 | CACACT | Oyster Bay C1 | 10 | Ambient | 34-A
2 | A06 | 487 | 118.0 | 2.97 | 16 | Larval | whole body, pooled | 1.27 | 290 | 7007 | CTAGTC | Oyster Bay C1 | 10 | Ambient | 40-A
2 | C02 | 488 | 60.0 | 5.00 | 14 | Larval | whole body, pooled | 3.06 | NT | 7028 | GCTCGA | Oyster Bay C1 | 10 | Ambient | 44-A
2 | C11 | 489 | 68.0 | 5.00 | 15 | Larval | whole body, pooled | 3.84 | 272 | 7038 | ACCGTG | Oyster Bay C1 | 10 | Ambient | 49-A
2 | B07 | 490 | 186.0 | 1.88 | 14 | Larval | whole body, pooled | 2.32 | NT | 7021 | GAGTCC | Oyster Bay C1 | 10 | Ambient | 64-A
2 | E02 | 491 | 58.4 | 5.00 | 15 | Larval | whole body, pooled | 6.64 | NT | 7054 | AATGAA | Oyster Bay C1 | 10 | Ambient | 66-A
2 | D04 | 492 | 82.6 | 4.24 | 15 | Larval | whole body, pooled | 2.02 | NT | 7043 | AAGACA | Oyster Bay C1 | 10 | Ambient | 81-A
2 | D11 | 506 | 29.2 | 5.00 | 15 | Larval | whole body, pooled | 1.63 | NT | 7051 | CACTAA | Oyster Bay C1 | 10 | High | 62-A
2 | E08 | 513 | 142.0 | 2.46 | 15 | Larval | whole body, pooled | 5.92 | NT | 7060 | TTCCGC | Oyster Bay C1 | 6 | Ambient | 45-A
2 | F05 | 521 | 66.6 | 5.00 | 19 | Larval | whole body, pooled | 8.66 | NT | 7071 | CAGATG | Oyster Bay C1 | 6 | High | 01-A
2 | A01 | 522 | 32.2 | 5.00 | 16 | Larval | whole body, pooled | 2.42 | NT | 7002 | GATCAC | Oyster Bay C1 | 6 | High | 07-A
2 | A04 | 523 | 71.4 | 4.90 | 16 | Larval | whole body, pooled | 2.16 | 276 | 7005 | ACATTA | Oyster Bay C1 | 6 | High | 25-A
2 | A11 | 524 | 63.0 | 5.00 | 14 | Larval | whole body, pooled | 3.24 | NT | 7013 | CCTAAG | Oyster Bay C1 | 6 | High | 28-A
2 | B06 | 525 | 140.0 | 2.50 | 14 | Larval | whole body, pooled | 2.44 | NT | 7020 | TATGTC | Oyster Bay C1 | 6 | High | 30-A
2 | B03 | 526 | 138.0 | 2.54 | 14 | Larval | whole body, pooled | 2.2 | NT | 7017 | TCTTAA | Oyster Bay C1 | 6 | High | 33-A
2 | D03 | 527 | 124.0 | 2.82 | 15 | Larval | whole body, pooled | 1.42 | 243 | 7042 | TGACAC | Oyster Bay C1 | 6 | High | 61-A
2 | C03 | 528 | 87.6 | 4.00 | 14 | Larval | whole body, pooled | 4.48 | NT | 7029 | GCGAAT | Oyster Bay C1 | 6 | High | 68-A
2 | E05 | 529 | 56.8 | 5.00 | 15 | Larval | whole body, pooled | 2.86 | NT | 7057 | AGTACT | Oyster Bay C1 | 6 | High | 70-A
2 | D08 | 531 | 95.4 | 3.67 | 15 | Larval | whole body, pooled | 2.1 | NT | 7048 | AATAGC | Oyster Bay C2 | 10 | Ambient | 17-A
2 | C09 | 532 | 93.6 | 3.74 | 14 | Larval | whole body, pooled | 2.12 | NT | 7035 | GTTACC | Oyster Bay C2 | 10 | Ambient | 42-A
2 | F01 | 533 | 6.5 | 5.00 | 17 | Larval | whole body, pooled | 5.36 | NT | 7063 | ACGTCT | Oyster Bay C2 | 10 | Ambient | 56-A
2 | D05 | 541 | 44.4 | 5.00 | 15 | Larval | whole body, pooled | 1.4 | NT | 7044 | ACAGAT | Oyster Bay C2 | 10 | High | 12-A
2 | A03 | 542 | 32.8 | 5.00 | 16 | Larval | whole body, pooled | 2.22 | 270 | 7004 | TGCACG | Oyster Bay C2 | 10 | High | 13-A
2 | C05 | 543 | 162.0 | 2.16 | 14 | Larval | whole body, pooled | 5.24 | NT | 7031 | ACCTAC | Oyster Bay C2 | 10 | High | 43-A
2 | B12 | 551 | 96.4 | 3.63 | 14 | Larval | whole body, pooled | 1.69 | NT | 7026 | AACGCC | Oyster Bay C2 | 6 | Ambient | 35-A
2 | B11 | 553 | 186.0 | 1.88 | 14 | Larval | whole body, pooled | 3.28 | NT | 7025 | TTTATG | Oyster Bay C2 | 6 | Ambient | 55-A
2 | B01 | 554 | 188.0 | 1.86 | 14 | Larval | whole body, pooled | 2.38 | NT | 7015 | GGCTGC | Oyster Bay C2 | 6 | Ambient | 78-A
2 | F09 | 561 | 28.0 | 5.00 | 21 | Larval | whole body, pooled | 4.1 | NT | 7066 | GACGAT | Oyster Bay C2 | 6 | High | 05-A
2 | G05 | 562 | 126 | 2.777778 | 23 | Larval | whole body, pooled | 9.88 | NT | 7064 | CAGGAC | Oyster Bay C2 | 6 | High | 11-A
2 | E11 | 563 | 47.0 | 5.00 | 16 | Larval | whole body, pooled | 4.66 | NT | 7045 | TAGGCT | Oyster Bay C2 | 6 | High | 15-A
2 | C08 | 564 | 152.0 | 2.30 | 14 | Larval | whole body, pooled | 2.24 | NT | 7034 | TTGGTA | Oyster Bay C2 | 6 | High | 52-A
2 | F07 | 565 | 31.2 | 5.00 | 19 | Larval | whole body, pooled | 3.2 | NT | 7067 | TCGTTC | Oyster Bay C2 | 6 | High | 57-A
2 | G04 | 571 | LOW | 5.00 | 23 | Larval | whole body, pooled | 0.232 | 287 | 7072 | GTAGAA | Negative control | NA | NA | NA
2 | A08 | 431b | 83.0 | 4.22 | 16 | Larval | whole body, pooled | 3.36 | 285 | 7010 | CGGTTA | Dabob Bay | 6 | High | 51-A
2 | E12 | 442b | 69.8 | 5.00 | 17 | Larval | whole body, pooled | 3.2 | NT | 7062 | CAATGC | Fidalgo Bay | 10 | Ambient | 38-A
2 | A02 | 452b | 97.2 | 3.60 | 16 | Larval | whole body, pooled | 2.68 | 294 | 7003 | ACCAGT | Fidalgo Bay | 10 | High | 18-A
2 | C07 | 461b | 84.0 | 4.17 | 14 | Larval | whole body, pooled | 2.02 | NT | 7033 | AGATAG | Fidalgo Bay | 6 | Ambient | 22-A
2 | F04 | 462b | 106 | 3.301887 | 19 | Larval | whole body, pooled | 8.46 | NT | 7001 | CAGCGT | Fidalgo Bay | 6 | Ambient | 29-A
2 | E03 | 471b | 108.0 | 3.24 | 15 | Larval | whole body, pooled | 5.2 | NT | 7055 | ACAACG | Fidalgo Bay | 6 | High | 19-A
2 | D12 | 472b | 97.0 | 3.61 | 15 | Larval | whole body, pooled | 2.42 | NT | 7052 | GGTATA | Fidalgo Bay | 6 | High | 21-A
2 | E06 | 552b | 74.6 | 4.69 | 15 | Larval | whole body, pooled | 1.84 | NT | 7058 | ATAAGA | Oyster Bay C2 | 6 | Ambient | 41-A
