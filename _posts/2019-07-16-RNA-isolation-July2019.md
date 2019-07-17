---
layout: post
title: Oly OA RNA isolation - Ctenidia 
---

### July 12, 2019 

Homogenized the rest of the Oly ctenidia tissues, as described in the [previous post](https://laurahspencer.github.io/LabNotebook/Ctendia-homogenization/). For most samples I acheived at minimum 30 mg tissue. The scale was not very sensitive, so I'm estimating an error of +/- 20mg. 

Tested the RNAzol isolation process on 6 samples, one from each cohort x treatment.  Followed the [RNAzol® RT RNA Isolation Reagent](https://www.genecopoeia.com/wp-content/uploads/2013/06/RNAzol_RT_RNA_Isolation_Reagent_User_Manual.pdf) protocol for Total RNA isolation, using half of my homogenate, so 0500 uL. 

RNAzol Steps: 
  - Labeled 1.7 mL tubes with sample numbers, 2 tubes per sample  
  - Added 200 uL DEPC-treated water to one set of tubes  
  - Transferred 500 uL of homogenate to tubes with water, returned rest of homogenate (500 uL) to -20 freezer.  
  - Vortexed homogenate + water vigorously 15 seconds 
  - Held mixture for 15 minutes at room temperature  
  - Centrifuged at 12,000 g (aka rcf) for 15 minutes. _NOTE: for this first batch of 6 samples, I accidentally centrifuged at 1,200 for 15 minutes, and didn't catch the error until later._   
  - Transfered supernatant to new tubes (used 2nd set of labeled tubes). DNA/proteins/polysaccharides remain in the bottom of the tubes. I added the label "DNA + Pr." to these tubes, and saved in the -20 freezer.  
  - Added 0.75 mL isopropanol to precipitate RNA  
  - Vortexed vigorously for 15 seconds.  Held for 10 minutes at room temperature  
  - Centrifuged at 12,000 rcf for 10 minutes 
  - Discarded supernatant. A white RNA pellet was visible in samples (which is good) 
  - Added 500 uL 75% ethanol, which I prepared using the 200 proof ethanol and DEPC-treated water. 
  - Mixed by hand, made sure the pellete was in the ethanol, then centrifuged at 8,000 rcf for 3 minutes. 
  - Discarded ethanol supernatant, repeated previous step, then discarded ethanol again. 
  - Added 50 uL DEPC-treated water, vortexted by hand for 10 seconds. 
  - Stored RNA dissolved in water in the -20 freezer for now.  
  
### July 15, 2019 

Another batch of RNA isolation today.  I ran 24 samples at once, which I won't do again, as it takes too long per step.  Otherwise, things went well and there were no changes to the above protocol.  One note: sample 295 had no visible RNA pellet, so I don't expect I'll find any RNA when I quantify. This was probably due to the small size of the initial tissue piece, and loss during mortar/pestle homogenization.  

### July 16, 2019 

Finished isolating ctenidia RNA today.  Ran 2 batches of 12 samples, which allowed me to finish more quickly, and ultimately didn't take much longer tha doing 24 in one batch.  Good to know for the future.  Again, no changes to protocol, but some notes: 
  - The last batch accidentally sat for 25 minutes during the initial homogenate + water step (should have been 15 minutes) - timer malfunction :/ 
  - Dissolved RNA pellet in 150 uL DEPC treated water, not 50 uL.  See notes below. 

#### RNA Quantification, round 1 

Quantifed 18 samples, 3 from each treatment x cohort. Used 5 uL (of 50 uL), and the [Qubit RNA assay kit (high sensitivity)](https://assets.thermofisher.com/TFS-Assets/LSG/manuals/Qubit_RNA_HS_Assay_UG.pdf). All except 1 sample (#302) read "TOO HIGH".  I chatted with Sam, and based on the size of my RNA pellets he suggested dissolving RNA in 150 uL water, not 50 uL, and using 1 uL with quantifying. 

Based on the above, I added 100 uL of DEPC-treated water to all samples that I had previously processed. NOTE: this means that the 18 samples that I already quantified now have a total volume of ~145 uL, while the rest will have 150 uL, with a few exceptions:   
  - I did not add more water to sample 302, as it quantified in the first round.   
  - Samples 295, X and Y: only added 50 uL water, as no pellets were visible.  

#### RNA Quantification, round 2 
Quantified all ctenidia samples (except for #302) using Qubit HS RNA assay.  Prepared the 53 samples, plus the two standards. Thawed samples, then transferred 1 uL into Qubit working solution (prepared using 60 uL reagent + 11,940 uL buffer).  Good news is that I have enough RNA in all samples - see [table in repo](https://github.com/laurahspencer/O.lurida_Stress/blob/master/Data/RNA-DNA-Isolation/2019-July_RNA-isolation-ctenidia-larvae.xlsx), also pasted below - need to dilute a few samples and re-quantify before proceeding to the QuantSeq library prep.  


Cohort | pCO2 | HOMOGENATE TUBE   # | TUBE COLOR | TISSUE TYPE | TISSUE SAMPLE # | VOL RNAzol | MASS TISSUE   (mg) | DATE   HOMOGENIZED | Homogenization   Batch | DATE RNA   ISOLATED | RNA Isolation   Batch | Total volume,   uL (RNA + H2O) | [RNA] ng/uL | Amount of RNA (ng) | Volume needed for 500 ng RNA | Notes
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
Dabob Bay | High | 291 | PURPLE | CTENIDIA | HL10-10 | 1 mL | 80-100 | 7/10/19 | 1 | 7/12/19 | 1 | 144 | 200 | 28,800 | 2.50 |  
Dabob Bay | High | 292 | PURPLE | CTENIDIA | HL10-11 | 1 mL | 10-30 | 7/10/19 | 1 | 7/15/19 | 2 | 144 | 47.8 | 6,883 | 10.46 |  
Dabob Bay | High | 293 | PURPLE | CTENIDIA | HL10-12 | 1 mL | 10-30 | 7/10/19 | 1 | 7/15/19 | 2 | 144 | 47.2 | 6,797 | 10.59 |  
Dabob Bay | High | 294 | PURPLE | CTENIDIA | HL6-10 | 1 mL | 80-100 | 7/10/19 | 4 | 7/15/19 | 2 | 149 | 108 | 16,092 | 4.63 |  
Dabob Bay | High | 295 | PURPLE | CTENIDIA | HL6-11 | 1 mL | 10-30 | 7/10/19 | 4 | 7/15/19 | 2 | 49 | 49.6 | 2,430 | 10.08 |  
Dabob Bay | High | 296 | PURPLE | CTENIDIA | HL6-12 | 1 mL | 60-80 | 7/12/19 | 5 | 7/16/19 | 3 | 149 | 198 | 29,502 | 2.53 |  
Dabob Bay | High | 297 | PURPLE | CTENIDIA | HL6-13 | 1 mL | < 10 | 7/12/19 | 5 | 7/16/19 | 3 | 149 | 4.2 | 626 | 119.05 |  
Dabob Bay | High | 298 | PURPLE | CTENIDIA | HL6-14 | 1 mL | 80-100 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 76.4 | 11,384 | 6.54 | Black substance in   homogenate - maybe from tube lid?
Dabob Bay | High | 299 | PURPLE | CTENIDIA | HL6-15 | 1 mL | 10-30 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 56.8 | 8,463 | 8.80 |  
  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |  
Dabob Bay | Ambient | 301 | RED | CTENIDIA | HL10-19 | 1 mL | 10-30 | 7/10/19 | 1 | 7/12/19 | 1 | 144 | 82.6 | 11,894 | 6.05 |  
Dabob Bay | Ambient | 302 | RED | CTENIDIA | HL10-20 | 1 mL | 10-30 | 7/10/19 | 1 | 7/15/19 | 2 | 45 | 36.8 | 1,656 | 13.59 |  
Dabob Bay | Ambient | 306 | RED | CTENIDIA | HL10-21 | 1 mL | 50-70 | 7/10/19 | 2 | 7/15/19 | 3 | 144 | 200 | 28,800 | 2.50 |  
Dabob Bay | Ambient | 304 | RED | CTENIDIA | HL6-19 | 1 mL | 50-70 | 7/10/19 | 1 | 7/15/19 | 2 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Dabob Bay | Ambient | 305 | RED | CTENIDIA | HL6-20 | 1 mL | 20-40 | 7/10/19 | 1 | 7/15/19 | 2 | 149 | 82 | 12,218 | 6.10 |  
Dabob Bay | Ambient | 303 | RED | CTENIDIA | HL6-21 | 1 mL | 80-100 | 7/12/19 | 1 | 7/16/19 | 2 | 149 | 120 | 17,880 | 4.17 |  
Dabob Bay | Ambient | 307 | RED | CTENIDIA | HL6-16 | 1 mL | 70-100 | 7/12/19 | 4 | 7/16/19 | 3 | 149 | 95.4 | 14,215 | 5.24 |  
Dabob Bay | Ambient | 308 | RED | CTENIDIA | HL6-17 | 1 mL | 70-90 | 7/12/19 | 4 | 7/16/19 | 4 | 149 | 63.6 | 9,476 | 7.86 |  
Dabob Bay | Ambient | 309 | RED | CTENIDIA | HL6-18 | 1 mL | 80-100 | 7/12/19 | 6 | 7/16/19 | 4 | 149 | 200 | 29,800 | 2.50 |  
  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |  
Oyster Bay | High | 311 | ORANGE | CTENIDIA | SN6-16 | 1 mL | 80-100 | 7/10/19 | 2 | 7/12/19 | 1 | 144 | 174 | 25,056 | 2.87 |  
Oyster Bay | High | 312 | ORANGE | CTENIDIA | SN6-17 | 1 mL | 80-100 | 7/10/19 | 2 | 7/15/19 | 2 | 144 | 97.4 | 14,026 | 5.13 |  
Oyster Bay | High | 313 | ORANGE | CTENIDIA | SN6-18 | 1 mL | Not recorded | 7/10/19 | 3 | 7/15/19 | 2 | 144 | 76.2 | 10,973 | 6.56 |  
Oyster Bay | High | 314 | ORANGE | CTENIDIA | SN6-19 | 1 mL | Not recorded | 7/10/19 | 3 | 7/15/19 | 2 | 149 | 49.6 | 7,390 | 10.08 |  
Oyster Bay | High | 315 | ORANGE | CTENIDIA | SN6-20 | 1 mL | 70-100 | 7/10/19 | 4 | 7/15/19 | 2 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Oyster Bay | High | 316 | ORANGE | CTENIDIA | SN6-21 | 1 mL | 100 + | 7/12/19 | 4 | 7/16/19 | 3 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Oyster Bay | High | 317 | ORANGE | CTENIDIA | SN6-22 | 1 mL | 70-90 | 7/12/19 | 5 | 7/16/19 | 3 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Oyster Bay | High | 318 | ORANGE | CTENIDIA | SN6-23 | 1 mL | 60-80 | 7/12/19 | 6 | 7/16/19 | 4 | 149 | 182 | 27,118 | 2.75 |  
Oyster Bay | High | 319 | ORANGE | CTENIDIA | SN6-24 | 1 mL | 20-40 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 61.8 | 9,208 | 8.09 |  
  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |  
Oyster Bay | Ambient | 321 | YELLOW | CTENIDIA | SN6-25 | 1 mL | 60-80 | 7/10/19 | 2 | 7/12/19 | 1 | 144 | 188 | 27,072 | 2.66 |  
Oyster Bay | Ambient | 322 | YELLOW | CTENIDIA | SN6-26 | 1 mL | 20-40 | 7/10/19 | 2 | 7/15/19 | 2 | 144 | 63.4 | 9,130 | 7.89 |  
Oyster Bay | Ambient | 323 | YELLOW | CTENIDIA | SN6-27 | 1 mL | 20-40 | 7/10/19 | 3 | 7/15/19 | 2 | 144 | 110 | 15,840 | 4.55 |  
Oyster Bay | Ambient | 324 | YELLOW | CTENIDIA | SN6-28 | 1 mL | 40-60 | 7/10/19 | 3 | 7/15/19 | 2 | 149 | 164 | 24,436 | 3.05 |  
Oyster Bay | Ambient | 325 | YELLOW | CTENIDIA | SN6-29 | 1 mL | 70-100 | 7/10/19 | 5 | 7/15/19 | 2 | 149 | 200 | 29,800 | 2.50 |  
Oyster Bay | Ambient | 326 | YELLOW | CTENIDIA | SN6-30 | 1 mL | 70-100 | 7/12/19 | 5 | 7/16/19 | 3 | 149 | 168 | 25,032 | 2.98 |  
Oyster Bay | Ambient | 327 | YELLOW | CTENIDIA | SN6-31 | 1 mL | 40-60 | 7/12/19 | 6 | 7/16/19 | 3 | 149 | 94.6 | 14,095 | 5.29 |  
Oyster Bay | Ambient | 328 | YELLOW | CTENIDIA | SN6-32 | 1 mL | 50-70 | 7/12/19 | 6 | 7/16/19 | 4 | 149 | 176 | 26,224 | 2.84 |  
Oyster Bay | Ambient | 329 | YELLOW | CTENIDIA | SN6-33 | 1 mL | 70-100 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 150 | 22,350 | 3.33 |  
  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |  
Fidalgo Bay | High | 331 | GREEN | CTENIDIA | NF6-16 | 1 mL | 40-60 | 7/10/19 | 2 | 7/12/19 | 1 | 144 | 56.4 | 8,122 | 8.87 |  
Fidalgo Bay | High | 332 | GREEN | CTENIDIA | NF6-17 | 1 mL | 10-40 | 7/10/19 | 2 | 7/15/19 | 2 | 144 | 63.4 | 9,130 | 7.89 |  
Fidalgo Bay | High | 333 | GREEN | CTENIDIA | NF6-18 | 1 mL | 40-60 | 7/10/19 | 3 | 7/15/19 | 2 | 144 | 82.8 | 11,923 | 6.04 |  
Fidalgo Bay | High | 334 | GREEN | CTENIDIA | NF6-19 | 1 mL | 30-50 | 7/10/19 | 3 | 7/15/19 | 2 | 149 | 66.2 | 9,864 | 7.55 |  
Fidalgo Bay | High | 335 | GREEN | CTENIDIA | NF6-20 | 1 mL | 90-110 | 7/10/19 | 4 | 7/15/19 | 2 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Fidalgo Bay | High | 336 | GREEN | CTENIDIA | NF6-21 | 1 mL | 90-110 | 7/12/19 | 4 | 7/16/19 | 3 | 149 | 130 | 19,370 | 3.85 |  
Fidalgo Bay | High | 337 | GREEN | CTENIDIA | NF6-22 | 1 mL | 100 + | 7/12/19 | 6 | 7/16/19 | 3 | 149 | 198 | 29,502 | 2.53 |  
Fidalgo Bay | High | 338 | GREEN | CTENIDIA | NF6-23 | 1 mL | 20-40 | 7/12/19 | 6 | 7/16/19 | 4 | 149 | 83.8 | 12,486 | 5.97 |  
Fidalgo Bay | High | 339 | GREEN | CTENIDIA | NF6-24 | 1 mL | 10-30 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 78.6 | 11,711 | 6.36 |  
  |   |   |   |   |   |   |   |   |   |   |   |   |   |   |   |  
Fidalgo Bay | Ambient | 341 | BLUE | CTENIDIA | NF6-25 | 1 mL | 20-50 | 7/10/19 | 2 | 7/12/19 | 1 | 144 | 92.4 | 13,306 | 5.41 |  
Fidalgo Bay | Ambient | 342 | BLUE | CTENIDIA | NF6-26 | 1 mL | 80-100 | 7/10/19 | 3 | 7/15/19 | 2 | 144 | 178 | 25,632 | 2.81 |  
Fidalgo Bay | Ambient | 343 | BLUE | CTENIDIA | NF6-27 | 1 mL | 50-70 | 7/10/19 | 3 | 7/15/19 | 2 | 144 | 146 | 21,024 | 3.42 |  
Fidalgo Bay | Ambient | 344 | BLUE | CTENIDIA | NF6-28 | 1 mL | 40-70 | 7/10/19 | 5 | 7/15/19 | 2 | 149 | 28.4 | 4,232 | 17.61 |  
Fidalgo Bay | Ambient | 345 | BLUE | CTENIDIA | NF6-29 | 1 mL | 70-100 | 7/10/19 | 5 | 7/15/19 | 2 | 149 | TOO HIGH | >28,000 | #VALUE! |  
Fidalgo Bay | Ambient | 346 | BLUE | CTENIDIA | NF6-30 | 1 mL | 20-40 | 7/12/19 | 5 | 7/16/19 | 3 | 149 | 74 | 11,026 | 6.76 |  
Fidalgo Bay | Ambient | 347 | BLUE | CTENIDIA | NF6-31 | 1 mL | 10-30 | 7/12/19 | 6 | 7/16/19 | 3 | 149 | 89.8 | 13,380 | 5.57 |  
Fidalgo Bay | Ambient | 348 | BLUE | CTENIDIA | NF6-32 | 1 mL | 50-70 | 7/12/19 | 6 | 7/16/19 | 4 | 149 | 51.4 | 7,659 | 9.73 |  
Fidalgo Bay | Ambient | 349 | BLUE | CTENIDIA | NF6-33 | 1 mL | 10-40 | 7/12/19 | 7 | 7/16/19 | 4 | 149 | 55.4 | 8,255 | 9.03 |  

