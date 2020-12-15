---
layout: post
title: C.magister: MiSeq data processing 
---

I am helping to analyze some initial MBDSeq data from a Dungeness crab OA experiment, run by NOAA (Krista Nichols' group, with Mac) on the MiSeq to do a quick QC before sending the libraries off for full sequencing.

My task:
Run the MiSeq data through a trimming/mapping and alignment pipeline so we can get an idea of mapping efficiency and bisulfite conversion rate (i.e. CHG methylation from Bismark). The pipeline is based on the one developed by the MethCompare group (check out their repo), and which I tested for the WGBS data. See my WGBS pipeline in this Jupyter Notebook Notebook-01_Exploring-WGBS-data.ipynb.

### Check out my Jupyter Notebook entry for more details!: [MBD-01 Processing QC MiSeq data.ipynb](https://nbviewer.jupyter.org/github/laurahspencer/C.magister_methyl-oa/blob/master/notebooks/MBD-01%20Processing%20QC%20MiSeq%20data.ipynb)  

NOTE for Science Hour: I encountered a path/directory issue running Bismark on Mox.  I had to set my working directory to the folder which housed my trimmed reads (rather than specifying the read location).  According to the scripts written by others, for example [the one from the MethCompare group](https://github.com/hputnam/Meth_Compare/blob/master/code/00.02-C1-alignment.sh), this isn't necessary. What gives?  

Here's the Bismark summary report for the MiSeq data (NOTE: 6 of the samples are not here, since their data files were corrupted during transfer from NOAA). Click on the image to view in HTML format: 


[![Bismark Summary Report](https://user-images.githubusercontent.com/17264765/102286490-48885e80-3eed-11eb-8a16-90655e61e051.png)](https://nbviewer.jupyter.org/github/laurahspencer/C.magister_methyl-oa/blob/master/qc-processing/MiSeq-QC/bismark/bismark_summary_report.html)
