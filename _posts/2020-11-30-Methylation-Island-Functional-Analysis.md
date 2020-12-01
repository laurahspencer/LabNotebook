---
layout: post
title: Methylation Island Functional Analysis 
--- 

In last week's Epigenetic Reading Group we discussed this [Long et al. 2013](https://doi.org/10.7554/eLife.00348.002) paper, which found some interesting associations between the size of a non-methylated island (NMI) across 7 vertebrate taxa, and the function of genes within islands of various sizes. They found that "[conserved] NMIs tended to be longer, have higher CpG density, and are generally found associated with the TSSs of protein coding genes.", and "In contrast, tissue-specific NMIs that are differentially methylated tended to be shorter, have lower CpG density and are found away from protein-coding gene TSSs."  

To investigate any association between _methylated island_ size and function in the Olympia oyster (_Ostrea lurida_), I identified methylated islands using the MBD-Seq data, extracted long and short islands (>1000bp, <1000bp), identified annotated genes that overlap with the methyation islands, then identified enriched biological and molecular functions in short and long methylated islands. 

In this first round analysis I chose to assign "long" methylation islands as those >1000 bp, and "short" as those <1000bp (and in this case, greater than 500bp).  This threshold was based on the size distribution of unique (shorter) and shared (longer) non-methylated islands in the vertebrates that Long et al. found (see screen shot below). Obviously, the size distribution of non-methylated islands in vertebrates could be _very_ different from methylated islands in invertebrates. But, one has to start somewhere! 

![image](https://user-images.githubusercontent.com/17264765/100695886-10a6e600-3347-11eb-9dde-c93669112842.png)


I performed my analysis in the RMarkdown notebook [01b-General-Methylation-Patterns.Rmd](https://github.com/sr320/paper-oly-mbdbs-gen/blob/master/code/01b-General-Methylation-Patterns.Rmd). I'm pasting my code and results here: 

---

## Methylation islands functional analysis 

Here I will find methylation islands, measure their length, then bin them into short islands & long islands, identify genes that overlap with the islands, then see whether short/long islands are enriched for certain biological functions or molecular processes. I will use [Yaamini's script](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb), and the script from [Jeong et al. 2018 code](https://github.com/soojinyilab/Methylation-Islands/raw/master/methyl_island_sliding_window.pl), which I saved in the resources/ subdirectory of this repo. See also the [Jeong et al. paper](https://doi.org/10.1093/gbe/evy203), and [this GitHub issue](https://github.com/RobertsLab/resources/issues/834) where Yaamini & Steven discuss the optimal settings to use when running the script. 

Usage of the script:
  - ./methyl_island_sliding_window.pl <window size> <mCpG fraction> <step size> <sorted mCpG File>  
  - window size - starting size of the methylation island window. _I will use 500 bp, as per Yaamini & Steven's analysis._  
  - mCpG fraction - the minimum fraction of methylated CpGs required within the window to be accepted. _I will use 0.02, aka 2%_  
  - step size - base pairs to extend an accepted window by (continues extending by the step size as long as the mCpG fraction is met). _I will use 50 bp_  
  - mCpG File - input file with a list of all methylated CpGs in the genome, sorted by scaffold/chromosome and position 

### Create input file - methylated loci 
File needs to have chromosome and locus (aka start bp) 
```{bash}
awk '{print $1"\t"$2}' ../analyses/methylation-characteristics/all_methylated_5x.bed > ../analyses/methylation-characteristics/methylation-islands/all_methylated_5x-reduced.bed
head ../analyses/methylation-characteristics/methylation-islands/all_methylated_5x-reduced.bed
```
```
Contig0	161
Contig0	349
Contig0	481
Contig0	514
Contig0	532
Contig0	583
Contig0	742
Contig0	775
Contig0	782
Contig0	844
```
### Run Methylation Island Analysis 

```{bash}
../analyses/methylation-characteristics/methylation-islands/methyl_island_sliding_window.pl 500 0.02 50 \ ../analyses/methylation-characteristics/methylation-islands/all_methylated_5x-reduced.bed > \
../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50.tab
```

#### Resulting file structure and number of methylation islands 
```{bash}
# chr, star, end, number mCpG
head ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50.tab
# Number of methylation islands
wc -l ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50.tab
```
```
Contig0	481	1471	21
Contig0	8123	8407	10
Contig0	13826	14435	13
Contig0	35182	35743	16
Contig0	61419	61752	10
Contig0	64123	64431	14
Contig0	65414	65584	10
Contig0	66900	67377	11
Contig0	67564	67974	18
Contig0	69160	69640	11
   48455 ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50.tab
```

#### Filter by MI length (>500 bp) and include MI length in a new column
```{bash}
awk '{if ($3-$2 >= 500) { print $1"\t"$2"\t"$3"\t"$4"\t"$3-$2}}' ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50.tab \
> ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab

#preview resulting file, and count how many MI remain 
head ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab
wc -l ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab
```

#### Count max & min mCpG in an island
```{bash}
awk 'NR==1{max = $4 + 0; next} {if ($4 > max) max = $4;} END {print max}' \
../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab
awk 'NR==1{min = $4 + 0; next} {if ($4 < min) min = $4;} END {print min}' \
../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab
```
```
720
11
```

#### Find average length of islands 
```{bash}
awk '{ total += $2 } END { print total/NR }' ../analyses/methylation-characteristics/methylation-islands/methylation-islands_500-02-50_filtered.tab
```
```
8983.82
```

#### Inspect results of Methylation Island analysis 
```{r}
meth.islands <- read_delim(file=here::here("analyses", "methylation-characteristics", "methylation-islands", "methylation-islands_500-02-50_filtered.tab"), delim = "\t", col_names=FALSE) %>% 
  setNames(c("contig_island", "start_island", "end_island", "n_mCpG_island", "length_island"))

# Save ALL methylated islands to .bed file (for gene enrichment analysis)
write_delim(meth.islands, here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-all.bed"), delim = '\t', col_names = F)

# Check out length distribution 
hist(log(meth.islands$length_island))
```
![image](https://user-images.githubusercontent.com/17264765/100694426-edc70280-3343-11eb-8510-f78f0e92425e.png)

#### See distribution of LONG methylated islands, count and save as .bed 
```{r}
hist(subset(meth.islands, length_island>1000)$length_island, breaks = 200)
nrow(subset(meth.islands, length_island>1000))
write_delim(subset(meth.islands, length_island>1000), here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-long.bed"), delim = '\t', col_names = F)
```
```
7379
```
![image](https://user-images.githubusercontent.com/17264765/100694398-db4cc900-3343-11eb-8bf8-7920ce474983.png)

#### See distribution of SHORT methylated islands, count and save as .bed 
```{r}
hist(subset(meth.islands, length_island<1000)$length_island, breaks = 200)
nrow(subset(meth.islands, length_island<1000))
write_delim(subset(meth.islands, length_island<1000), here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-short.bed"), delim = '\t', col_names = F)
```
![image](https://user-images.githubusercontent.com/17264765/100694483-0e8f5800-3344-11eb-9a03-c0df4b00cd70.png)
```
19142
```
#### Find genes located within methylated islands 
```{bash}
# LONG methylated islands (MI)
bedtools intersect -wb -a "../analyses/methylation-characteristics/methylation-islands/meth-island-long.bed" -b "../genome-features/Olurida_v081-20190709.gene.2kbslop.gff" >  ../analyses/methylation-characteristics/methylation-islands/meth-island-long-genes.bed

# SHORT methylated islands (MI)
bedtools intersect -wb -a "../analyses/methylation-characteristics/methylation-islands/meth-island-short.bed" -b "../genome-features/Olurida_v081-20190709.gene.2kbslop.gff" >  ../analyses/methylation-characteristics/methylation-islands/meth-island-short-genes.bed

# ALL methylated islands (MI)
bedtools intersect -wb -a "../analyses/methylation-characteristics/methylation-islands/meth-island-all.bed" -b "../genome-features/Olurida_v081-20190709.gene.2kbslop.gff" >  ../analyses/methylation-characteristics/methylation-islands/meth-island-all-genes.bed
```

#### Read in genes within methylated island, split "Notes" column and extract Uniprot ID 
```{r}
# Copy Uniprot SPID of genes overlapping with LONG meth islands 
read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-long-genes.bed"), delim = "\t", col_names = FALSE) %>% 
  setNames(c(colnames(meth.islands), 
             "contig_gene", "source", "feature", "start_gene", "end_gene", 
             "unknown1", "strand", "unknown2", "notes_gene")) %>% 
  mutate(SPID=str_extract(notes_gene, "SPID=(.*?);")) %>% 
  mutate(SPID=str_remove(SPID, "SPID=")) %>% mutate(SPID=str_remove(SPID, ";")) %>% 
  select(SPID) %>%   na.omit() %>% as.vector() %>% write_clip()

# Copy Uniprot SPID of genes overlapping with SHORT meth islands 
read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-short-genes.bed"), delim = "\t", col_names = FALSE) %>% select(X14) %>%
  mutate(SPID=str_extract(X14, "SPID=(.*?);")) %>% 
  mutate(SPID=str_remove(SPID, "SPID=")) %>% mutate(SPID=str_remove(SPID, ";")) %>% 
  select(SPID) %>%   na.omit() %>% as.vector() %>% write_clip()

# Copy Uniprot SPID of genes overlapping with ALL meth islands (my background list)
read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-all-genes.bed"), delim = "\t", col_names = FALSE) %>% select(X14) %>%
  mutate(SPID=str_extract(X14, "SPID=(.*?);")) %>% 
  mutate(SPID=str_remove(SPID, "SPID=")) %>% mutate(SPID=str_remove(SPID, ";")) %>% 
  select(SPID) %>%   na.omit() %>% as.vector() %>% write_clip()

# Question: many genes contain multiple "short" methylation islands. Do I include those genes multiple times in enrichment analysis? Or do I only include them once? 
```

####  Merge enriched GO terms with GO Slims 
```{r}
# Read in the GO Slim table  
GOSlim <- read_delim(here::here("resources", "GO-GOslim.sorted"), delim = '\t', col_names = FALSE) %>% 
  setNames(c("GO", "term", "slim", "category")) %>% 
  mutate_at(vars(category, slim), as.factor)

GO.enriched.MI.short <- read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-short-enriched-BF.txt"), delim = '\t', col_names = TRUE) %>% 
   separate(Term, into=c("GO", "term"), remove=TRUE,sep = "~") %>% 
   left_join(GOSlim, by=c("GO", "term")) #add slim 

GO.enriched.MI.long <- read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-long-enriched-BF.txt"), delim = '\t', col_names = TRUE) %>% 
   separate(Term, into=c("GO", "term"), remove=TRUE,sep = "~") %>% 
   left_join(GOSlim, by=c("GO", "term")) #add slim 
```

### Enriched Biological Functions in LONG methylation islands (>1000 bp)

| Term                                                                                        | Count | %    | PValue | List Total | Pop Hits | FDR                  |
|---------------------------------------------------------------------------------------------|-------|------|--------|------------|----------|----------------------|
| GO:0007156~homophilic cell adhesion via plasma membrane adhesion molecules                  | 34    | 1.5  | 4.3    | 1961       | 40       | 0.019897411819458567 |
| GO:0006351~transcription, DNA-templated                                                     | 252   | 11.1 | 0.002  | 1961       | 469      | 1.0                  |
| GO:0007155~cell adhesion                                                                    | 60    | 2.7  | 0.005  | 1961       | 97       | 1.0                  |
| GO:0007186~G-protein coupled receptor signaling pathway                                     | 33    | 1.4  | 0.005  | 1961       | 48       | 1.0                  |
| GO:0007507~heart development                                                                | 41    | 1.8  | 0.01   | 1961       | 64       | 1.0                  |
| GO:0007169~transmembrane receptor protein tyrosine kinase signaling pathway                 | 18    | 0.8  | 0.02   | 1961       | 24       | 1.0                  |
| GO:0050852~T cell receptor signaling pathway                                                | 10    | 0.4  | 0.02   | 1961       | 11       | 1.0                  |
| GO:0006355~regulation of transcription, DNA-templated                                       | 206   | 9.1  | 0.02   | 1961       | 393      | 1.0                  |
| GO:0045944~positive regulation of transcription from RNA polymerase II promoter             | 106   | 4.7  | 0.03   | 1961       | 193      | 1.0                  |
| GO:0008203~cholesterol metabolic process                                                    | 17    | 0.8  | 0.03   | 1961       | 23       | 1.0                  |
| GO:0008104~protein localization                                                             | 20    | 0.9  | 0.04   | 1961       | 29       | 1.0                  |
| GO:0006357~regulation of transcription from RNA polymerase II promoter                      | 47    | 2.1  | 0.04   | 1961       | 80       | 1.0                  |
| GO:0006397~mRNA processing                                                                  | 49    | 2.2  | 0.04   | 1961       | 84       | 1.0                  |
| GO:0034220~ion transmembrane transport                                                      | 17    | 0.8  | 0.04   | 1961       | 24       | 1.0                  |
| GO:0060071~Wnt signaling pathway, planar cell polarity pathway                              | 7     | 0.3  | 0.05   | 1961       | 7        | 1.0                  |
| GO:0007528~neuromuscular junction development                                               | 14    | 0.6  | 0.05   | 1961       | 19       | 1.0                  |
| GO:0018108~peptidyl-tyrosine phosphorylation                                                | 20    | 0.9  | 0.06   | 1961       | 30       | 1.0                  |
| GO:0006511~ubiquitin-dependent protein catabolic process                                    | 39    | 1.7  | 0.06   | 1961       | 66       | 1.0                  |
| GO:0006366~transcription from RNA polymerase II promoter                                    | 39    | 1.7  | 0.06   | 1961       | 66       | 1.0                  |
| GO:0042127~regulation of cell proliferation                                                 | 22    | 1.0  | 0.06   | 1961       | 34       | 1.0                  |
| GO:0043966~histone H3 acetylation                                                           | 8     | 0.4  | 0.07   | 1961       | 9        | 1.0                  |
| GO:0050982~detection of mechanical stimulus                                                 | 8     | 0.4  | 0.07   | 1961       | 9        | 1.0                  |
| GO:0072661~protein targeting to plasma membrane                                             | 8     | 0.4  | 0.07   | 1961       | 9        | 1.0                  |
| GO:0008355~olfactory learning                                                               | 8     | 0.4  | 0.07   | 1961       | 9        | 1.0                  |
| GO:0007411~axon guidance                                                                    | 34    | 1.5  | 0.07   | 1961       | 57       | 1.0                  |
| GO:0045893~positive regulation of transcription, DNA-templated                              | 59    | 2.6  | 0.07   | 1961       | 106      | 1.0                  |
| GO:0007275~multicellular organism development                                               | 94    | 4.2  | 0.08   | 1961       | 176      | 1.0                  |
| GO:0007257~activation of JUN kinase activity                                                | 9     | 0.4  | 0.08   | 1961       | 11       | 1.0                  |
| GO:0030501~positive regulation of bone mineralization                                       | 9     | 0.4  | 0.08   | 1961       | 11       | 1.0                  |
| GO:0046426~negative regulation of JAK-STAT cascade                                          | 6     | 0.3  | 0.09   | 1961       | 6        | 1.0                  |
| GO:0044331~cell-cell adhesion mediated by cadherin                                          | 6     | 0.3  | 0.09   | 1961       | 6        | 1.0                  |
| GO:0070593~dendrite self-avoidance                                                          | 6     | 0.3  | 0.09   | 1961       | 6        | 1.0                  |
| GO:0060021~palate development                                                               | 14    | 0.6  | 0.09   | 1961       | 20       | 1.0                  |
| GO:0000398~mRNA splicing, via spliceosome                                                   | 33    | 1.5  | 0.09   | 1961       | 56       | 1.0                  |
| GO:0070588~calcium ion transmembrane transport                                              | 23    | 1.0  | 0.09   | 1961       | 37       | 1.0                  |
| GO:0016339~calcium-dependent cell-cell adhesion via plasma membrane cell adhesion molecules | 10    | 0.4  | 0.1    | 1961       | 13       | 1.0                  |
| GO:0007157~heterophilic cell-cell adhesion via plasma membrane cell adhesion molecules      | 10    | 0.4  | 0.1    | 1961       | 13       | 1.0                  |
| GO:0021591~ventricular system development                                                   | 10    | 0.4  | 0.1    | 1961       | 13       | 1.0                  |


### Enriched Biological Functions in SHORT methylation islands (<1000 bp)

| Term                                                 | Count | %                  | PValue               | List Total | Pop Hits | FDR |
|------------------------------------------------------|-------|--------------------|----------------------|------------|----------|-----|
| GO:0042493~response to drug                          | 55    | 1.423027166882277  | 0.022317643564457215 | 3363       | 59       | 1.0 |
| GO:0006974~cellular response to DNA damage stimulus  | 105   | 2.716688227684347  | 0.027144714536471236 | 3363       | 118      | 1.0 |
| GO:0007018~microtubule-based movement                | 39    | 1.0090556274256144 | 0.08008950477964477  | 3363       | 42       | 1.0 |
| GO:0051726~regulation of cell cycle                  | 39    | 1.0090556274256144 | 0.08008950477964477  | 3363       | 42       | 1.0 |
| GO:0006979~response to oxidative stress              | 33    | 0.8538163001293662 | 0.08106833809744202  | 3363       | 35       | 1.0 |
| GO:0007264~small GTPase mediated signal transduction | 60    | 1.5523932729624839 | 0.09132361327223226  | 3363       | 67       | 1.0 |
| GO:0006260~DNA replication                           | 65    | 1.6817593790426906 | 0.09411699904720267  | 3363       | 73       | 1.0 |

### Enrichment Analysis using GO MWU 

#### Make input files for GO_MWU 

Save loci IDS and GO TERMS for All GENES that were identified that overlap with methylated islands (including +/- 2kb) 

```{r}
read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-all-genes.bed"), delim = "\t", col_names = FALSE) %>% 
  setNames(c(colnames(meth.islands), 
             "contig_gene", "source", "feature", "start_gene", "end_gene", 
             "unknown1", "strand", "unknown2", "notes_gene")) %>% 
  mutate(island_bps=paste(contig_island, start_island, end_island, sep="_")) %>%
  mutate(GO=str_extract(notes_gene, "Ontology_term=(.*?);")) %>% 
mutate(GO = str_replace(GO, pattern="Ontology_term=",replacement = "")) %>%
  mutate(GO = str_replace(GO, pattern=";",replacement = "")) %>%  
  mutate(GO = str_replace_all(GO, pattern=",",replacement = ";")) %>%
  select(island_bps, GO) %>% drop_na(GO) %>%
  write.table(here::here("analyses", "GO_MWU", "GO_MWU_GO-terms_meth-islands"),sep="\t",quote = F,row.names = F, col.names=F)
```

### Save loci IDS and SIGNIFICANCE (0=significant, 1=not significant) for SHORT methylated islands 

```{r}
# Create a dataframe of "contig_bps" for all genes that overlap with methylation islands (aka the background list of genes), and add a column to indicate significance. Start by adding "1" to all.  
go.mwu.islands.all <- read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-all-genes.bed"), delim = "\t", col_names = FALSE) %>% 
  setNames(c(colnames(meth.islands), 
             "contig_gene", "source", "feature", "start_gene", "end_gene", 
             "unknown1", "strand", "unknown2", "notes_gene")) %>% 
  mutate(island_bps=paste(contig_island, start_island, end_island, sep="_")) %>% 
  select(island_bps) %>% add_column(sig = c(1)) 

# Generate vector of genes that overlap with long methylated islands, then short islands 
long <- read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-long-genes.bed"), delim = "\t", col_names = FALSE) %>% 
  mutate(island_bps=paste(X1, X2, X3, sep="_")) %>% select(island_bps) %>% unlist() %>% as.vector()
short <- read_delim(here::here("analyses", "methylation-characteristics", "methylation-islands", "meth-island-short-genes.bed"), delim = "\t", col_names = FALSE) %>% 
  mutate(island_bps=paste(X1, X2, X3, sep="_")) %>% select(island_bps) %>% unlist() %>% as.vector()

# Replace 0's in the significance column for any "contig_bps" that was a LONG meth island, then with a SHORT one 
go.mwu.islands.long <- go.mwu.islands.all
go.mwu.islands.long[which(go.mwu.islands.long$island_bps %in% long),]$sig <- 0

go.mwu.islands.short <- go.mwu.islands.all
go.mwu.islands.short[which(go.mwu.islands.short$island_bps %in% short),]$sig <- 0

# Save significance files for GO MWU 
write.csv(go.mwu.islands.long, here::here("analyses", "GO_MWU", "GO_MWU_signif_meth-islands-long"),quote = F,row.names = F)
write.csv(go.mwu.islands.short, here::here("analyses", "GO_MWU", "GO_MWU_signif_meth-islands-short"),quote = F,row.names = F)
```

### To run GO MWU analysis, open the R file "GO_MWU-islands.R" and follow prompt. 

### RESULTS of GO MWU analysis: 1 significant GO term in the SHORT methylation islands = **cell adhesion (p-adjusted=0.001192251)** 

### Re-do enrichment analysis with revised bp threshold (2000bp) for long vs. short methylation islands 

Out of curiosity, I re-did the entire enrichment analysis with LONG methylation islands >2000bp, and SHORT islands <2000 bp.  No functions were enriched according to GO MWU. Here are the DAVID results: 

#### LONG islands (>2kb)  
![image](https://user-images.githubusercontent.com/17264765/100696628-d0486780-3348-11eb-9223-8eb7621a5030.png)

#### SHORT islands (<2kb, but >500bp)  
No enriched biological functions. 
