---
layout: post
title: C. magister MiSeq analysis continued 
--- 

I ran the MiSeq data through the trimming/filtering/alignment pipeline, and a handful of the samples have very low %CpG methylation compared to the others: samples 5, 6, 9, 13, 14, & kind-of 19. Check out the [Bismark summary report](https://nbviewer.jupyter.org/github/laurahspencer/C.magister_methyl-oa/blob/master/qc-processing/MiSeq-QC/bismark/bismark_summary_report.html), and the [MultiQC report](https://nbviewer.jupyter.org/github/laurahspencer/C.magister_methyl-oa/blob/master/qc-processing/MiSeq-QC/FastQC/multiqc_report.html) (on trimmed/filtered reads). In an effort to identify possible causes, I looked at correlations among %CpG meth and various library prep and sequencing metrics (from [this table,](https://raw.githubusercontent.com/laurahspencer/C.magister_methyl-oa/master/qc-processing/MiSeq-QC/mbdall.txt) processed in [this RMarkdown notebook](https://nbviewer.jupyter.org/github/laurahspencer/C.magister_methyl-oa/blob/master/notebooks/Notebook-03_MiSeq-QC-Analysis.html)). Here's a big correlation plot with all library prep / sequencing metrics (CpGmeth is 3rd from the bottom/right). 

![image](https://user-images.githubusercontent.com/17264765/103317298-de3ce700-49df-11eb-9284-c6bc12b9b579.png)

#### The strongest correlation is with "%total reads that are unique": 
![image](https://user-images.githubusercontent.com/17264765/103317305-e2690480-49df-11eb-8d89-9f68f73d9f5b.png)

#### Here's also %meth ~ coverage plots for each sample   

![image](https://user-images.githubusercontent.com/17264765/103317735-e8abb080-49e0-11eb-9df9-0ab68b76821c.png)

#### But some other factors also correlate loosely with %CpG meth 

![image](https://user-images.githubusercontent.com/17264765/103317308-e6952200-49df-11eb-9d8c-e44fea5d4b30.png)
![image](https://user-images.githubusercontent.com/17264765/103317316-eb59d600-49df-11eb-9586-f2fba7c2b77a.png)
![image](https://user-images.githubusercontent.com/17264765/103317324-eeed5d00-49df-11eb-8fe8-ef501ead4fe9.png)
![image](https://user-images.githubusercontent.com/17264765/103317337-f280e400-49df-11eb-9148-83fc046d07e6.png)


Contacting Sam to see if he has any thoughts regarding library prep, and possible impacts to alignment and %meth. 
