---
layout: post
title: DNasing RNA, Round II-V
---

## DNasing, Rounds II-V

I performed 3 rounds of DNase reactions (n=30 per round) on the Oly larval and ctenidia RNA. I followed the same protocol as [Round 1](https://laurahspencer.github.io/LabNotebook/DNAsing-RNA-I/), using 50 uL RNA for each sample, 5 uL buffer, 1 uL DNase, and 5 uL inactivation reagent.  

There were two samples - 38 and 46 (extracted previously in 2018) - which had less than 50 uL total volume in the original RNA sample. For these I used 25 uL RNA, 2.5 uL buffer, 1 uL DNase and 2.5 uL inactivation reagent. 

Several of the samples also required dilution prior to DNasing, since the maximum amount of RNA in a 50 uL reaction is 10 ug. For these I added 50 uL DEPC-treated water and vortexed to mix, prior to DNasing:  477, 43, 564, 291, 304, 306, 309, 315, 316, 317, 321, 325, 335, 345. 

#### Some images of the Turbo DNase process 

This is the Turbo DNase kit I used. 

![IMG_8846](https://user-images.githubusercontent.com/17264765/62645654-8476e100-b901-11e9-9ce4-01f7f64efa3c.JPG)

In it is: 
  A) Buffer 
  B) TurboDNase enzyme 
  C) Inactivation Reagent 
  D) Ultrapure water (to resuspend inactivation reagent, if it has dried). 

NOTE: the kit says it's good for 50 reactions, but that's for 100 uL volume / reaction. For me, the limiting factor was the DNase enzyme - there was 120 uL DNsae (used 1 uL per sample) - so I did ~110 samples with one kit before pulling DNase from the other. 

![IMG_8847](https://user-images.githubusercontent.com/17264765/62645657-850f7780-b901-11e9-9ed8-77fea0f38a91.JPG)

2 sets of labeled 0.5 mL tubes. Step 1 is to transfer 50 uL RNA into one set of tubes.  

After adding buffer (10% of RNA volume, in my case 5 uL), and DNase (1 uL), I vortexted gently and incubated at 37C for ~25 minutes in the Thermo Cycler. It takes no time to heat up, keeps track of the time. Maximum # tubes at once is 30. 

![IMG_8851](https://user-images.githubusercontent.com/17264765/62646585-7cb83c00-b903-11e9-9940-1220a7e8dc96.JPG)

![IMG_8855](https://user-images.githubusercontent.com/17264765/62646583-7cb83c00-b903-11e9-861f-143226639a73.JPG)

![IMG_8852](https://user-images.githubusercontent.com/17264765/62646581-7c1fa580-b903-11e9-8070-f3fac6192703.JPG)

I forgot to get images of the inactivation reagent step - the solution is milky white, and after mixing and incubating at rooom temp for 5 minutes, samples are centrifuged to "pellet" the reagent, then the supernatant containing RNA is transferred to the 2nd batch of tubes.  NOTE: I held everything on ice while working. 


## qPCR to check for DNA contamination 

I ran 3 more batches of qPCR to check for DNA contamination. 

For these batches, I selected Olympia oyster primers ordered by Jake
 
![Snip20190806_39](https://user-images.githubusercontent.com/17264765/62647339-2946ed80-b905-11e9-8508-9de8a5eae2f9.png)

![IMG_8853](https://user-images.githubusercontent.com/17264765/62646576-7b870f00-b903-11e9-8ae6-08565af21b40.JPG)

Primers are located in the freezer in FTR 213.

![IMG_8850](https://user-images.githubusercontent.com/17264765/62646580-7c1fa580-b903-11e9-9e0d-035b63b16798.JPG)

Found the Oly primers using the database. 

![image](https://user-images.githubusercontent.com/17264765/62647320-1fbd8580-b905-11e9-9a94-d871b0c7ee0a.png)

I created working stocks of my primers - diluted 12 uL each primer in 108 uL DECP-treated water, creating 120 uL at 10 uM for each. 

Created a mastermix for all reactions using the following calculations, then pipetted 19 uL mastermix onto qPCR plates, and 1 uL each sample in duplicate. 

| Date                           | 1 plate , 32 rxns | All 3 reactions |
|--------------------------------|-------------------|-----------------|
| # Samples                      | 32                | 96.0            |
| # Reactions                    | 68                | 204.0           |
| Template (RNA sample) (uL)     | 68.0              | 204.0           |
| Sso Fast (uL)                  | 748.0             | 2,244.0         |
| Pf, 10 uM (uL)                 | 37.4              | 112.2           |
| Pr, 10 uM (uL)                 | 37.4              | 112.2           |
| DEPC-treated water (uL)        | 598.4             | 1,795.2         |
| Total Volume in mastermix (uL) | 1,421.2           | 4,263.6         |


### Round II  

Results look good -  only the positive control (DNA sample 1A, in green) had a detected melt temperature. I realized later that I accidentally used the wrong qPCR protocol - I used "CFX_2StepAmp**60**_EVAGreen+Melt.prcl" instead of "CFX_2StepAmp_EVAGreen+Melt.prcl". The difference is that step 3 is 60C, while in the correct protocol step 3 is 55C. For my purposes, and since results look good, it seems that this error does not warrant a re-do. I'll check with the lab, though. 

![image](https://user-images.githubusercontent.com/17264765/62648861-7b3d4280-b908-11e9-947d-543ee23da723.png)

### Round III

Results look a bit weird, different than before. The fluorescence (RFU) goes up at high temperatures - not sure if this indicates an issue.  Also, two samples had melt temperatures -  543 and 475 - but only in 1 rep for each. 

![Snip20190807_49](https://user-images.githubusercontent.com/17264765/62650582-11269c80-b90c-11e9-96c0-69cb24e79f4f.png)
![Snip20190807_48](https://user-images.githubusercontent.com/17264765/62650583-11269c80-b90c-11e9-8c5a-d994414fde37.png)
![Snip20190807_46](https://user-images.githubusercontent.com/17264765/62650585-11269c80-b90c-11e9-9035-b09af0dd2ff0.png)

### Round IV

Results look okay, except for one well again had a weird increase in RFU at high temps (sample 476, rep 2). However, there was no melt temperature measured in the RNA samples. 

![Snip20190807_52](https://user-images.githubusercontent.com/17264765/62651215-526b7c00-b90d-11e9-919b-9977b5c9ef02.png)


#### All edited CFX files (with sample names, colors) are located in the [qPCR-DNA-contamination](https://github.com/laurahspencer/O.lurida_Stress/tree/master/Data/RNA-DNA-Isolation/qPCR-DNA-contamination) directory in my O.lurida_Stress repo.  

Raw CFX files are saved to owl [here](https://owl.fish.washington.edu/scaphapoda/qPCR_data/cfx_connect_data/)

