---
layout: post
title: Checking RNA quality with Bioanalyzer
---

I used the [Bioanalyzer RNA 6000 pico assay](https://www.agilent.com/en/product/automated-electrophoresis/bioanalyzer-systems/bioanalyzer-rna-kits-reagents/bioanalyzer-high-sensitivity-rna-analysis-228255) to check quality of a few RNA samples. RNA samples analyzed: 

The following are samples I am using to both check RNA quality, and to test the QuantSeq library prep protocol. I am testing samples from a variety of cohorts, treatments, tissue type. The RNA concentrations also vary a bit. 

| Cohort        | Treatment  | Tissue   | RNA Sample # | [RNA] after Dnase treatment (ng/uL) | ng RNA in 5uL (QuantSeq max vol) |
|---------------|------------|----------|--------------|-------------------------------------|----------------------------------|
| Dabob Bay     | 6 Low High | Ctenidia | 296          | 180.0                               | 900.0                            |
| Oyster Bay    | 6 Ambient  | Ctenidia | 323          | 102.0                               | 510.0                            |
| Fidalgo Bay   | 6 Ambient  | Ctenidia | 341          | 89.6                                | 448.0                            |
| Dabob Bay     | 10 Ambient | Larvae   | 403          | 136.0                               | 680                              |
| Fidalgo Bay   | 6 Low      | Larvae   | 472b         | 97.0                                | 485                              |
| Oyster Bay C1 | 10 Ambient | Larvae   | 490          | 186.0                               | 930                              |
| Oyster Bay C1 | 6 Ambient  | Larvae   | 513          | 142.0                               | 710                              |
| Oyster Bay C2 | 10 Ambient | Larvae   | 531          | 95.4                                | 477                              |

I pulled 10uL from the samples into newly labeled tubes for all future testing of protocol, quality, etc. 

Following the [RNA 600 Pico Chip manual](https://www.agilent.com/cs/library/usermanuals/public/G2938-90046_RNA600Pico_KG_EN.pdf), and with help from Sam, I ran these 8 samples on the Bioanalyzer. Results file is located in my [quantseq github repo](https://github.com/fish546-2018/laura-quantseq/blob/master/data/2100%20expert_Eukaryote%20Total%20RNA%20Pico_DE72902486_2019-08-20_14-14-19.xad). I consulted with Sam, and it looks like my RNA is not degraded. According to Sam, oyster 28S ribosomal RNA isn't usually present in Bionanalyzer results, which precludes using RIN as quality scores since it's calculated based on the 18S/28S ratio. So, I am basing my QA on Sam's visual assessment of the gels and peaks. Conclusion: RNA quality looks fine. 

---
#### From the manual, what a high quality sample should produce: 

![2019-08-21_pico-manual-expected-results.png](https://github.com/fish546-2018/laura-quantseq/blob/master/notebooks/screen-shots/2019-08-21_pico-manual-expected-results.PNG?raw=true)

#### My electropherogram results: 

![2019-08-21_pico-electropherogram.PNG](https://github.com/fish546-2018/laura-quantseq/blob/master/notebooks/screen-shots/2019-08-21_pico-electropherogram.PNG?raw=true)

#### Same results, visualized as bands on a gel  

![screen-shots/2019-08-21_pico-gel.PNG](https://github.com/fish546-2018/laura-quantseq/blob/master/notebooks/screen-shots/2019-08-21_pico-gel.PNG?raw=true)
