---
layout: post
title: Measuring QuantSeq larvae 
---

The QuantSeq run I am prepping for will look at gene expression in _O. lurida_ larvae, which were produced by adults that had previously been held in varying winter temperature and pCO2.  All larvae were collected and frozen within a day or two of being released from the brood chamber, therefore they should all be at the same developmental stage. The size upon release, however, could be slightly different depending on the larval growth rate, and if larval release is triggered by something (e.g. food, tank cleaning).  Since larval size could correspond with developmental stage which impacts gene expression profiles, I measured all the larvae that will also be sequenced.  

While homogenizing frozen larvae with mortar + pestle, I preserved some of the larvae in ethanol, held in a fridge. To measure, I used the Nikon camera + microscope in Jackie's lab, and the NIS-Element software's automatic measurement capabilities. Steps: 

- Suspend larvae in ethanol with a pipette, transfer to a slide with cover slip  
- Capture image of larvae that are lying on their side, so the full width/length could be measured 
- Apply a binary layer to the image using the pre-determined threshold setting (based on color). The threshold also uses circularity and size to determine where the binary boundaries occur, and what to include. 
- Use the automatic measurement option to draw perimeters around all binary objects, then generate measurement statistics about each object. Here are details of the measurements  I pulled for each object, from the [NIS-Element user manual](http://www.mvi-inc.com/wp-content/uploads/NIS_4.00_D_User_Guide.pdf). Note: I believe the MaxFeret is the best representation of shell width, and MaxFeret90 is shell height.  

![image](https://user-images.githubusercontent.com/17264765/63718872-74676880-c800-11e9-896a-11854c334f54.png)
![image](https://user-images.githubusercontent.com/17264765/63718648-e4292380-c7ff-11e9-903c-96a5ee4e0e47.png)
![image](https://user-images.githubusercontent.com/17264765/63718724-120e6800-c800-11e9-8b22-ce8922b2e7e9.png)
![image](https://user-images.githubusercontent.com/17264765/63718769-336f5400-c800-11e9-8faf-750a1e7c9013.png)
![image](https://user-images.githubusercontent.com/17264765/63718811-47b35100-c800-11e9-86d0-094fe072d6a6.png)
![image](https://user-images.githubusercontent.com/17264765/63718834-5ac62100-c800-11e9-87a8-b0e3930632c5.png)
![image](https://user-images.githubusercontent.com/17264765/63718943-a8428e00-c800-11e9-9707-ee5bbe326ffd.png)
![image](https://user-images.githubusercontent.com/17264765/63718921-952fbe00-c800-11e9-9d8a-5b78bb24bf80.png)

When measuring objects, it's important to do quality control - some larvae should not be included since they are at a weird angle, or tissue/junk is included in the auto-generated object, thus making the measurements erroneous. After doing this QC, I exported 2 images simultanously: 1) microscope image without any annotations, layers, etc.; 2) binary layer showing objects that were measured, object IDs, and scale bar. It took me a long time to figure out the software, which I don't think is very intuitive. I made a screen recording of the process - check it out: 

![screen recording link here]()

I measured at minimum 50 larvae from each of my 58 larval sample. Here are some quick and dirty plots of MaxFeret90 (aka shell height) and MaxFeret (aka shell width): 

![image](https://user-images.githubusercontent.com/17264765/63728085-50fbe800-c817-11e9-8325-3c1e9fc50ee2.png)

![image](https://user-images.githubusercontent.com/17264765/63728113-66711200-c817-11e9-9e55-7815390fc2c7.png)

![image](https://user-images.githubusercontent.com/17264765/63728170-915b6600-c817-11e9-941c-73cebfa5fc15.png)



I need to measure the following larvae - missed these since I already had RNA: 

| Date Collected | Spawning Group | Population    | Treatment | Larval sample # | RNA Sample # | Date homogenized |
|----------------|----------------|---------------|-----------|-----------------|--------------|------------------|
| 5/21/17        | SN-10 Low B    | Oyster Bay C1 | 10 Low    | 06-A            | 41           | na               |
| 5/23/17        | SN-10 Low B    | Oyster Bay C1 | 10 Low    | 08-A            | 43           | na               |
| 5/26/17        | SN-10 Low B    | Oyster Bay C1 | 10 Low    | 24-A            | 46           | na               |
| 5/27/17        | SN-10 Low B    | Oyster Bay C1 | 10 Low    | 26-A            | 47           | na               |
| 5/31/17        | SN-10 Low A    | Oyster Bay C1 | 10 Low    | 32-A            | 44           | na               |
| 6/14/17        | SN-10 Low B    | Oyster Bay C1 | 10 Low    | 62-A            | 506          | 07/20/19         |
| 6/24/17        | SN-10 Low A    | Oyster Bay C1 | 10 Low    | 79-A            | 45           | na               |
| 5/23/17        | SN-6 Ambient B | Oyster Bay C1 | 6 Ambient | 10-A            | 35           | na               |
| 6/5/17         | SN-6 Ambient B | Oyster Bay C1 | 6 Ambient | 45-A            | 513          | 07/23/19         |
| 6/6/17         | SN-6 Ambient B | Oyster Bay C1 | 6 Ambient | 48-A            | 39           | na               |
| 6/15/17        | SN-6 Ambient B | Oyster Bay C1 | 6 Ambient | 69-A            | 37           | na               |
| 6/19/17        | SN-6 Ambient A | Oyster Bay C1 | 6 Ambient | 77-A            | 34           | na               |

