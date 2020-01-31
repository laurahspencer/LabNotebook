---
layout: post
title: Oly DMG analysis, Jan. 30th, 2020 
---

Today I identified 46 differentially methylated genes among two Olympia oyster populations, Hood Canal and South Sound. This was performed using a binomial GLM and Chi-square tests. The script was adapted from Hollie Putnam's script ([/hputnam/Geoduck_Meth/master/RAnalysis/Scripts/GM.Rmd](https://raw.githubusercontent.com/hputnam/Geoduck_Meth/master/RAnalysis/Scripts/GM.Rmd)), which _may_ have been adopted from the Lieu et al. 2018 paper [](https://doi.org/10.1126/sciadv.aar8028). 

The analysis was performed in a RMarkdown notebook, please see that here: [09-DMG-analysis](https://htmlpreview.github.io/?https://raw.githubusercontent.com/sr320/paper-oly-mbdbs-gen/master/code/09-DMG-analysis.html) 

Here are the GO terms associated with genes of known function. Some notes:  
 -- 18 out of the 46 genes were annotated with GO terms  
 -- 9 out of the 46 genes were annotated but did not have associated GO terms (may have to find those manually ...)   
 -- 19 out of the 46 genes were of unknown function   

| term ID    	| description                                          	| frequency 	| pin? 	| log10 p-value 	| uniqueness 	| dispensability 	|
|------------	|------------------------------------------------------	|-----------	|------	|---------------	|------------	|----------------	|
| GO:0006468 	| protein phosphorylation                              	| 4.137 %   	|      	| -3.7877       	| 0.40       	| 0.00           	|
| GO:0006807 	| nitrogen compound metabolic process                  	| 38.744 %  	|      	| -2.2764       	| 0.78       	| 0.03           	|
| GO:0006207 	| 'de novo' pyrimidine nucleobase biosynthetic process 	| 0.192 %   	|      	| -2.2764       	| 0.46       	| 0.06           	|
| GO:0006281 	| DNA repair                                           	| 2.234 %   	|      	| -2.4853       	| 0.50       	| 0.20           	|
| GO:0006030 	| chitin metabolic process                             	| 0.077 %   	|      	| -1.6311       	| 0.49       	| 0.21           	|
| GO:0006520 	| cellular amino acid metabolic process                	| 5.591 %   	|      	| -2.2764       	| 0.42       	| 0.35           	|
| GO:0006412 	| translation                                          	| 5.686 %   	|      	| -2.4853       	| 0.28       	| 0.55           	|
| GO:0016567 	| protein ubiquitination                               	| 0.523 %   	|      	| -1.4336       	| 0.44       	| 0.56           	|

![image](https://user-images.githubusercontent.com/17264765/73504720-280eee80-4385-11ea-9ca1-a95f8c8b86b0.png)

