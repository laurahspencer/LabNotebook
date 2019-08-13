---
layout: post
title: Prepping for QuantSeq library prep
---

I am usig QuantSeq to look at gene expression in Oly larvae and adult ctenidia tissue, both collecting as part of the OA/Temp study. I have some preliminary data from the larvae which looks very interesting, suggests that adult exposure results in altered gene express in newly released larvae, despite larvae being fertilized and grown in common conditions. The preliminary run was 8 samples - 4 from treated parents, 4 from ambient parents - I'm sequencing a bunch more larvae from all treatments and cohorts to see if the parental treatment effects are consistent across populations. I will also detect gene expression differences among populations, which should also be interesting given their differing growth and survival rates. 

QuantSeq, which is also known as "TagSeq" or "3' mRNA-Seq", sequences from the 3' end of a transcript, and when building libraries it only generates one fragment per transcript, thus it should be highly accurate for gene expression studies. Library prep uses total RNA as input, and entails the following steps: 


![image](https://user-images.githubusercontent.com/17264765/62904639-0446e600-bd1c-11e9-9809-cf62acbc4c25.png)

I spoke with Katherine to get some tips on this protocol.  She says that it's a beast, particularly since I have a ton of samples.  She was extremely helpful, and in addition to tips via phone she wrote down important things [in a google doc](https://docs.google.com/document/d/1-Q_ijbhPoeK40dOBciHH9Qm0jVEbwJbQ39j1RybclgQ/edit?pli=1). I've pasted it here: 

### Tips  
- Concentrate samples if dilute  
- Randomize sample types between rows and lanes  
- 500 ng RNA initially (or at least 200), try to be consistent across samples but ok if not  
- Repeater pipette for ethanol  
- Magnetic plate that fits 96 well plate well  
- Make thermocycler protocols ahead of time. Make sure ramp speed is correct.  
- Make 250uL/sample 80% ethanol ahead of both purification steps    
- After library prep is done, do Quibit HS DNA on each library.   
- Use bioanalyzer HS DNA to verify library quality. This step can take a long time (and be kinda pricey), as it takes 45 min to run 10 samples. If your samples generally seem  to all work, you can decide if you want to just spot check (across tissute types, batches, etc.). I recommend always doing bioanalyzer on samples that require >=16 cycles or had low input/quality DNA as these are more likely to fail   
- I will go through a TON of 200 uL pipette tips   
- Optimal # samples per batch is 40   
- Practice on 8 samples (extra samples) to get used to protocol   
- I will need foil or 96 well-plate tape. Always spin down plates before removing covering, to ensure no cross contamination.   
- During purification, make sure beads are all stuck to wall. If I accidentally suck some up into the pipette, put it back!   
- make fresh SYBR dilution: 1.25 uL 100x SYBR stock + 23.75 uL DMSO = 25 uL 5x SYBR   
https://www.lumiprobe.com/p/sybrgreen-realtime-pcr  
  - _Laura update: protocol says I need 2.5x working stock. Instead dilute  1.25 uL 100x SYBR into 48.75 uL DMSO, for 50 uL of 2.5x SYBR working solution._   
- Need to create a custom qPCR protocol for this, to determinet he # cycles to use for amplification. Samples will have varying # cycles! When calculating how many cycles, always round down (i.e. bad to over-cycle).   
- When doing PCR to amplify libraries, keeping track of sample location is hard. Be careful, plan accordingly.  
- To sequence, Katherine used a High Seq 4000, and did ~80 samples per lane. QUESTION: do technical replicates?   
- If sequencing facility has never done QuantSeq before, get protocol from Katherine (e.g. concentration to use).   

### Stuff to get 
- Lexogen PCR Add-on Kit for Illumina (Cat. No. 020).  
- SYBR green: umiprobe dsGreen for Real-Time PCR x100 (100 uL). https://www.lumiprobe.com/p/sybrgreen-realtime-pcr   
- Magnetic plate that works well with 96 well plate (PCR strips ok too)   
- 20-50 uL multichannel   
- 150 uL multichannel   
- To concentrate RNA: https://www.zymoresearch.com/collections/rna-clean-concentrator-kits-rcc/products/rna-clean-concentrator-5   
- Maybe more 200 uL pipette tips   

### Things to do and to acquire prior to library prep: 
X pcr add-on kit for illumina - rec'd 8/13  
X quibit tubes - rec'd 8/13/19  
X QuantSeq library prep kits - rec'd 8/9. 
X Magnetic plate that fits 96 well plates - we have 2, located in PCR supplies drawer.  
X Figure out how to quickly spin down well plates - use big centrifuge in FTR 213. Received instruction.   Important points: use a 2nd plate to balance. Weigh sample plate and add water to balance plate to match weight.   
X Order Actin primer set that worked on Oly larvae gDNA - Sam submitted order 8/13/19.     
- Calibrate multichannel pipettes - need 2: 
  - 20-50 uL (have?)  
  - 200 uL  
- Zymo concentrator - purchase requested 8/12. We have on kit already, I can probably use some of that if needed.  
- SYBR green - purchase requested 8/12.   
- Re-do qPCR with subset of samples to ensure Turbo DNase worked.  
- Quantify DNased RNA that still need it. 
- Identify samples that need to be concentrated, then use Zymo to concentrate it.   
- Check quality of RNA for a subset of samples - across tissues and extraction batches - using Bioanalyzer.  Looking for a "quality value" >=5.   
- Move a thermocycler to my bench  
- Create custom protcols on thermocycler   

THEN: Test whole protocol with 8 samples where I have ample RNA.  

