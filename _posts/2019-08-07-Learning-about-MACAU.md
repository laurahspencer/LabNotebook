---
layout: post
title: Learning about MACAU
---

Today I oriented myself to MACAU (Mixed model Association for Count data via data AUgmentation). MACAU is a program that assess the influence of a continuous predictor variable, e.g. age, on methylation while controlling for relatedness. To do so, it models raw read counts from bisulfie sequencing using a binomial mixed model. The software and manuals are available on the [Zhou lab's website](http://www.xzlab.org/software.html)

To learn about the principles underlying this program, and the problems this program addresses, I read this paper:  
 "A Flexible, Efficient Binomial Mixed Model for Identifying Differential DNA Methylation in Bisulfite Sequencing Data” by Amanda J. Lea, Jenny Tung, Xiang Zhou. [https://doi.org/10.1371/journal.pgen.1005650](https://doi.org/10.1371/journal.pgen.1005650).  
 
 The [MACAU user manual](http://www.xzlab.org/software/macau/MACAUmanual.pdf) is helpful to understand which input files I will need, which include: 
 
 1) Methylated read counts - Matrix containing read counts for methylated sites 
 2) Total read counts -  Matrix containing read counts for all sites
 3) Relatedness matrix -  genetic relationship matrix for all individuals 
 4) Predictor - a vector of a continuous variable 
 
Here are some helpful snippets from the paper and the manual: 

<img src="https://user-images.githubusercontent.com/17264765/62661775-f877b000-b926-11e9-8bf3-74c4b90e6378.png" width="650">

---

MACAU analyzes raw read counts, not %, thus discerning between noise and DML. In practice, I'll need to have 2 count files - 1) total read counts, and 2) methylated read counts. I don't know wheter I have those files ready to go from Steven's processing - that's something to figure out. 


<img src="https://user-images.githubusercontent.com/17264765/62662134-d894bc00-b927-11e9-8376-b473fa52b80f.png" width="650">

<img src="https://user-images.githubusercontent.com/17264765/62662284-76888680-b928-11e9-8366-a8ef1a2020b9.png" width="650">


---

MACAU includes a relatedness term in the model, thus controlling for genetic inheritance of methylation patterns. In practice, I will need a relatedness matrix file. Katherine generated this for me from SNPs. To start, I will use a relatedness files she generated using SNPs from only the Hood Canal and South Sound oysters, located in the repo's [2bRAD directory](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/analyses/2bRAD/HCSSmbdsamples_rab.txt).


<img src="https://user-images.githubusercontent.com/17264765/62661839-23fa9a80-b927-11e9-81c4-ab51aa41dce7.png" width="650">

<img src="https://user-images.githubusercontent.com/17264765/62661859-3248b680-b927-11e9-8ed0-46ce63de2847.png" width="650">

<img src="https://user-images.githubusercontent.com/17264765/62662254-5658c780-b928-11e9-853b-5d29315053f0.png" width="650">


____ 

MACAU tests the null hypothesis that the predictor of interest has no effect on DNA methylation levels:H0: β = 0. The predictor could be a variety of things. In our case, the predictor is [shell size, wet weight](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/data/mbd_size.csv).  More than one variable can be inputed as a covariate matrix. 


<img src="https://user-images.githubusercontent.com/17264765/62662628-8785c780-b929-11e9-9fa0-da848146b07d.png" width="650">

<img src="https://user-images.githubusercontent.com/17264765/62662882-61145c00-b92a-11e9-8456-5f460bcd433d.png" width="650">


--- 

MACAU operates on a Linux computer. Sam installed the binaries on a few computers (see this [github issue #722](https://github.com/RobertsLab/resources/issues/722#issuecomment-518752977) (note: not the R package).  Once I have all the input files I should be able to run MACAU fairly easily, since the command is simple, we'll see! 

`./macau -g [filename] -t [filename] -p [filename] -k [filename] -bmm -o [prefix]`

<img src="https://user-images.githubusercontent.com/17264765/62663575-ebf65600-b92c-11e9-9c12-9b19fdfdb3ce.png" width="650">
