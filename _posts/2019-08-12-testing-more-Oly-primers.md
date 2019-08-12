---
layout: post
title: Testing more Oly primers for DNA contamination
---

The last two rounds of qPCR I did to test for DNA contamination (Rounds III & IV) had some odd results -  melt curves showed some samples' RFU to increase at high temp. I conferred with Sam and he agreed that I should re-do the last 2 runs. Also, suggested looking at the amplification curves. Turns out that none of my runs resulted in the positive control (my DNA from larvae) amplifying. Some points from Sam: "Regarding your lack of amplification of your positive control. This is most likely due to the presence of introns in the gDNA. I'm guessing the primers were designed off of mRNA, since they were being used to evaluate gene expression, and mRNA doesn't have introns. The presence of introns in gDNA usually is a problem in qPCRs because the size of the gDNA that falls between the two primer annealing regions is too large to be amplified in the time frame used for qPCR. The solution is to find an existing primer set that is known to amplify gDNA during qPCR (amplicon size <200bp)." 

One primer that Sam successfully used before is gone, so I'm testing 3 other primer sets that were ordered at the same time to find one that could work. To test, I'm running 3 positive controls, and 3 no template controls for each primer. I'm testing those outlined in blue, below: 

Gone but worked in past: 
- 1505	Ol_Act_F
- 1504	Ol_Act_R

Testin: 
- 1509	Ol_Ef1a_F
- 1508	Ol_Ef1a_R
- 1503	Ol_Arp_F
- 1502	Ol_Arp_R
- 1678	Flk_Actin_FWD
- 1677	Flk_Actin_REV


For each of the primer sets , I diluted 1 uL of the FWD and REV into 9 uL, then created mastermixes using these calcs: 

| Date                           | Test primer |
|--------------------------------|-------------|
| # Samples                      | 3           |
| # Reactions                    | 6           |
| Template (RNA sample) (uL)     | 6.0         |
| Sso Fast (uL)                  | 66.0        |
| Pf, 10 uM (uL)                 | 3.3         |
| Pr, 10 uM (uL)                 | 3.3         |
| DEPC-treated water (uL)        | 52.8        |
| Total Volume in mastermix (uL) | 125.4       |


Plate setup. I used DNA sample 2a as my positive controls. 

| 8 | 9       | 10      | 11      | 12 |   |
|---|---------|---------|---------|----|---|
| A | EFG1a + | EFG1a + | EFG1a + | x  | x |
| B | EFG1a - | EFG1a - | x       | x  | x |
| C | x       | x       | x       | x  | x |
| D | ARP +   | ARP +   | ARP +   | x  | x |
| E | ARP -   | x       | x       | x  | x |
| F | x       | ARP -   | ARP -   | x  | x |
| G | Actin + | Actin + | Actin + | x  | x |
| H | Actin - | Actin - | Actin - | x  | x |
