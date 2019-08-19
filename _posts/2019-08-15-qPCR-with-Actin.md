---
layout: post
title: qPCR with Actin primer set, etc.  
---

### Preparing previously sequenced larval samples 

I spoke with Katherine last week about combining QuantSeq data from the previous run and the upcoming run. She advised (and I agreed) that it's optimal for me to re-run those samples. The kits have changed slightly, in addition to me not DNasing the samples previously, her prepping the libraries etc.  I have RNA in the -80, so I'm going to catch them up to the other samples today by Turbo DNasing them, and including in the qPCR re-do. Samples are: 34, 35, 37, 39, 41, 44, 45, 47.


### DNA contamination re-check with Actin primer set 

Today we received a new set of Actin primers that Sam had previously used to check for gDNA contamination. I tested them on one of my larval DNA samples, and voila! My positive control amplified, and the curves appear as expected.  

![image](https://user-images.githubusercontent.com/17264765/63123568-1fa13380-bf5e-11e9-9900-e6f617d59558.png)

![image](https://user-images.githubusercontent.com/17264765/63123616-43fd1000-bf5e-11e9-8682-b92578d38b84.png)

Rather than re-doing ALL samples, I did a sub-set, as I'd like to avoid more thawing/freezing cycles.  Oddly, the + control didn't amplify again with this new primer set. 

![image](https://user-images.githubusercontent.com/17264765/63289042-01e01100-c273-11e9-963c-4717f28561a8.png)

![image](https://user-images.githubusercontent.com/17264765/63289114-2d62fb80-c273-11e9-853c-225f3741a2fc.png)

I discussed the issue with Steven and Sam. Since I did in fact DNase all my samples, I shall move ahead with the RNA concenrating and quality checking.

### Quantifying remaining RNA samples after DNasing 

In addition to re-running qPCR on 42 samples, I also measured [RNA] for samples that I had not yet quantified due to running out of Qubit tubes.  The samples that I did qPCR on were mostly those that also needed quantifying (to minimize freeze/thaw), however I also did some other samples to include a random assortment.  

### Next step: concentrate dilute samples with Zymo Cleaner/Concentrator 

The QuantSeq prep starts with up to 500 ng RNA in 5uL (or less). Katherine advised that I should use no less than 200 or 300 ng.  So, here is how much RNA I have in 5 uL: 

| Tissue   | Cohort              | Treatment (pCO2, Temp) | RNA Tube # | [RNA] after Dnase treatment (ng/uL) | ng RNA in 0.5 uL | Concentrate? |
|----------|---------------------|------------------------|------------|-------------------------------------|------------------|--------------|
| Ctenidia | Dabob Bay           | High                   | 291        | 158.0                               | 790.0            |              |
| Ctenidia | Dabob Bay           | High                   | 292        | 29.6                                | 148.0            | X            |
| Ctenidia | Dabob Bay           | High                   | 293        | 39.6                                | 198.0            | X            |
| Ctenidia | Dabob Bay           | High                   | 294        | 110.0                               | 550.0            |              |
| Ctenidia | Dabob Bay           | High                   | 295        | 34.8                                | 174.0            | X            |
| Ctenidia | Dabob Bay           | High                   | 296        | 180.0                               | 900.0            |              |
| Ctenidia | Dabob Bay           | High                   | 298        | 182.0                               | 910.0            |              |
| Ctenidia | Dabob Bay           | High                   | 299        | 50.4                                | 252.0            | X            |
| Ctenidia | Dabob Bay           | Ambient                | 301        | 75.8                                | 379.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 302        | 62.4                                | 312.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 306        | 136.0                               | 680.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 304        | 200.0                               | 1,000.0          |              |
| Ctenidia | Dabob Bay           | Ambient                | 305        | 75.2                                | 376.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 303        | 95.2                                | 476.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 307        | 89.4                                | 447.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 308        | 73.6                                | 368.0            |              |
| Ctenidia | Dabob Bay           | Ambient                | 309        | 170.0                               | 850.0            |              |
| Ctenidia | Oyster Bay          | High                   | 311        | 158.0                               | 790.0            |              |
| Ctenidia | Oyster Bay          | High                   | 312        | 90.6                                | 453.0            |              |
| Ctenidia | Oyster Bay          | High                   | 313        | 72.4                                | 362.0            |              |
| Ctenidia | Oyster Bay          | High                   | 314        | 42.2                                | 211.0            | X            |
| Ctenidia | Oyster Bay          | High                   | 315        | 148.0                               | 740.0            |              |
| Ctenidia | Oyster Bay          | High                   | 316        | 146.0                               | 730.0            |              |
| Ctenidia | Oyster Bay          | High                   | 317        | 158.0                               | 790.0            |              |
| Ctenidia | Oyster Bay          | High                   | 318        | 174.0                               | 870.0            |              |
| Ctenidia | Oyster Bay          | High                   | 319        | 77.6                                | 388.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 321        | 148.0                               | 740.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 322        | 44.6                                | 223.0            | X            |
| Ctenidia | Oyster Bay          | Ambient                | 323        | 102.0                               | 510.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 324        | 172.0                               | 860.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 325        | 180.0                               | 900.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 326        | 130.0                               | 650.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 327        | 85.2                                | 426.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 328        | 156.0                               | 780.0            |              |
| Ctenidia | Oyster Bay          | Ambient                | 329        | 162.0                               | 810.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 331        | 42.2                                | 211.0            | X            |
| Ctenidia | Fidalgo Bay         | High                   | 332        | 65.8                                | 329.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 333        | 78.6                                | 393.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 334        | 64.8                                | 324.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 335        | 180.0                               | 900.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 336        | 94.8                                | 474.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 337        | 194.0                               | 970.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 338        | 81.6                                | 408.0            |              |
| Ctenidia | Fidalgo Bay         | High                   | 339        | 77.2                                | 386.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 341        | 89.6                                | 448.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 342        | 162.0                               | 810.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 343        | 114.0                               | 570.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 344        | 25.0                                | 125.0            | X            |
| Ctenidia | Fidalgo Bay         | Ambient                | 345        | 190.0                               | 950.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 346        | 43.6                                | 218.0            | X            |
| Ctenidia | Fidalgo Bay         | Ambient                | 347        | 69.0                                | 345.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 348        | 54.4                                | 272.0            |              |
| Ctenidia | Fidalgo Bay         | Ambient                | 349        | 82.0                                | 410.0            |              |
| Larvae   | Dabob Bay           | 10 Ambient             | 401        | 93.4                                | 467              |              |
| Larvae   | Dabob Bay           | 10 Ambient             | 402        | 114.0                               | 570              |              |
| Larvae   | Dabob Bay           | 10 Ambient             | 403        | 136.0                               | 680              |              |
| Larvae   | Dabob Bay           | 10 Ambient             | 404        | 112.0                               | 560              |              |
| Larvae   | Dabob Bay           | 10 High                | 411        | 72.6                                | 363              |              |
| Larvae   | Dabob Bay           | 10 High                | 412        | 31.2                                | 156              | X            |
| Larvae   | Dabob Bay           | 10 High                | 413        | 130.0                               | 650              |              |
| Larvae   | Dabob Bay           | 10 High                | 414        | 168.0                               | 840              |              |
| Larvae   | Dabob Bay           | 6 Ambient              | 421        | 57.6                                | 288              |              |
| Larvae   | Dabob Bay           | 6 High                 | 431b       | 83.0                                | 415              |              |
| Larvae   | Dabob Bay           | 6 High                 | 432        | 74.0                                | 370              |              |
| Larvae   | Dabob Bay           | 6 High                 | 434        | 63.0                                | 315              |              |
| Larvae   | Fidalgo Bay         | 10 Ambient             | 441        | 16.2                                | 81               | X            |
| Larvae   | Fidalgo Bay         | 10 Ambient             | 442b       | 69.8                                | 349              |              |
| Larvae   | Fidalgo Bay         | 10 Ambient             | 443        | 60.2                                | 301              |              |
| Larvae   | Fidalgo Bay         | 10 Ambient             | 444        | 70.6                                | 353              |              |
| Larvae   | Fidalgo Bay         | 10 Ambient             | 445        | 160.0                               | 800              |              |
| Larvae   | Fidalgo Bay         | 10 High                | 451        | 68.4                                | 342              |              |
| Larvae   | Fidalgo Bay         | 10 High                | 452b       | 97.2                                | 486              |              |
| Larvae   | Fidalgo Bay         | 10 High                | 453        | 196.0                               | 980              |              |
| Larvae   | Fidalgo Bay         | 6 Ambient              | 461b       | 84.0                                | 420              |              |
| Larvae   | Fidalgo Bay         | 6 Ambient              | 462b       | 106.0                               | 530              |              |
| Larvae   | Fidalgo Bay         | 6 High                 | 471b       | 108.0                               | 540              |              |
| Larvae   | Fidalgo Bay         | 6 High                 | 472b       | 97.0                                | 485              |              |
| Larvae   | Fidalgo Bay         | 6 High                 | 473        | 124.0                               | 620              |              |
| Larvae   | Fidalgo Bay         | 6 High                 | 474        | 77.2                                | 386              |              |
| Larvae   | Fidalgo Bay         | 6 High                 | 475        | 27.4                                | 137              | X            |
| Larvae   | Fidalgo Bay         | 6 High                 | 476        | 39.2                                | 196              | X            |
| Larvae   | Fidalgo Bay         | 6 High                 | 477        | 164.0                               | 820              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 481        | 89.2                                | 446              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 482        | 22.2                                | 111              | X            |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 483        | 99.0                                | 495              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 484        | 58.4                                | 292              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 485        | 19.1                                | 96               | X            |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 486b       | 148.0                               | 740              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 487        | 118.0                               | 590              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 488        | 60.0                                | 300              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 489        | 68.0                                | 340              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 490        | 186.0                               | 930              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 491        | 58.4                                | 292              |              |
| Larvae   | Oyster Bay C1       | 10 Ambient             | 492        | 82.6                                | 413              |              |
| Larvae   | Oyster Bay C1       | 10 High                | 41         | 108.0                               | 540              |              |
| Larvae   | Oyster Bay C1       | 10 High                | 43         | 156.0                               | 780              |              |
| Larvae   | Oyster Bay C1       | 10 High                | 46         | 98.6                                | 493              |              |
| Larvae   | Oyster Bay C1       | 10 High                | 47         | 128.0                               | 640              |              |
| Larvae   | Oyster Bay C1       | 10 High                | 44         | 41.4                                | 207              | X            |
| Larvae   | Oyster Bay C1       | 10 High                | 506        | 29.2                                | 146              | X            |
| Larvae   | Oyster Bay C1       | 10 High                | 45         | 25.1                                | 126              | X            |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 35         | 94.4                                | 472              |              |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 38         | LOW                                 | #VALUE!          |              |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 513        | 142.0                               | 710              |              |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 39         | 89.8                                | 449              |              |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 37         | 74.6                                | 373              |              |
| Larvae   | Oyster Bay C1       | 6 Ambient              | 34         | 104.0                               | 520              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 521        | 66.6                                | 333              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 522        | 32.2                                | 161              | X            |
| Larvae   | Oyster Bay C1       | 6 High                 | 523        | 71.4                                | 357              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 524        | 63.0                                | 315              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 525        | 140.0                               | 700              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 526        | 138.0                               | 690              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 527        | 124.0                               | 620              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 528        | 87.6                                | 438              |              |
| Larvae   | Oyster Bay C1       | 6 High                 | 529        | 56.8                                | 284              |              |
| Larvae   | Oyster Bay C2       | 10 Ambient             | 531        | 95.4                                | 477              |              |
| Larvae   | Oyster Bay C2       | 10 Ambient             | 532        | 93.6                                | 468              |              |
| Larvae   | Oyster Bay C2       | 10 Ambient             | 533        | 6.5                                 | 33               | X            |
| Larvae   | Oyster Bay C2       | 10 High                | 541        | 44.4                                | 222              | X            |
| Larvae   | Oyster Bay C2       | 10 High                | 542        | 32.8                                | 164              | X            |
| Larvae   | Oyster Bay C2       | 10 High                | 543        | 162.0                               | 810              |              |
| Larvae   | Oyster Bay C2       | 6 Ambient              | 551        | 96.4                                | 482              |              |
| Larvae   | Oyster Bay C2       | 6 Ambient              | 552b       | 74.6                                | 373              |              |
| Larvae   | Oyster Bay C2       | 6 Ambient              | 553        | 186.0                               | 930              |              |
| Larvae   | Oyster Bay C2       | 6 Ambient              | 554        | 188.0                               | 940              |              |
| Larvae   | Oyster Bay C2       | 6 High                 | 561        | 28.0                                | 140              | X            |
| Larvae   | Oyster Bay C2       | 6 High                 | 562        | 126.0                               | 630              |              |
| Larvae   | Oyster Bay C2       | 6 High                 | 563        | 47.0                                | 235              | X            |
| Larvae   | Oyster Bay C2       | 6 High                 | 564        | 152.0                               | 760              |              |
| Larvae   | Oyster Bay C2       | 6 High                 | 565        | 31.2                                | 156              | X            |
| Control  | homog. Control      | RNA Control            | 571        | LOW                                 | LOW              |              |
| Control  | homog. Control      | RNA Control            | 572        |                                     |                  |              |
| Control  | RNAzol control      | RNA Control            | 574        |                                     |                  |              |
| Control  | Turbo Dnase control | RNA Control            | 575        |                                     |                  |              |
