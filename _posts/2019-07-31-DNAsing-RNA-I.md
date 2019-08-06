---
layout: post
title: DNasing RNA, Round I
--- 

According to the QuantSeq library prep protocol, I need to ensure no DNA contamination in my RNA samples. Sam advises that all RNAzol-processed samples will definitely have some DNA contamination. So, I am using the Turbo DNase kit to clean my RNA.  

## July 31st - DNased RNA, qPCR to identify DNA contamination, batch 1 (n=30)

I ran a first batch of reactions (n=30) using the Turbo DNase kit. The kits we had in the -20 freezer were old (from 2014), so I walked over to the BioSciences stock room (J wing) and purchased 2 new kits (50 reactions per; I'll need a couple more kits).  NOTE: each kit is ~$145.

## Turbo DNA Protocol 

All reactions used 50 uL RNA, as per the manual's example. Here were my steps: 
- Labeled 2 sets of 0.5 mL microcentrifuge tubes with RNA sample names
- Transfered 1 uL of DNase into one set of tubes 
- Transfered 5 uL of Turbo DNase Buffer into each tube 
- Transfered 50 uL of RNA into tubes 
- Mixed gently - used low setting on the vortexer. 
- Incubated at 37C - used the thermocycler in FTR 209 - for 20 minutes 
- Removed samples from thermocycler, added 5 uL inactivation reagent solution. Prior to pipetting this inactivation reagent, I vortexted it thoroughly. 
- Incubated at room temperature for 5 minutes. Vortexed each sample briefly twice during this incubation time to keep solution mixed. 
- Centrifuged samples for 90 seconds at 10,000 rcf 
- Carefully transferred supernatant to fresh, labeled tubes. 
- Held DNased RNA on ice, while I learned how to run qPCR 

## qPCR to assess DNA contamination 

I will use the SsoFast enzyme, a DNA polymerase technology that performs a super fast reaction, and the BIORAD CFX Connect qPCR machine.  

Protocol:  
1. Created mastermix for PCR reactions for a total of 64 wells. The following table shows volumes needed for 1 pCCR reaction, then volumes needed for a mastermix for 64 reactions: 


|                                | per reaction | 7/31/19 |
|--------------------------------|--------------|---------|
| # Samples                      | 1            | 30      |
| # Reactions                    | 1.0          | 62      |
| Template (RNA sample) (uL)     | 1.0          | 62.0    |
| Sso Fast (uL)                  | 10.0         | 682.0   |
| Pf, 10 uM (uL)                 | 0.5          | 34.1    |
| Pr, 10 uM (uL)                 | 0.5          | 34.1    |
| DEPC-treated water (uL)        | 8.0          | 545.6   |
| Total Volume in mastermix (uL) | 20.0         | 1,357.8 |


Following Sam's [lab notebook entry](https://robertslab.github.io/sams-notebook/2018/11/15/qPCR-Ronit's-DNased-C.gigas-RNA-with-Elongation-Factor-Primers.html), I used elongation factor primers to check for DNA contamination: 
- EF1_qPCR_5’ (SRID 309) (Forward primer, Pf, SRID = 310)  
- EF1_qPCR_3’ (SRID 310) (Reverse primer, Pr, SRID = 309)

These primer stocks are stored in a small fridge in FTR 213, and can be found using the [primer database](https://docs.google.com/spreadsheets/d/1Tu1c9mzJFjL2vN5xwQctRLGQhLRNB0heha5ZQMrNsOA/edit#gid=1593695112). The stock concentrations are 100 micromolar. I need to use a working stock at 10 uM, so I melted the stocks and diluted 15 uL of each stock in 135 uL DEPC-treated ultrapure water (150 uL total volume). 

The total volume per reaction is 20 uL.  After creating the mastermix, I pipetted 19 uL of mastermix into a qPCR well plate. NOTE: the type of plate is specific - it's a white plate, and "low profile" - which is specified on the qPCR software. I then pipetted 1 uL of each sample (i.e. template), in duplicate, to the well plate. I loaded samples horizontally (A1, A2, A3 ... etc.) for ease of reading data downstream.  In addition to including a control sample, which has been processed alongside the other samples since homogenization (sample 571), I included a No Template Control (NTC, 1uL water added instead of a sample), and [DNA isolated from Oly larvae](https://laurahspencer.github.io/LabNotebook/DNA-Isolation-Oly-Larvae/) back in March 2018 (sample 69a, RNA sample 8a) as a positive control. To seal the plate I used the clear tape-like cover, rather than the clear plastic caps. I did not vortex the well plate prior to the qPCR reaction. 

I carried the well plate over to the qPCR room, loaded it onto the CFX Connect, and opened the MAESTRO software on the adjacent computer. I used the Wizard to help configure the run. Here are the steps to execute the run: 

#### Select "User Defined" 
![Capture01](https://user-images.githubusercontent.com/17264765/62267596-81e82900-b3e1-11e9-9250-d211cf961f95.PNG)

#### Select Protocol: "CFX_2StepAmp_EVAGreen+Melt.prcl"
![Capture02](https://user-images.githubusercontent.com/17264765/62267597-81e82900-b3e1-11e9-9a8f-e6f317aadeef.PNG)

#### Select plate file: "QuickPlate_96 wells_sybr_white.pltd" - this ensures that all wells are measured. We don't assign sample names prior to running, but can edit the data file after completion. 
![Capture03](https://user-images.githubusercontent.com/17264765/62267598-8280bf80-b3e1-11e9-83a6-4bb5f9b766e4.PNG)

#### Select "Next"
![Capture04](https://user-images.githubusercontent.com/17264765/62267599-8280bf80-b3e1-11e9-8a4b-1fb3b2932e89.PNG)

#### Select "Save" -  it will automatically save the file to Owl and filename will include the run date. 
![Capture05](https://user-images.githubusercontent.com/17264765/62267600-8280bf80-b3e1-11e9-8ee6-84a5fcf8e8f0.PNG)

#### This is a screenshot immediately upon protocol initiation 
![Capture06](https://user-images.githubusercontent.com/17264765/62267601-8280bf80-b3e1-11e9-9a76-9bc82fc978ac.PNG)

I downloaded MAESTRO to my computer (Mac version), and edited the plate setup to include sample names, and color coded melt curves by sample type:  GREEN is positive control (n=2); RED is NTC (n=1), and PINK is the homogenization/isolation/DNase control (n=1); BLUE are the samples.  

![image](https://user-images.githubusercontent.com/17264765/62268011-3767ac00-b3e3-11e9-8545-faf57bff44ac.png)

Data and report are saved on github in the [O.lurida_Stress repo](https://github.com/laurahspencer/O.lurida_Stress/tree/master/Data/RNA-DNA-Isolation/2019-07-31_qPCR-DNA-contamination). 

The melt curve doesn't look like [Sam's recent run](https://robertslab.github.io/sams-notebook/2018/11/15/qPCR-Ronit's-DNased-C.gigas-RNA-with-Elongation-Factor-Primers.html). However, I realize that the primers were not O. lurida, but were C. gigas.  I didn't think to ask whether the primers needed to be O. lurida specific, but I'm guessing yes.  I will plan to move forward with the next batch of Turbo DNase-ing, and will figure out which primers are optimal. Interestingly I did see some DNA, and a melt temperature, for the positive controls, but the fluorescence was not as high as [Sam's example](https://robertslab.github.io/sams-notebook/2018/11/15/qPCR-Ronit's-DNased-C.gigas-RNA-with-Elongation-Factor-Primers.html). Also interesting is that my homogenization/isolation control (pink) had a weird peak, suggesting some contamination. 

![image](https://user-images.githubusercontent.com/17264765/62268260-1fdcf300-b3e4-11e9-9b21-ea58ef98770b.png)

Here ares ome qPCR notes from Sam's instructions: 
- Keep RNA on ice while working with them, and store in -80 always. 
- There are 2x SsoFast aliquots in the fridge, and also in the freezer in the "PCR supplies" box in the -20 (both in FTR 209). 
- Mastermixes should be used the same day they are prepared, but can sit on ice for a few hours.  
- qPCR plates can be prepared, then sealed and held in the fridge for a bit.  For example, I could prepare one qPCR plate, then while it is running I can prepare another and hold it in the fridge until the machine is ready again. 
- Always use the button to open/close the BioRAD CFX Connect lid - don't manually close the lid 

## Quantified DNased RNA 

Used Qubit HS RNA to measure RNA concentration in DNased samples. Approximate volume remaining for DNased RNA is 50 uL. I find it odd that some of my samples have more concentrated RNA after the DNasing. I will look in to that.  

| Date larvae collected | Cohort        | Treatment   | TISSUE SAMPLE # | Homo./RNA TUBE # | VOL RNAzol (mL) | MASS TISSUE (mg) | [RNA] ng/uL | Volume for DNase treatment | Amount of RNA in Dnase treatment (ug), max is 10 ug | Date Turbo Dnase treatment | [RNA] after Turbo Dnase treatment |
|-----------------------|---------------|-------------|-----------------|------------------|-----------------|------------------|-------------|----------------------------|-----------------------------------------------------|----------------------------|-----------------------------------|
| 5/24/17               | Dabob Bay     | 10 Ambient  | 14-A            | 401              | 1               | 100              | 52.0        | 50                         | 2.60                                                | 7/31/19                    | 93.4                              |
| 5/31/17               | Dabob Bay     | 10 Ambient  | 31-A            | 402              | 1               | 10               | 140.0       | 50                         | 7.00                                                | 7/31/19                    | 114.0                             |
| 5/26/17               | Dabob Bay     | 10 Low      | 23-A            | 411              | 1               | 10               | 57.2        | 50                         | 2.86                                                | 7/31/19                    | 72.6                              |
| 5/27/17               | Dabob Bay     | 10 Low      | 27-A            | 412              | 1               | 10               | 60.8        | 50                         | 3.04                                                | 7/31/19                    | 31.2                              |
| 6/12/17               | Dabob Bay     | 6 Ambient   | 59-A            | 421              | 1               | 10               | 43.0        | 50                         | 2.15                                                | 7/31/19                    | 57.6                              |
| 6/7/17                | Dabob Bay     | 6 Low       | 51-A            | 431b             | 1               | 20               | 61.2        | 50                         | 3.06                                                | 7/31/19                    | 83.0                              |
| 6/17/17               | Dabob Bay     | 6 Low       | 72-A            | 432              | 1               | 50               | 47.6        | 50                         | 2.38                                                | 7/31/19                    | 74.0                              |
| 5/25/17               | Fidalgo Bay   | 10 Ambient  | 20-A            | 441              | 1               | 70               | 46.0        | 50                         | 2.30                                                | 7/31/19                    | 16.2                              |
| 6/3/17                | Fidalgo Bay   | 10 Ambient  | 38-A            | 442b             | 1               | 80               | 56.2        | 50                         | 2.81                                                | 7/31/19                    | 69.8                              |
| 5/24/17               | Fidalgo Bay   | 10 Low      | 16-A            | 451              | 1               | 70               | 68.4        | 50                         | 3.42                                                | 7/31/19                    | 68.4                              |
| 5/24/17               | Fidalgo Bay   | 10 Low      | 18-A            | 452b             | 1               | 80               | 48.4        | 50                         | 2.42                                                | 7/31/19                    | 97.2                              |
| 5/26/17               | Fidalgo Bay   | 6 Ambient   | 22-A            | 461b             | 1               | 100              | 54.0        | 50                         | 2.70                                                | 7/31/19                    | 84.0                              |
| 5/29/17               | Fidalgo Bay   | 6 Ambient   | 29-A            | 462b             | 1               | 60               | 69.8        | 50                         | 3.49                                                | 7/31/19                    | 106.0                             |
| 5/25/17               | Fidalgo Bay   | 6 Low       | 19-A            | 471b             | 1               | 100              | 71.0        | 50                         | 3.55                                                | 7/31/19                    | 108.0                             |
| 5/26/17               | Fidalgo Bay   | 6 Low       | 21-A            | 472b             | 1               | 70               | 64.0        | 50                         | 3.20                                                | 7/31/19                    | 97.0                              |
| 5/20/17               | Oyster Bay C1 | 10 Ambient  | 02-A            | 481              | 1               | 40               | 64.4        | 50                         | 3.22                                                | 7/31/19                    | 89.2                              |
| 5/20/17               | Oyster Bay C1 | 10 Ambient  | 04-A            | 482              | 1               | 60               | 67.2        | 50                         | 3.36                                                | 7/31/19                    | 22.2                              |
| 5/23/17               | Oyster Bay C1 | 10 Ambient  | 09-A            | 484              | 1               | 40               | 66.2        | 50                         | 3.31                                                | 7/31/19                    | 58.4                              |
| 6/15/17               | Oyster Bay C1 | 10 Ambient  | 66-A            | 491              | 1               | 20               | 126.0       | 50                         | 6.30                                                | 7/31/19                    | 58.4                              |
| 6/14/17               | Oyster Bay C1 | 10 Low      | 62-A            | 506              | 1               | 80               | 63.8        | 50                         | 3.19                                                | 7/31/19                    | 29.2                              |
| 6/5/17                | Oyster Bay C1 | 6 Ambient   | 45-A            | 513              |                 | 30               | 156.0       | 50                         | 7.80                                                | 7/31/19                    | 142.0                             |
| 5/21/17               | Oyster Bay C1 | 6 Low       | 01-A            | 521              | 1               | 70               | 54.4        | 50                         | 2.72                                                | 7/31/19                    | 66.6                              |
| 5/22/17               | Oyster Bay C1 | 6 Low       | 07-A            | 522              | 1               | 20               | 60.8        | 50                         | 3.04                                                | 7/31/19                    | 32.2                              |
| 6/15/17               | Oyster Bay C1 | 6 Low       | 68-A            | 528              | 1               | 30               | 162.0       | 50                         | 8.10                                                | 7/31/19                    | 87.6                              |
| 5/24/17               | Oyster Bay C2 | 10 Ambient  | 17-A            | 531              | 1               | 60               | 88.2        | 50                         | 4.41                                                | 7/31/19                    | 95.4                              |
| 5/23/17               | Oyster Bay C2 | 10 Low      | 12-A            | 541              | 1               | 40               | 45.6        | 50                         | 2.28                                                | 7/31/19                    | 44.4                              |
| 5/24/17               | Oyster Bay C2 | 10 Low      | 13-A            | 542              | 1               | 30               | 82.0        | 50                         | 4.10                                                | 7/31/19                    | 32.8                              |
| 6/3/17                | Oyster Bay C2 | 6 Ambient   | 41-A            | 552b             | 1               | 80               | 64.8        | 50                         | 3.24                                                | 7/31/19                    | 74.6                              |
| 5/21/17               | Oyster Bay C2 | 6 Low       | 05-A            | 561              | 1               | 40               | 43.4        | 50                         | 2.17                                                | 7/31/19                    | 28.0                              |
| NA                    | RNA Control   | RNA Control |                 | 571              | 1               | 10               | LOW         | 50                         | LOW                                                 | 7/31/19                    | LOW                               |


