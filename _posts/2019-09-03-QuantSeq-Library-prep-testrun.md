---
layout: post
title: QuantSeq Library Prep test-run
---

Katherine suggested I work through the library prep protocol with a few samples to practice and work out kinks. From her experience, the libraries she generated later in the game were of higher quaity. I'm generating 8 test libraries - this was her recommendation based  on the heavy use of 8-channel pipettes. 

Following the [QuantSeq manual]() and tips from Katherine. 

Quick reference for Libary Generation

![image](https://user-images.githubusercontent.com/17264765/64201717-2b3c9780-ce44-11e9-8479-621e22366b9d.png)

### Notes from this test run: 

Started on 8/20/2019

Prior to beginning, I created programs on the [PTC-200 DNA Engine Cycler](https://timothyspringer.org/files/tas/files/biorad-ptc200_thermalcycler.pdf). Programs are labeled "Quantseq-#", with the # corresponding to step number in the QuantSeq manual.   

### first strand cDNA synthesis 
- Based on Katherine's suggestions, I will generate 40 libraries at one, in 5 rows of 8 on a PCR plate. When adding solutions to each well, rather than using a single channel pipette and transferring solutions to individual wells, I'm going to use PCR strips, load enough of each solution into 8 wells (with a bit excess), and use a multichannel to distribute. This way I can hopefuly reduce time. 
- Between each step I need to quickly "spin down" my PCR plate.  Our benchtop centrifuge has a minimum run time of 1 minute, and with the time it takes to accelerate and decelerate total time is ~4 minutes. The "quickly spin down" instructions do not define a centrifuge speed to use, so I set it to 3 rcf based on whatt is  specified in the equipmentt list - "Benchtop centrifuge (3,000 x g, rotor compatible with 96-well plates". **To reduce the amount of time to spin down plates, I should either set the speed lower, OR simply start, then stop the centrifuge manually.**  

### RNA removal 
- No notes.

### second strand cDNA synthesis 
- SS1 and USS are indeed viscous; hold pipette in place when pulling volumes.
- SAFE STOPPING POINT

Placed in -20C overnight. 

### Purificaion 
(this is 1 of 2 purifications, dubbed "pre-PCR")
- As Katherine hinted, it's important to have a magnetic plate that fits the PCR plates used. The plates we have just have rods - no wells for plates to hold plates in place, which doesn't work. **I ordered a magnet/plate from ebay to hopefully improve the process.** 
- SAFE STOPPING POINT.  

Placed in -20 on 8/21/19 until next step (qPCR assay).  

### qPCR assay for optimal # cycles 
Performed on 9/3/2019  
- Created a custom qPCR protocol with Sam's help - 

| qPCR Assay Mastermis Calcs         |              |                          |                |
|------------------------------------|--------------|--------------------------|----------------|
| Item                               | per rxn (uL) | all rxns (inc. NTC) (uL) | all rxns * 1.1 |
| Number of samples                  | 1            | 9                        |                |
| cDNA, diluted to 19uL              | 1.7          | 15.3                     | 16.8           |
| PCR mix (PCR)                      | 7            | 63                       | 69.3           |
| P7 Primer (7000)                   | 5            | 45                       | 49.5           |
| Enzyme mix (E)                     | 1            | 9                        | 9.9            |
| 2.5x SYBR Green I nucleic acid dye | 1.2          | 10.8                     | 11.9           |
| Elution Buffer (EB)                | 14.1         | 126.9                    | 139.6          |
| Mastermix total vol                | 28.3         | 254.7                    | 280.2          |
|                                    |              |                          |                |
|                                    |              |                          |                |
| SYBR Green Calcs                   | Volumes      |                          |                |
| Stock concentration                | 100          |                          |                |
| Desired concentratino              | 2.5          |                          |                |
| Total volume needed 2.5x           | 11.9         |                          |                |
| Volume 100x                        | 0.2970       |                          |                |
| Volume DMSO                        | 11.58        |                          |                |
| Dilution ratio (should be 1:40)    | 0.025        |                          |                |
| Final concentration                | 2.5          |                          |                |

Results:  Located on Owl, https://owl.fish.washington.edu/scaphapoda/qPCR_data/cfx_connect_data/ with today's date. 

No amplification :/ max RFU should be ~10x10^12, and my no-template control (NTC) has same curve as samples. 

![2019-09-03_ qPCR-assay-test-run](https://user-images.githubusercontent.com/17264765/64284204-e715c980-cf0d-11e9-9e54-d4d0e8202cea.png)

First troubleshooting step is to see if I actually synthesized cDNA - I believe I can use the Qubit for that.  If yes, then I messed up the qPCR somehow (wouldn't surprise me). Perhaps there is an issue with bubbles? Maybe BioRad settings needed be adjusted for sybr green?  

I ordered a trial QuantSeq kit (n=4) for trouble shooting, and as a result spoke with the WA respresentative. Notes from our call:  
- If cDNA synthesis did not occur, then I most likely had contamination with organics or salts, which can inhibit 1st strand synthesis and cause cDNA to degrade when trying to degrade RNA. 
- The TurboDNase method might be an issue, since it did not include a column.
- Using a cleaner column on existing RNA may do the trick. I did order one box of Zymo Cleaner-Concentrator (n=50), so could run all my samples through this column. 
- 500 ng of input  RNA is not necesary (she said that "no one uses that much"). 100 ng should be adequate! 

I will receive  the trial kit tomorrow, and Kristy is connecting me with the tech support guy. 

