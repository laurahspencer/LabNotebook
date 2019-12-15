---
layout: post
title: QuantSeq Purification and qPCR Assay Batches 1 & 2
---

### ds cDNA purification, pre-PCR 
I purified the ds cDNA from batches 1 and 2. Notes on how to improve that process:  
- Bubbles! Bubbles make it difficult to use a mutichannel pipette. Need to improve handling to minimize bubble formation.   
- Should reduce amount of time I keep samples on magnet and to dry. The beads seem to crack a bit, and it's difficult to resuspend them in the final elution step.   
- Max # of PCR plate columns for purification step = 8   

### qPCR assay for optimal endpoint PCR cycles  
I performed the qPCR assay on all the ctenidia samples (batches 1 and 2). Here are the amplification curves: 

![image](https://user-images.githubusercontent.com/17264765/70856182-bf7ab980-1e8b-11ea-8f1c-c5e2c315f76e.png)

### Batch 1 end-point PCF cycle calculations 

Sample   No. | 50% max | No. Cycles @ 50% | No. Cycles @ 50%   minus 3 cycles | Cycles, round   down
-- | -- | -- | -- | --
328 | 1,662 | 16.51 | 13.51 | 13
299 | 1,603 | 18.54 | 15.54 | 15
301 | 1,572 | 18.66 | 15.66 | 15
342 | 1,673 | 18.85 | 15.85 | 15
331 | 1,572 | 18.94 | 15.94 | 16
307 | 1,001 | 28.77 | 25.77 | 25
295 | 1,498 | 21.86 | 18.86 | 18
qPCR NTC | 1,352 | 29.1 | 26.1 | 26
304 | 1,392 | 18.05 | 15.05 | 15
305 | 1,334 | 19.41 | 16.41 | 16
311 | 1,449 | 19.09 | 16.09 | 16
NTC2 - B1 | 1,521 | 25.92 | 22.92 | 23
298 | 1,437 | 18.27 | 15.27 | 15
348 | 1,394 | 19.33 | 16.33 | 16
315 | 1,496 | 18.74 | 15.74 | 15
qPCR NTC | 1,483 | 28.23 | 25.23 | 25
344 | 1,545 | 18.19 | 15.19 | 15
325 | 2,029 | 17.64 | 14.64 | 14
338 | 1,572 | 19.31 | 16.31 | 16
347 | 1,547 | 17.9 | 14.9 | 15
312 | 1,510 | 18.77 | 15.77 | 15
321 | 1,561 | 18.03 | 15.03 | 15
333 | 1,651 | 18.07 | 15.07 | 15
291 | 1,265 | 18.06 | 15.06 | 15
308 | 1,297 | 19.1 | 16.1 | 16
NTC1 - B1 | 1,538 | 25.23 | 22.23 | 22
335 | 1,449 | 18.86 | 15.86 | 15
318 | 1,586 | 19.01 | 16.01 | 16
294 | 1,444 | 19.3 | 16.3 | 16
324 | 1,648 | 19.34 | 16.34 | 16

### Batch 2 end-point PCR cycle calculations 

Sample   No. | RFU @ endpoint | 50% max | No. Cycles @ 50% | No. Cycles @ 50%   minus 3 cycles | Cycles, round   down
-- | -- | -- | -- | -- | --
343 | 3583 | 1791.5 | 16.62 | 13.62 | 13
345 | 4129 | 2,065 | 17.31 | 14.31 | 14
303 | 3543 | 1,772 | 17.28 | 14.28 | 14
346 | 2823 | 1,412 | 17.75 | 14.75 | 14
302 | 3153 | 1,577 | 16.81 | 13.81 | 13
336 | 3300 | 1,650 | 22.82 | 19.82 | 19
292 | 3253 | 1,627 | 18.6 | 15.6 | 15
NTC1 -B2 | 3079 | 1,540 | 20.67 | 17.67 | 17
317 | 3576 | 1,788 | 18.8 | 15.8 | 15
322 | 2540 | 1,270 | 19.31 | 16.31 | 16
332 | 2689 | 1,345 | 20.02 | 17.02 | 17
334 | 2851 | 1,426 | 20.78 | 17.78 | 17
349 | 2825 | 1,413 | 18.57 | 15.57 | 15
337 | 3325 | 1,663 | 18.65 | 15.65 | 15
341 | 3152 | 1,576 | 17.61 | 14.61 | 14
313 | 3184 | 1,592 | 18.47 | 15.47 | 15
309 | 2962 | 1,481 | 18.05 | 15.05 | 14
327 | 2973 | 1,487 | 18.89 | 15.89 | 15
319 | 3037 | 1,519 | 20.88 | 17.88 | 16
326 | 2810 | 1,405 | 18.12 | 15.12 | 15
306 | 3698 | 1,849 | 21.19 | 18.19 | 18
323 | 3537 | 1,769 | 18.47 | 15.47 | 15
314 | 2996 | 1,498 | 20.58 | 17.58 | 17
316 | 2929 | 1,465 | 19.02 | 16.02 | 16
339 | 2761 | 1,381 | 19.33 | 16.33 | 16
293 | 2213 | 1,107 | 19.67 | 16.67 | 16
329 | 2164 | 1,082 | 18.72 | 15.72 | 15
296 | 2182 | 1,091 | 18.1 | 15.1 | 15

### Notes 
- The highest quality RNA samples require 12 cycles.  According to my QuantSeq rep, the very lowest quality and concentration RNA samples require 25 cycles.  
- I should use the optimal cycle number for end-point PCR, BUT some people that are in a hurry run all samples using the average of three consecutive cycles. For instance, if some samples need 14, some need 15, and some need 16, one can run all samples for 15 cycles. However, that is not the optimal protocol. I will proceed, but may need to re-generate a few libraries, which I will determine using Qubut and high sensitivity DNA chip on the Bioanalyzer.  
